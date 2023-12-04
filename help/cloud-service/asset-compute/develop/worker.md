---
title: Een Asset compute-worker ontwikkelen
description: De arbeiders van de asset compute zijn de kern van een Asset compute projecten zoals verstrekt douanefunctionaliteit die, of orkest, het werk uitvoert dat op activa wordt uitgevoerd om een nieuwe vertoning tot stand te brengen.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 652
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 0%

---

# Een Asset compute-worker ontwikkelen

De arbeiders van de asset compute zijn de kern van een project van de Asset compute zoals verstrekt douanefunctionaliteit die, of orkest, het werk uitvoert dat op activa wordt uitgevoerd om een nieuwe vertoning tot stand te brengen.

In het project Asset compute wordt automatisch een eenvoudige worker gegenereerd die het oorspronkelijke binaire getal van het element kopieert naar een benoemde uitvoering, zonder transformaties. In deze zelfstudie zullen we deze worker aanpassen om een interessantere uitvoering te maken, om de kracht van Asset compute workers te illustreren.

We maken een Asset compute-worker die een nieuwe horizontale afbeeldingsuitvoering genereert die lege ruimte links en rechts van de uitvoering van het element dekt met een vervaagde versie van het element. De breedte, de hoogte en de vervaging van de uiteindelijke uitvoering worden geparametereerd.

## Logische stroom van een aanroep van een Asset compute-worker

Asset compute workers implementeren het Asset compute SDK worker API-contract in de `renditionCallback(...)` functie, die conceptueel is:

+ __Invoer:__ De oorspronkelijke binaire en verwerkingsprofielparameters van een AEM element
+ __Uitvoer:__ Een of meer uitvoeringen die aan het AEM-element moeten worden toegevoegd

![Logische stroom van asset compute worker](./assets/worker/logical-flow.png)

1. AEM de Auteursdienst roept de Asset compute worker aan, die de activa verstrekt __(1 bis)__ origineel binair (`source` parameter), en __(1 ter)__ alle parameters die zijn gedefinieerd in het verwerkingsprofiel (`rendition.instructions` parameter).
1. De Asset compute SDK organiseert de uitvoering van de metagegevens van de aangepaste Asset compute `renditionCallback(...)` functie, die een nieuwe binaire vertoning produceert, die op het originele binaire getal van het element wordt gebaseerd __(1 bis)__ en alle parameters __(1 ter)__.

   + In dit leerprogramma wordt de vertoning &quot;in proces&quot;gecreeerd, betekenend de worker de vertoning samenstelt, nochtans kan het bronbinaire getal naar andere dienst APIs van het Web voor vertoningsgeneratie eveneens worden verzonden.

1. De Asset compute-worker slaat de binaire gegevens van de nieuwe uitvoering op `rendition.path`.
1. De binaire gegevens waarnaar wordt geschreven `rendition.path` wordt via de Asset compute SDK naar AEM Auteur Service vervoerd en wordt blootgesteld als __(4 bis)__ een tekstuitvoering en __(4 ter)__ Doorgegaan naar het metagegevensknooppunt van het element.

In het bovenstaande diagram worden de problemen beschreven die de Asset compute ontwikkelaar onder ogen ziet en wordt een logische stroom weergegeven naar de aanroep van de Asset compute worker. Voor de nieuwsgierige [interne gegevens over de uitvoering van de Asset compute](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) zijn beschikbaar, maar alleen van de SDK API-contracten voor openbare Asset computen kan worden afgeweken.

## Anatomie van een worker

Alle Asset compute werknemers volgen dezelfde basisstructuur en input/output contract.

