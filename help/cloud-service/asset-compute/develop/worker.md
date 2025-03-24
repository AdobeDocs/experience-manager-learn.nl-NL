---
title: Een Asset Compute-worker ontwikkelen
description: Asset Compute-workers vormen de kern van een Asset Compute-project en bieden aangepaste functionaliteit die het werk dat met een element wordt uitgevoerd uitvoert of georkestreerd om een nieuwe uitvoering te maken.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6282
thumbnail: KT-6282.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 7d51ec77-c785-4b89-b717-ff9060d8bda7
duration: 451
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1386'
ht-degree: 0%

---

# Een Asset Compute-worker ontwikkelen

Asset Compute-workers vormen de kern van een Asset Compute-project en bieden aangepaste functionaliteit waarmee het werk dat met een element wordt uitgevoerd, wordt uitgevoerd of georkestreerd om een nieuwe uitvoering te maken.

Het Asset Compute-project genereert automatisch een eenvoudige worker die het oorspronkelijke binaire getal van het element kopieert naar een benoemde uitvoering, zonder transformaties. In deze zelfstudie zullen we deze worker aanpassen om een interessantere uitvoering te maken, om de kracht van Asset Compute-workers te illustreren.

We maken een Asset Compute-worker die een nieuwe horizontale afbeeldingsuitvoering genereert die lege ruimte links en rechts van de uitvoering van het element dekt met een vervaagde versie van het element. De breedte, de hoogte en de vervaging van de uiteindelijke uitvoering worden geparametereerd.

## Logische stroom van een aanroep van een Asset Compute-worker

Asset Compute Workers implementeren het Asset Compute SDK worker API-contract in de functie `renditionCallback(...)` , wat conceptueel is:

+ __Input:__ de originele binaire parameters en van het Profiel van een AEM-element
+ __Output:__ Één of meerdere vertoningen die aan de activa van AEM moeten worden toegevoegd

![ de arbeids logische stroom van Asset Compute ](./assets/worker/logical-flow.png)

1. De dienst van de Auteur van AEM roept de worker van Asset Compute aan, die de activa __(1a) verstrekt__ origineel binair (`source` parameter), en __(1b)__ om het even welke parameters die in het Profiel van de Verwerking (`rendition.instructions` parameter) worden bepaald.
1. Asset Compute SDK organiseert de uitvoering van de functie van de meta-gegevensarbeider van douaneAsset Compute `renditionCallback(...)`, die een nieuwe binaire vertoning produceert, op het originele binaire element van activa __(1a)__ en om het even welke parameters __(1b)__ wordt gebaseerd.

   + In dit leerprogramma wordt de vertoning &quot;in proces&quot;gecreeerd, betekenend de worker de vertoning samenstelt, nochtans kan het bronbinaire getal naar andere dienst APIs van het Web voor vertoningsgeneratie eveneens worden verzonden.

1. De Asset Compute-worker slaat de binaire gegevens van de nieuwe vertoning op in `rendition.path` .
1. De binaire gegevens die aan `rendition.path` worden geschreven worden vervoerd via Asset Compute SDK naar de Dienst van de Auteur van AEM en als __(4a)__ een tekstvertoning en __(4b)__ voortgeduurd aan de de meta-gegevensknoop van de activa worden blootgesteld die.

