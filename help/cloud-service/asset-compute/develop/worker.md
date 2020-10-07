---
title: Een worker Asset Compute ontwikkelen
description: De arbeiders van de Compute van activa zijn de kern van projecten van de Compute van Activa zoals verstrekt douanefunctionaliteit die, of orchestraten, het werk uitvoert op activa om een nieuwe vertoning tot stand te brengen.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6282
thumbnail: KT-6282.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '1508'
ht-degree: 0%

---


# Een worker Asset Compute ontwikkelen

De arbeiders van de Compute van activa zijn de kern van een project van de Compute van Activa zoals verstrekt douanefunctionaliteit die, of orchestraten, het werk uitvoert op activa om een nieuwe vertoning tot stand te brengen.

Met het project Asset Compute wordt automatisch een eenvoudige worker gegenereerd die het oorspronkelijke binaire getal van het element kopieert naar een benoemde uitvoering, zonder transformaties. In deze zelfstudie zullen we deze worker aanpassen om een interessantere uitvoering te maken, om de kracht van Asset Compute-workers te illustreren.

We maken een worker Asset Compute die een nieuwe horizontale afbeeldingsuitvoering genereert die lege ruimte links en rechts van de uitvoering van het element dekt met een vervaagde versie van het element. De breedte, hoogte en vervaging van de uiteindelijke uitvoering worden geparametereerd.

## Logische stroom van de aanroeping van een worker Asset Compute

Workers voor Asset Compute implementeren het API-contract voor de worker van Asset Compute SDK in de `renditionCallback(...)` functie, die conceptueel is:

+ __Invoer:__ Het binaire element en de parameters van een AEM element
+ __Uitvoer:__ Een of meer uitvoeringen die aan het AEM-element moeten worden toegevoegd

![Logische stroom van de worker Asset Compute](./assets/worker/logical-flow.png)

1. Wanneer een worker Asset Compute wordt aangeroepen vanuit de AEM Auteur-service, is deze tegen een AEM middel via een verwerkingsprofiel. Het originele binaire binaire getal van het element __(1a)__ wordt doorgegeven aan de worker via de `source` parameter van de callback-functie van de renditie en __(1b)__ alle parameters die in het verwerkingsprofiel via een `rendition.instructions` parameterset zijn gedefinieerd.
1. De laag van SDK van de Compute van Activa keurt het verzoek van het verwerkingsprofiel goed, en organiseert de uitvoering van de `renditionCallback(...)` functie van de arbeider van de Compute van het douaneMiddel, die het bronbinaire getal in __(1a)__ omzet die op om het even welke die parameters wordt verstrekt door __(1b)__ wordt verstrekt om een vertoning van het bronbinaire getal te produceren.
   + In dit leerprogramma wordt de vertoning &quot;in proces&quot;gecreeerd, betekenend de worker de vertoning samenstelt, nochtans kan het bronbinaire getal naar andere dienst APIs van het Web voor vertoningsgeneratie eveneens worden verzonden.
1. De worker Asset Compute slaat de binaire representatie van de uitvoering op, `rendition.path` waarmee deze kan worden opgeslagen in de AEM-auteurservice.
1. Na voltooiing, `rendition.path` worden de binaire gegevens die aan worden geschreven vervoerd via de Asset Compute SDK en via de AEM Auteur Service als vertoning beschikbaar in AEM UI blootgesteld.