```javascript
'use strict';

// Any npm module imports used by the worker
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

/**
Exports the worker implemented by a custom rendition callback function, which parametrizes the input/output contract for the worker.
 + `source` represents the asset's original binary used as the input for the worker.
 + `rendition` represents the worker's output, which is the creation of a new asset rendition.
 + `params` are optional parameters, which map to additional key/value pairs, including a sub `auth` object that contains Adobe I/O access credentials.
**/
exports.main = worker(async (source, rendition, params) => {
    // Perform any necessary source (input) checks
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        // Throw appropriate errors whenever an erring condition is met
        throw new SourceCorruptError('source file is empty');
    }

    // Access any custom parameters provided via the Processing Profile configuration
    let param1 = rendition.instructions.exampleParam;

    /** 
    Perform all work needed to transform the source into the rendition.
    
    The source data can be accessed:
        + In the worker via a file available at `source.path`
        + Or via a presigned GET URL at `source.url`
    **/
    if (success) {
        // A successful worker must write some data back to `renditions.path`. 
        // This example performs a trivial 1:1 copy of the source binary to the rendition
        await fs.copyFile(source.path, rendition.path);
    } else {
        // Upon failure an Asset Compute Error (exported by @adobe/asset-compute-commons) should be thrown.
        throw new GenericError("An error occurred!", "example-worker");
    }
});

/**
Optionally create helper classes or functions the worker's rendition callback function invokes to help organize code.

Code shared across workers, or to complex to be managed in a single file, can be broken out across supporting JavaScript files in the project and imported normally into the worker. 
**/
function customHelperFunctions() { ... }
```

## De worker index.js openen

![Automatisch gegenereerde index.js](./assets/worker/autogenerated-index-js.png)

1. Zorg ervoor dat het Asset compute-project is geopend in VS-code
1. Ga naar de `/actions/worker` map
1. Open de `index.js` file

Dit is het JavaScript-bestand van de worker dat we in deze zelfstudie zullen wijzigen.

## Ondersteunende npm-modules installeren en importeren

Omdat op Node.js-Gebaseerd, profiteren de projecten van de Asset compute van robuuste [ecosysteem van de npm-module](https://npmjs.com). Om npm modules te gebruiken moeten wij hen eerst in ons project van de Asset compute installeren.

In deze worker gebruiken we de [jimp](https://www.npmjs.com/package/jimp) om de vertoningsafbeelding rechtstreeks in de code Node.js te maken en te bewerken.

>[!WARNING]
>
>Niet alle npm-modules voor het manipuleren van elementen worden ondersteund door Asset compute. npm-modules die afhankelijk zijn van het bestaan van toepassingen zoals ImageMagick of andere OS-afhankelijke bibliotheken, worden niet ondersteund. U kunt het gebruik van alleen JavaScript-npm-modules het beste beperken.

1. Open de bevellijn in de wortel van uw Asset compute project (dit kan in de Code van VS via worden gedaan __Terminal > Nieuwe terminal__) en voer de opdracht uit:

   ```
   $ npm install jimp
   ```

1. Het dialoogvenster Importeren `jimp` in de code van de worker zodat deze via de `Jimp` JavaScript-object.
Werk de `require` richtlijnen boven aan de `index.js` om de `Jimp` van het object `jimp` module:

   ```javascript
   'use strict';
   
   const Jimp = require('jimp');
   const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
   const fs = require('fs').promises;
   
   exports.main = worker(async (source, rendition, params) => {
       // Check handle a corrupt input source
       const stats = await fs.stat(source.path);
       if (stats.size === 0) {
           throw new SourceCorruptError('source file is empty');
       }
   
       // Do work here
   });
   ```

## Parameters lezen

De arbeiders van de asset compute kunnen in parameters lezen die binnen via Profielen van de Verwerking kunnen worden overgegaan die in AEM as a Cloud Service dienst van de Auteur worden bepaald. De parameters worden via de `rendition.instructions` object.

Deze kunnen worden gelezen door toegang te krijgen tot `rendition.instructions.<parameterName>` in de code van de worker.

Hier lezen we in de configureerbare uitvoering `SIZE`, `BRIGHTNESS` en `CONTRAST`, met standaardwaarden als er geen waarden zijn opgegeven via het verwerkingsprofiel. Let op: `renditions.instructions` worden doorgegeven als tekenreeksen wanneer deze worden aangeroepen vanuit AEM as a Cloud Service verwerkingsprofielen, zodat deze worden omgezet in de juiste gegevenstypen in de arbeiderscode.

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    // Processing Profiles pass in instructions as Strings, so make sure to parse to correct data types
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    // Do work here
}
```

## Fouten genereren{#errors}

Workers van asset computen kunnen situaties tegenkomen die tot fouten leiden. De Adobe Asset compute SDK biedt [een reeks vooraf gedefinieerde fouten](https://github.com/adobe/asset-compute-commons#asset-compute-errors) die kunnen worden gegenereerd wanneer dergelijke situaties zich voordoen. Als er geen specifiek fouttype van toepassing is, `GenericError` kan worden gebruikt, of specifieke `ClientErrors` kan worden gedefinieerd.

Voordat u begint met het verwerken van de uitvoering, moet u controleren of alle parameters geldig zijn en worden ondersteund in de context van deze worker:

+ Zorg ervoor dat de parameters van de vertoningsinstructie voor `SIZE`, `CONTRAST`, en `BRIGHTNESS` zijn geldig. Zo niet, dan genereert u een aangepaste fout `RenditionInstructionsError`.
   + Een aangepaste `RenditionInstructionsError` klasse die is uitgebreid `ClientError` wordt onder aan dit bestand gedefinieerd. Het gebruik van een specifieke aangepaste fout is handig wanneer [schriftelijke tests](../test-debug/test.md) voor de werknemer.

```javascript
'use strict';

const Jimp = require('jimp');
// Import the Asset Compute SDK provided `ClientError` 
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read in parameters and set defaults if parameters are provided
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        // Ensure size is within allowable bounds
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        // Ensure contrast is valid value
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Do work here
}