In het bovenstaande diagram worden de zorgen van de Asset Compute-ontwikkelaar en de logische doorstroming naar de aanroeping van Asset Compute-workers uitgelegd. Voor nieuwsgierig, zijn de [ interne details van de uitvoering van Asset Compute ](https://experienceleague.adobe.com/docs/asset-compute/using/extend/custom-application-internals.html) beschikbaar, nochtans slechts kunnen de openbare contracten van Asset Compute SDK API van afhangen.

## Anatomie van een worker

Alle Asset Compute-workers volgen dezelfde basisstructuur en invoer-/uitvoercontract.

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

![ auto-geproduceerde index.js ](./assets/worker/autogenerated-index-js.png)

1. Zorg ervoor dat het Asset Compute-project is geopend in VS-code
1. Ga naar de map `/actions/worker`
1. Het `index.js` -bestand openen

Dit is het JavaScript-bestand van de worker dat we in deze zelfstudie zullen wijzigen.

## Ondersteunende npm-modules installeren en importeren

Als Gebaseerd op Node.js, profiteren de projecten van Asset Compute van het robuuste [ npm moduleecosysteem ](https://npmjs.com). Als u npm-modules wilt gebruiken, moeten we ze eerst in ons Asset Compute-project installeren.

In deze worker, hefboomwerking wij [ jimp ](https://www.npmjs.com/package/jimp) om het vertoningsbeeld in de code te creëren en direct te manipuleren Node.js.

>[!WARNING]
>
>Asset Compute biedt geen ondersteuning voor alle npm-modules voor het manipuleren van bedrijfsmiddelen. npm-modules die afhankelijk zijn van het bestaan van toepassingen zoals ImageMagick of andere OS-afhankelijke bibliotheken, worden niet ondersteund. Het is het beste om het gebruik van npm-modules die alleen voor JavaScript bestemd zijn, te beperken.

1. Open de bevellijn in de wortel van uw project van Asset Compute (dit kan in de Code van VS via __Terminal > Nieuwe Terminal__ worden gedaan) en voer het bevel uit:

   ```
   $ npm install jimp
   ```

1. Importeer de module `jimp` in de code van de worker, zodat deze kan worden gebruikt via het JavaScript-object `Jimp` .
Werk de `require` instructies boven aan de `index.js` -worker bij om het `Jimp` -object te importeren vanuit de `jimp` -module:

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

Asset Compute-workers kunnen parameters lezen die kunnen worden doorgegeven via de verwerkingsprofielen die zijn gedefinieerd in de AEM as a Cloud Service Author-service. De parameters worden via het `rendition.instructions` -object aan de worker doorgegeven.

Deze kunnen worden gelezen door `rendition.instructions.<parameterName>` in de arbeiderscode te openen.

Hier lezen we de standaardwaarden `SIZE`, `BRIGHTNESS` en `CONTRAST` van de configureerbare vertoning. Deze waarden geven de standaardwaarden aan als er geen waarden zijn opgegeven via het verwerkingsprofiel. `renditions.instructions` worden als tekenreeksen doorgegeven wanneer deze worden aangeroepen vanuit AEM as a Cloud Service-verwerkingsprofielen, zodat deze worden omgezet in de juiste gegevenstypen in de arbeiderscode.

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

Asset Compute-workers kunnen situaties tegenkomen die tot fouten leiden. Adobe Asset Compute SDK verstrekt [ een reeks van vooraf bepaalde fouten ](https://github.com/adobe/asset-compute-commons#asset-compute-errors) die kunnen worden geworpen wanneer dergelijke situaties worden ontmoet. Als er geen specifiek fouttype van toepassing is, kan `GenericError` worden gebruikt of kan specifieke aangepaste `ClientErrors` worden gedefinieerd.

Voordat u begint met het verwerken van de uitvoering, moet u controleren of alle parameters geldig zijn en worden ondersteund in de context van deze worker:

+ Zorg ervoor dat de parameters voor de vertoningsinstructie voor `SIZE` , `CONTRAST` en `BRIGHTNESS` geldig zijn. Als dat niet het geval is, genereert u een aangepaste fout `RenditionInstructionsError` .
   + Een aangepaste `RenditionInstructionsError` -klasse die `ClientError` uitbreidt, wordt onder aan dit bestand gedefinieerd. Het gebruik van een specifieke, douanefout is nuttig wanneer [ het schrijven tests ](../test-debug/test.md) voor de worker.

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

1. Maak een nieuw `renditionImage` canvas met vierkante afmetingen die zijn opgegeven met de parameter `size` .
1. Een `image` -object maken op basis van de binaire waarde van het bronelement
1. Gebruik de __Jimp__ bibliotheek om het beeld om te zetten:
   + De oorspronkelijke afbeelding uitsnijden naar een gecentreerd vierkant
   + Een cirkel knippen vanuit het midden van de &quot;vierkante&quot; afbeelding
   + Schalen zodat deze binnen de afmetingen passen die worden gedefinieerd door de parameterwaarde `SIZE`
   + Contrast aanpassen op basis van de parameterwaarde `CONTRAST`
   + Helderheid aanpassen op basis van de parameterwaarde `BRIGHTNESS`
1. Plaats de getransformeerde `image` in het midden van de `renditionImage` met een transparante achtergrond
1. Schrijf de samengestelde `renditionImage` naar `rendition.path` , zodat deze weer in AEM kan worden opgeslagen als een elementuitvoering.

Deze code past [ Jimp APIs ](https://github.com/oliver-moran/jimp#jimp) aan om deze beeldtransformaties uit te voeren.

Asset Compute Workers moeten hun werk synchroon voltooien en `rendition.path` moet volledig zijn teruggeschreven naar voordat de `renditionCallback` van de worker is voltooid. Dit vereist dat asynchrone functieaanroepen synchroon worden gemaakt met de operator `await` . Als u niet vertrouwd met JavaScript asynchrone functies bent en hoe te om hen te hebben op een synchrone manier uitvoeren, vertrouwt u met [ JavaScript wachtende exploitant ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await).

De voltooide worker `index.js` moet er als volgt uitzien:

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

Nu de arbeiderscode volledig is, en eerder in [ manifest.yml ](./manifest.md) werd geregistreerd en gevormd, kan het worden uitgevoerd gebruikend het lokale Hulpmiddel van de Ontwikkeling van Asset Compute om de resultaten te zien.

1. Van de basis van het Asset Compute-project
1. Uitvoeren `aio app run`
1. Wacht tot Asset Compute Development Tool in een nieuw venster wordt geopend
1. In __selecteer een dossier...__ drop down, selecteer een steekproefbeeld om te verwerken
   + Selecteer een voorbeeldafbeeldingsbestand dat u wilt gebruiken als binair bronelement
   + Als niets nog bestaat, tik __(+)__ aan de linkerzijde, en upload a [ steekproefbeeld ](../assets/samples/sample-file.jpg) dossier, en vernieuw het browser venster van Hulpmiddelen van de Ontwikkeling
1. Werk `"name": "rendition.png"` bij als deze worker om een transparante PNG te genereren.
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

1. Tik __Looppas__ en wacht op de vertoning om te produceren
1. De __vertoningen__ sectie previews de geproduceerde vertoning. Tik op de voorvertoning van de vertoning om de volledige vertoning te downloaden

   ![ StandaardPNG vertoning ](./assets/worker/default-rendition.png)

### De worker uitvoeren met parameters

De parameters, die via de configuraties van het Profiel van de Verwerking worden overgegaan, kunnen in de Hulpmiddelen van de Ontwikkeling van Asset Compute worden gesimuleerd door hen als sleutel/waardeparen op de vertoningsparameter JSON te verstrekken.

>[!WARNING]
>
>Tijdens lokale ontwikkeling, kunnen de waarden binnen worden overgegaan gebruikend diverse gegevenstypes, wanneer overgegaan van AEM als de Profielen van de Verwerking van Cloud Service als koorden, zodat ervoor zorgen de correcte gegevenstypes indien nodig worden geparseerd.
> De functie `crop(width, height)` van Jimp vereist bijvoorbeeld dat de parameters van Jimp `int` &#39;s zijn. Wanneer `parseInt(rendition.instructions.size)` niet aan een int wordt geparseerd, mislukt de aanroep van `jimp.crop(SIZE, SIZE)` omdat de parameters het type String niet incompatibel zijn.

Onze code accepteert parameters voor:

+ `size` definieert de grootte van de vertoning (hoogte en breedte als gehele getallen)
+ `contrast` definieert het contrast aanpassen, moet liggen tussen -1 en 1, als floats
+ `brightness` definieert de lichte aanpassing, moet liggen tussen -1 en 1, als floats

Deze worden in de worker `index.js` gelezen via:

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

1. Tik __opnieuw Looppas__
1. Tik op de voorvertoning van de vertoning om de gegenereerde vertoning te downloaden en te bekijken. Let op de afmetingen en hoe het contrast en de helderheid zijn gewijzigd ten opzichte van de standaardweergave.

   ![ Geparametereerde vertoning PNG ](./assets/worker/parameterized-rendition.png)

1. Upload andere beelden aan het __dossier van Source__ dropdown, en probeer lopend de worker tegen hen met verschillende parameters!

## Worker index.js op Github

De laatste `index.js` is beschikbaar op Github op:

+ [ aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Problemen oplossen

+ [Gedeeltelijk getekende/beschadigde vertoning geretourneerd](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