In het bovenstaande diagram worden de bekommernissen over het berekenen van bedrijfsmiddelen voor ontwikkelaars en de logische stroom naar de oproep van de worker Asset Compute (Asset Compute) verwoord. Voor de nieuwsgierigheid zijn de [interne details van de uitvoering](https://docs.adobe.com/content/help/en/asset-compute/using/extend/custom-application-internals.html) van Asset Compute beschikbaar, maar alleen de openbare contracten van SDK API voor Asset Compute moeten afhangen.

## Anatomie van een worker

Alle medewerkers van Asset Compute volgen dezelfde basisstructuur en invoer-/uitvoercontract.

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

1. Zorg ervoor dat het project Asset Compute is geopend in VS-code
1. Navigate to the `/actions/worker` folder
1. Open the `index.js` file

Dit is het JavaScript-bestand van de worker dat we in deze zelfstudie zullen wijzigen.

## Ondersteunende npm-modules installeren en importeren

Omdat op Node.js gebaseerd, de projecten van de Activa berekenen profiteren van het robuuste [npm modulecosysteem](https://npmjs.com). Om npm modules te hefboomwerking moeten wij hen eerst in ons project van de Compute van Activa installeren.

In deze worker gebruiken we de [jimp](https://www.npmjs.com/package/jimp) om de vertoningsafbeelding rechtstreeks in de code Node.js te maken en te bewerken.

>[!WARNING]
>
>Niet alle npm-modules voor het manipuleren van elementen worden ondersteund door Asset Compute. npm-modules die afhankelijk zijn van de bestaande toepassingen van andere toepassingen, zoals ImageMagick of OS-afhankelijke bibliotheken. U kunt het gebruik van alleen JavaScript-npm-modules het beste beperken.

1. Open de bevellijn in de wortel van uw Activa Compute project (dit kan in de Code van VS via __Terminal > Nieuwe Terminal__) worden gedaan en voer het bevel uit:

   ```
   $ npm install jimp
   ```

1. Importeer de `jimp` module in de code van de worker, zodat deze kan worden gebruikt via het `Jimp` JavaScript-object.
Werk de `require` instructies boven aan de worker bij `index.js` om het `Jimp` object uit de `jimp` module te importeren:

   ```javascript
   'use strict';
   
   const { Jimp } = require('jimp');
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

De arbeiders van de Compute van activa kunnen in parameters lezen die binnen via Profielen van de Verwerking kunnen worden overgegaan die in AEM als dienst van de Auteur van de Cloud Service worden bepaald. De parameters worden via het `rendition.instructions` object aan de worker doorgegeven.

Deze kunnen worden gelezen door `rendition.instructions.<parameterName>` in de arbeiderscode toegang te hebben.

Hier zullen wij in de configureerbare vertoning `SIZE`, `BRIGHTNESS` en `CONTRAST`, die standaardwaarden verstrekken als niets via het Profiel van de Verwerking is verstrekt. Merk op dat `renditions.instructions` worden overgegaan binnen als koorden wanneer aangehaald van AEM als Profielen van de Verwerking van de Cloud Service, zodat zij in de correcte gegevenstypes in de arbeiderscode worden omgezet.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

Workers van Asset Compute kunnen situaties tegenkomen die tot fouten leiden. De SDK van Asset Compute van Adobe biedt [een reeks vooraf gedefinieerde fouten](https://github.com/adobe/asset-compute-commons#asset-compute-errors) die kunnen worden gegenereerd wanneer dergelijke situaties zich voordoen. Als er geen specifiek fouttype van toepassing is, `GenericError` kan dit worden gebruikt of kan er een specifieke aangepaste `ClientErrors` definitie worden gebruikt.

Voordat u begint met het verwerken van de uitvoering, moet u controleren of alle parameters geldig zijn en worden ondersteund in de context van deze worker:

+ Zorg ervoor dat de parameters voor de vertoningsinstructie geldig zijn voor `SIZE`, `CONTRAST`en `BRIGHTNESS` geldig zijn. Als dat niet het geval is, genereert u een aangepaste fout `RenditionInstructionsError`.
   + Onder aan dit bestand wordt een aangepaste `RenditionInstructionsError` klasse `ClientError` gedefinieerd die een uitbreiding vormt. Het gebruik van een specifieke aangepaste fout is handig wanneer u tests [voor de worker](../test-debug/test.md) schrijft.

```javascript
'use strict';

const { Jimp } = require('jimp');
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

1. Maak een nieuw `renditionImage` canvas met vierkante afmetingen die via de `size` parameter zijn opgegeven.
1. Een `image` object maken op basis van de binaire waarde van het bronelement
1. Gebruik de __Jimp__ -bibliotheek om de afbeelding te transformeren:
   + De oorspronkelijke afbeelding uitsnijden naar een gecentreerd vierkant
   + Een cirkel knippen vanuit het midden van de &quot;vierkante&quot; afbeelding
   + Schalen zodat deze past binnen de afmetingen die zijn gedefinieerd door de `SIZE` parameterwaarde
   + Contrast aanpassen op basis van de `CONTRAST` parameterwaarde
   + Helderheid aanpassen op basis van de `BRIGHTNESS` parameterwaarde
1. Plaats de getransformeerde kleur `image` in het midden van de achtergrond `renditionImage` met een transparante achtergrond
1. Schrijf de samenstelling, `renditionImage` zodat `rendition.path` het terug in AEM als activa uitvoering kan worden bewaard.

Deze code maakt gebruik van de API&#39;s [Jimp](https://github.com/oliver-moran/jimp#jimp) om deze afbeeldingstransformaties uit te voeren.

Workers voor het samenstellen van bedrijfsmiddelen moeten hun werk synchroon voltooien en de gegevens `rendition.path` moeten volledig worden teruggeschreven voordat de worker `renditionCallback` is voltooid. Dit vereist dat de asynchrone functievraag synchroon gebruikend de `await` exploitant wordt gemaakt. Als u niet bekend bent met asynchrone JavaScript-functies en hoe u deze synchroon kunt laten uitvoeren, moet u bekend zijn met de wachttijdoperator [van](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)JavaScript.

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

    const SIZE = parseInt(rendition.instructions.size) || 800; 
    const CONTRAST = parseFloat(rendition.instructions.contrast) || 0;
    const BRIGHTNESS = parseFloat(rendition.instructions.brightness) || 0;

    if (SIZE <= 10 || SIZE >= 10000) {
        throw new RenditionInstructionsError("'size' must be between 10 and 10,000");
    } else if (CONTRAST <= -1 || CONTRAST >= 1) {
        throw new RenditionInstructionsError("'contrast' must between -1 and 1");
    } else if (BRIGHTNESS <= -1 || BRIGHTNESS >= 1) {
        throw new RenditionInstructionsError("'brightness' must between -1 and 1");
    }

    // Create target rendition image of the target size with a transparent background (0x0)
    let renditionImage =  new Jimp(SIZE, SIZE, 0x0);

    // Read and perform transformations on the source binary image
    let image = await Jimp.read(source.path);

    // Crop a circle from the source asset, and then apply contrast and brightness using Jimp
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
    renditionImage.write(rendition.path);
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

Nu de arbeiderscode volledig is, en eerder in [manifest.yml](./manifest.md)geregistreerd en gevormd, kan het worden uitgevoerd gebruikend het lokale Hulpmiddel van de Ontwikkeling van de Compute van Activa om de resultaten te zien.

1. Van de wortel van het project van de Verwerking van Activa
1. Uitvoeren `app aio run`
1. Wacht tot Asset Compute Development Tool in een nieuw venster wordt geopend
1. In het dialoogvenster Een bestand __selecteren...__ een voorbeeldafbeelding selecteren die u wilt verwerken
   + Selecteer een voorbeeldafbeeldingsbestand dat u wilt gebruiken als binair bronelement
   + Als er nog geen bestanden zijn, tikt u op __(+)__ aan de linkerkant en uploadt u een [voorbeeldafbeeldingsbestand](../assets/samples/sample-file.jpg) en vernieuwt u het browservenster Ontwikkelingsgereedschappen
1. Werk het bij `"name": "rendition.png"` als deze worker een transparante PNG genereert.
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
1. Tik op __Uitvoeren__ en wacht tot de vertoning is gegenereerd
1. In de sectie __Uitvoeringen__ wordt een voorvertoning van de gegenereerde uitvoering weergegeven. Tik op de voorvertoning van de vertoning om de volledige vertoning te downloaden

   ![Standaard PNG-uitvoering](./assets/worker/default-rendition.png)

### De worker uitvoeren met parameters

De parameters, die via de configuraties van het Profiel van de Verwerking worden overgegaan, kunnen in de Hulpmiddelen van de Ontwikkeling van de Verwerking van Activa worden gesimuleerd door hen als sleutel/waardeparen op de vertoningsparameter JSON te verstrekken.

>[!WARNING]
>
>Tijdens lokale ontwikkeling, kunnen de waarden binnen worden overgegaan gebruikend diverse gegevenstypes, wanneer overgegaan van AEM als de Profielen van de Verwerking van de Cloud Service als koorden, zodat ervoor zorgen de correcte gegevenstypes indien nodig worden geparseerd.
> Bijvoorbeeld, vereist de `crop(width, height)` functie van Jimp zijn parameters `int`s. Als `parseInt(rendition.instructions.size)` `jimp.crop(SIZE, SIZE)` niet aan int wordt geparseerd, dan zal de vraag aan ontbreken aangezien de parameters incompatibel type &quot;Koord&quot;zullen zijn.

Onze code accepteert parameters voor:

+ `size` Hiermee definieert u de grootte van de vertoning (hoogte en breedte als gehele getallen)
+ `contrast` definieert het contrast aanpassen, moet liggen tussen -1 en 1, als floats
+ `brightness`  definieert de lichte aanpassing, moet liggen tussen -1 en 1, als floats

Deze worden in de worker gelezen `index.js` via:

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

1. Tik nogmaals __op Uitvoeren__
1. Tik op de voorvertoning van de vertoning om de gegenereerde vertoning te downloaden en te bekijken. Let op de afmetingen en hoe het contrast en de helderheid zijn gewijzigd ten opzichte van de standaardweergave.

   ![Parameterized PNG rendition](./assets/worker/parameterized-rendition.png)

1. Upload andere afbeeldingen naar het vervolgkeuzemenu voor het __bronbestand__ en probeer de worker met andere parameters uit te voeren.

## Worker index.js op Github

De finale `index.js` is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/actions/worker/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/worker/index.js)

## Problemen oplossen

### Vertoning wordt gedeeltelijk getekend geretourneerd

+ __Fout__: Uitvoering wordt onvolledig gerenderd, wanneer het bestand voor de totale uitvoering groot is

   ![Problemen oplossen - Vertoning wordt gedeeltelijk teruggestuurd](./assets/worker/troubleshooting__await.png)

+ __Oorzaak__: De functie van de worker `renditionCallback` wordt afgesloten voordat de uitvoering volledig kan worden uitgevoerd `rendition.path`.
+ __Resolutie__: Controleer de code van de douanearbeider en zorg ervoor alle asynchrone vraag synchroon wordt gemaakt.