// Create a new ClientError to handle invalid rendition.instructions values
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        // Provide a:
        // + message: describing the nature of this erring condition
        // + name: the name of the error; usually same as class name
        // + reason: a short, searchable, unique error token that identifies this error
        super(message, "RenditionInstructionsError", "rendition_instructions_error");

        // Capture the strack trace
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## De vertoning maken

Wanneer de parameters worden gelezen, ontsmet en gevalideerd, wordt code geschreven om de uitvoering te genereren. De pseudo-code voor het genereren van de vertoning ziet er als volgt uit:

1. Een nieuwe `renditionImage` canvas met vierkante afmetingen, opgegeven via `size` parameter.
1. Een `image` object van binair getal van bronelement
1. Gebruik de __Jimp__ bibliotheek voor het transformeren van de afbeelding:
   + De oorspronkelijke afbeelding uitsnijden naar een gecentreerd vierkant
   + Een cirkel knippen vanuit het midden van de &quot;vierkante&quot; afbeelding
   + Schalen zodat deze passen binnen de afmetingen die zijn gedefinieerd in het dialoogvenster `SIZE` parameterwaarde
   + Contrast aanpassen op basis van de `CONTRAST` parameterwaarde
   + De helderheid aanpassen op basis van de `BRIGHTNESS` parameterwaarde
1. De getransformeerde locatie plaatsen `image` in het midden van de `renditionImage` met een transparante achtergrond
1. Schrijf de samengestelde, `renditionImage` tot `rendition.path` zodat het terug in AEM kan worden bewaard als activa worden uitgevoerd.

In deze code worden de [Jimp API&#39;s](https://github.com/oliver-moran/jimp#jimp) om deze afbeeldingstransformaties uit te voeren.

De werknemers van de asset compute moeten hun werk synchroon beëindigen, en `rendition.path` moet volledig worden teruggeschreven naar `renditionCallback` voltooid. Dit vereist dat de asynchrone functievraag synchroon wordt gemaakt gebruikend `await` operator. Als u niet bekend bent met asynchrone functies in JavaScript en hoe u deze synchroon kunt laten uitvoeren, moet u zich vertrouwd maken met [JavaScript-operator voor wachten](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

De voltooide worker `index.js` zou als moeten kijken:

```javascript
'use strict';

const Jimp = require('jimp');
const { worker, SourceCorruptError, ClientError } = require('@adobe/asset-compute-sdk');
const fs = require('fs').promises;

exports.main = worker(async (source, rendition, params) => {
    const stats = await fs.stat(source.path);
    if (stats.size === 0) {
        throw new SourceCorruptError('source file is empty');
    }

    // Read/parse and validate parameters
    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 1,0000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image 
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness
    image.crop(
            image.bitmap.width < image.bitmap.height ? 0 : (image.bitmap.width - image.bitmap.height) / 2,
            image.bitmap.width < image.bitmap.height ? (image.bitmap.height - image.bitmap.width) / 2 : 0,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height,
            image.bitmap.width < image.bitmap.height ? image.bitmap.width : image.bitmap.height
        )   
        .circle()
        .scaleToFit(SIZE, SIZE)
        .contrast(CONTRAST)
        .brightness(BRIGHTNESS);

    // Place the transformed image onto the transparent renditionImage to save as PNG
    renditionImage.composite(image, 0, 0)

    // Write the final transformed image to the asset's rendition
    await renditionImage.writeAsync(rendition.path);
});

// Custom error used for renditions.instructions parameter checking
class RenditionInstructionsError extends ClientError {
    constructor(message) {
        super(message, "RenditionInstructionsError", "rendition_instructions_error");
        Error.captureStackTrace(this, RenditionInstructionsError);
    }
}
```

## De worker uitvoeren

Nu de arbeiderscode volledig is, en eerder geregistreerd en gevormd in [manifest.yml](./manifest.md), kan het worden uitgevoerd gebruikend het lokale Hulpmiddel van de Ontwikkeling van de Asset compute om de resultaten te zien.

1. Van de wortel van het project van de Asset compute
1. Uitvoeren `aio app run`
1. Wacht tot Asset compute Development Tool in een nieuw venster wordt geopend
1. In de __Een bestand selecteren...__ een voorbeeldafbeelding selecteren die u wilt verwerken
   + Selecteer een voorbeeldafbeeldingsbestand dat u wilt gebruiken als binair bronelement
   + Tik op de knop __(+)__ aan de linkerkant, en upload een [voorbeeldafbeelding](../assets/samples/sample-file.jpg) bestand, en vernieuw het browservenster Ontwikkelingsgereedschappen
1. Bijwerken `"name": "rendition.png"` als deze worker een transparante PNG genereert.
   + Merk op dat deze &quot;naam&quot;parameter slechts voor het Hulpmiddel van de Ontwikkeling wordt gebruikt, en niet op zou moeten baseren.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png"
           }
       ]
   }
   ```

1. Tikken __Uitvoeren__ en wacht tot de vertoning is gegenereerd
1. De __Uitvoeringen__ geeft een voorvertoning weer van de gegenereerde vertoning. Tik op de voorvertoning van de vertoning om de volledige vertoning te downloaden

   ![Standaard PNG-uitvoering](./assets/worker/default-rendition.png)

### De worker uitvoeren met parameters

De parameters, die via de configuraties van het Profiel van de Verwerking worden overgegaan, kunnen in de Hulpmiddelen van de Ontwikkeling van de Asset compute worden gesimuleerd door hen als sleutel/waardeparen op de vertoningsparameter JSON te verstrekken.

>[!WARNING]
>
>Tijdens lokale ontwikkeling, kunnen de waarden binnen worden overgegaan gebruikend diverse gegevenstypes, wanneer overgegaan van AEM als de Profielen van de Verwerking van de Cloud Service als koorden, zodat ervoor zorgen de correcte gegevenstypes indien nodig worden geparseerd.
> Jimp&#39;s `crop(width, height)` functie vereist dat de parameters ervan `int`s. Indien `parseInt(rendition.instructions.size)` wordt niet geparseerd aan int., dan de vraag aan `jimp.crop(SIZE, SIZE)` mislukt omdat de parameters niet compatibel zijn met het type String.

Onze code accepteert parameters voor:

+ `size` Hiermee definieert u de grootte van de vertoning (hoogte en breedte als gehele getallen)
+ `contrast` definieert het contrast aanpassen, moet liggen tussen -1 en 1, als floats
+ `brightness`  definieert de lichte aanpassing, moet liggen tussen -1 en 1, als floats

Deze worden gelezen in de worker `index.js` via:

+ `const SIZE = parseInt(rendition.instructions.size) || 800`
+ `const CONTRAST = parseFloat(rendition.instructions.contrast) || 0`
+ `const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0`

1. Werk de renderingsparameters bij om de grootte, het contrast en de helderheid aan te passen.

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.png",
               "size": "450",
               "contrast": "0.30",
               "brightness": "0.15"
           }
       ]
   }
   ```

1. Tikken __Uitvoeren__ opnieuw
1. Tik op de voorvertoning van de vertoning om de gegenereerde vertoning te downloaden en te bekijken. Let op de afmetingen en hoe het contrast en de helderheid zijn gewijzigd ten opzichte van de standaardweergave.

   ![Parameterized PNG rendition](./assets/worker/parameterized-rendition.png)

1. Andere afbeeldingen uploaden naar de __Bronbestand__ vervolgkeuzelijst en probeer de worker met andere parameters tegen hen uit te voeren!

## Worker index.js op Github

De definitieve `index.js` is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Problemen oplossen

+ [Gedeeltelijk getekende/beschadigde vertoning geretourneerd](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
