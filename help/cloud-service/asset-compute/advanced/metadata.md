---
title: Een Asset compute metagegevensworker ontwikkelen
description: Leer hoe u een Asset compute-metagegevensworker maakt die de meest gebruikte kleuren in een afbeeldingselement afleidt en de namen van de kleuren in AEM terugschrijft naar de metagegevens van het element.
feature: Asset Compute Microservices
topics: metadata, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 6ece6e82-efe9-41eb-adf8-78d9deed131e
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 0%

---

# Een Asset compute metagegevensworker ontwikkelen

De arbeiders van de Asset compute van de douane kunnen XMP (XML) gegevens produceren die terug naar AEM worden verzonden en als meta-gegevens op een middel worden opgeslagen.

Vaak voorkomende gevallen van gebruik zijn:

+ Integraties met systemen van derden, zoals een PIM (Product Information Management System), waar aanvullende metagegevens moeten worden opgehaald en opgeslagen op het middel
+ Integratie met Adobe-services, zoals Content and Commerce AI, om metagegevens over elementen te verfraaien met extra leerkenmerken voor computers
+ Metagegevens over het element afgeleid van zijn binair getal en opslaan als metagegevens over elementen in AEM as a Cloud Service

## Wat u gaat doen

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

In deze zelfstudie maken we een metagegevensworker voor de Asset compute die de meest gebruikte kleuren in een afbeeldingselement afleidt, en schrijven we de namen van de kleuren terug naar de metagegevens van het element in AEM. Hoewel de worker zelf standaard is, wordt deze zelfstudie gebruikt om te verkennen hoe Asset compute-workers kunnen worden gebruikt om metagegevens terug te schrijven naar elementen in AEM as a Cloud Service.

## Logische stroom van de aanroeping van een de meta-gegevensarbeider van een Asset compute

De aanroep van metagegevensworkers voor Asset computen is vrijwel gelijk aan die van [binaire rendering genereert workers](../develop/worker.md), waarbij het belangrijkste verschil het retourneringstype een XMP (XML)-uitvoering is waarvan de waarden ook naar de metagegevens van het element worden geschreven.

asset compute workers implementeren het Asset compute SDK worker API-contract in de `renditionCallback(...)` functie, die conceptueel is:

+ __Invoer:__ De oorspronkelijke binaire en verwerkingsprofielparameters van een AEM element
+ __Uitvoer:__ Een XMP (XML)-uitvoering is als een uitvoering aan het AEM-element vastgehouden en de metagegevens van het element

![Logische stroom van de worker voor metagegevens van asset compute](./assets/metadata/logical-flow.png)

1. De AEM Auteursdienst roept de Asset compute meta-gegevensarbeider aan, die de activa verstrekt __(1 bis)__ origineel binair, en __(1 ter)__ alle parameters die zijn gedefinieerd in het verwerkingsprofiel.
1. De Asset compute SDK organiseert de uitvoering van de metagegevens van de aangepaste Asset compute `renditionCallback(...)` functie, die een XMP (XML) vertoning afleidt, die op het binaire element van het element wordt gebaseerd __(1 bis)__ en alle parameters van het verwerkingsprofiel __(1 ter)__.
1. De Asset compute-worker slaat de XMP (XML)-weergave op `rendition.path`.
1. De XMP (XML) gegevens waarnaar wordt geschreven `rendition.path` wordt via de Asset compute SDK naar de AEM Author Service vervoerd en wordt aangeboden als __(4 bis)__ een tekstuitvoering en __(4 ter)__ Doorgegaan naar het metagegevensknooppunt van het element.

## Vorm manifest.yml{#manifest}

Alle werknemers van de Asset compute moeten in het [manifest.yml](../develop/manifest.md).

Open de `manifest.yml` en voeg een arbeidersingang toe die de nieuwe worker vormt, in dit geval `metadata-colors`.

_Herinneren `.yml` is gevoelig voor witruimte._

```
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
      metadata-colors:
        function: actions/metadata-colors/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          memorySize: 512 # in MB   
```

`function` verwijst naar de implementatie van de worker die is gemaakt in het dialoogvenster [volgende stap](#metadata-worker). De arbeiders van de naam semantisch (bijvoorbeeld `actions/worker/index.js` had een betere naam kunnen hebben `actions/rendition-circle/index.js`), zoals deze in de [URL van worker](#deploy) en bepaalt tevens de [naam testsuite van worker](#test).

De `limits` en `require-adobe-auth` per worker afzonderlijk worden geconfigureerd. In deze worker `512 MB` Er wordt geheugen toegewezen als de code (potentieel) grote binaire afbeeldingsgegevens inspecteert. De andere `limits` worden verwijderd om standaardwaarden te gebruiken.

## Een metagegevensworker ontwikkelen{#metadata-worker}

Een nieuw JavaScript-bestand voor een metagegevensworker maken in het Asset compute-project op het pad [defined manifest.yml voor de nieuwe worker](#manifest), om `/actions/metadata-colors/index.js`

### Npm-modules installeren

Extra npm-modules installeren ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors), en [kleurnaam](https://www.npmjs.com/package/color-namer)) die wordt gebruikt in deze Asset compute-worker.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Code metagegevensworker

Deze worker ziet er ongeveer hetzelfde uit als [rendition genererende worker](../develop/worker.md), het belangrijkste verschil is dat er XMP (XML) gegevens naar de `rendition.path` om terug naar AEM te worden opgeslagen.


```javascript
"use strict";

const { worker, SourceCorruptError } = require("@adobe/asset-compute-sdk");
const fs = require("fs").promises;

// Require the @adobe/asset-compute-xmp module to create XMP 
const { serializeXmp } = require("@adobe/asset-compute-xmp");

// Require supporting npm modules to derive image colors from image data
const getColors = require("get-image-colors");
// Require supporting npm modules to convert image colors to color names
const namer = require("color-namer");

exports.main = worker(async (source, rendition, params) => {
  // Perform any necessary source (input) checks
  const stats = await fs.stat(source.path);
  if (stats.size === 0) {
    // Throw appropriate errors whenever an erring condition is met
    throw new SourceCorruptError("source file is empty");
  }
  const MAX_COLORS = 10;
  const DEFAULT_COLORS_FAMILY = 'basic';

  // Read the color family parameter to use to derive the color names
  let colorsFamily = rendition.instructions.colorsFamily || DEFAULT_COLORS_FAMILY;

  if (['basic', 'hex', 'html', 'ntc', 'pantone', 'roygbiv'].indexOf(colorsFamily) === -1) { 
      colorsFamily = DEFAULT_COLORS_FAMILY;
  }
  
  // Use the `get-image-colors` module to derive the most common colors from the image
  let colors = await getColors(source.path, { options: MAX_COLORS });

  // Convert the color Chroma objects to their closest names
  let colorNames = colors.map((color) => getColorName(colorsFamily, color));

  // Serialize the data to XMP metadata
  // These properties are written to the [dam:Asset]/jcr:content/metadata resource
  // This stores
  // - The list of color names is stored in a JCR property named `wknd:colors`
  // - The colors family used to derive the color names is stored in a JCR property named `wknd:colorsFamily`
  const xmp = serializeXmp({
      // Use a Set to de-duplicate color names
      "wknd:colors": [...new Set(colorNames)],
      "wknd:colorsFamily": colorsFamily
    }, {
      // Define any property namespaces used in the above property/value definition
      // These namespaces are automatically registered in AEM if they do not yet exist
      namespaces: {
        wknd: "https://wknd.site/assets/1.0/",
      },
    }
  );

  // Save the XMP metadata to be written back to the asset's metadata node
  await fs.writeFile(rendition.path, xmp, "utf-8");
});

/**
 * Helper function that derives the closest color name for the color, based on the colors family
 * 
 * @param {*} colorsFamily the colors name family to use
 * @param {*} color the color to convert to a name
 */
function getColorName(colorsFamily, color) {
    if ('hex' === colorsFamily) {  return color; }

    let names = namer(color.rgb())[colorsFamily];

    if (names.length >= 1) { return names[0].name; }
}
```

## De metagegevensworker lokaal uitvoeren{#development-tool}

Als de code van de worker is voltooid, kan deze worden uitgevoerd met het lokale hulpprogramma voor ontwikkeling van Asset computen.

Omdat ons Asset compute project twee arbeiders (vorige) bevat [cirkeluitvoering](../develop/worker.md) en `metadata-colors` worker), de [asset compute Development Tool](../develop/development-tool.md) bij profieldefinitie worden uitvoeringsprofielen voor beide workers weergegeven. De tweede profieldefinitie verwijst naar de nieuwe `metadata-colors` worker.

![XML-metagegevensuitvoering](./assets/metadata/metadata-rendition.png)

1. Van de wortel van het project van de Asset compute
1. Uitvoeren `aio app run` om het Asset compute Development Tool te starten
1. In de __Een bestand selecteren...__ vervolgkeuzelijst, kies een [voorbeeldafbeelding](../assets/samples/sample-file.jpg) om te verwerken
1. In de tweede profieldefinitieconfiguratie, die naar `metadata-colors` worker, update `"name": "rendition.xml"` terwijl deze worker een XMP (XML)-uitvoering genereert. Voeg desgewenst een `colorsFamily` parameter (ondersteunde waarden) `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

   ```json
   {
       "renditions": [
           {
               "worker": "...",
               "name": "rendition.xml",
               "colorsFamily": "pantone"
           }
       ]
   }
   ```

1. Tikken __Uitvoeren__ en wacht tot de XML-uitvoering is gegenereerd
   + Aangezien beide workers worden vermeld in de profieldefinitie, genereren beide uitvoeringen. De definitie van het bovenste profiel verwijst optioneel naar de instelling [cirkelvormer](../develop/worker.md) kan worden verwijderd om te voorkomen dat het wordt uitgevoerd met het Ontwikkelingsgereedschap.
1. De __Uitvoeringen__ geeft een voorvertoning weer van de gegenereerde vertoning. Tik op de knop `rendition.xml` om het te downloaden, en het te openen in de Code van VS (of uw favoriete redacteur van XML/tekst) om het te herzien.

## De worker testen{#test}

Metagegevensworkers kunnen worden getest met de [zelfde Asset compute testframework als binaire uitvoeringen](../test-debug/test.md). Het enige verschil is `rendition.xxx` bestand in het testgeval moet de verwachte XMP (XML)-uitvoering zijn.

1. Maak de volgende structuur in het project Asset compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Gebruik de [voorbeeldbestand](../assets/samples/sample-file.jpg) als testcase `file.jpg`.
3. Voeg de volgende JSON toe aan de `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Noteer de `"fmt": "xml"` is vereist om de testsuite de opdracht te geven een `.xml` op tekst gebaseerde uitvoering.

4. Geef de verwachte XML op in de `rendition.xml` bestand. Dit kan worden verkregen door:
   + Het testinvoerbestand uitvoeren via het Development Tool en de (gevalideerde) XML-uitvoering opslaan.

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Uitvoeren `aio app test` van de wortel van het project van de Asset compute om alle testsuites uit te voeren.

### De worker distribueren naar Adobe I/O Runtime{#deploy}

Om deze nieuwe meta-gegevensarbeider van AEM Assets aan te halen, moet het aan Adobe I/O Runtime worden opgesteld, gebruikend het bevel:

```
$ aio app deploy
```

![Implementatie van een aio-app](./assets/metadata/aio-app-deploy.png)

Merk op dit alle arbeiders in het project zal opstellen. Controleer de [instructies voor onverkorte implementatie](../deploy/runtime.md) voor hoe te om aan werkruimten van het Stadium en van de Productie op te stellen.

### Integreren met AEM verwerkingsprofielen{#processing-profile}

Roep de worker van AEM aan door een nieuwe aangepaste verwerkingsprofielservice te maken of door een bestaande aangepaste verwerkingsprofielservice te wijzigen die deze geïmplementeerde worker activeert.

![Profiel verwerken](./assets/metadata/processing-profile.png)

1. Aanmelden bij AEM as a Cloud Service auteur als een __AEM__
1. Navigeren naar __Gereedschappen > Middelen > Profielen verwerken__
1. __Maken__ een nieuwe, of __bewerken__ en bestaande, verwerkingsprofiel
1. Tik op de knop __Aangepast__ en tikken __Nieuwe toevoegen__
1. De nieuwe service definiëren
   + __Metagegevensuitvoering maken__: Schakelen naar actief
   + __Eindpunt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Dit is de URL voor de worker die tijdens het [inzetten](#deploy) of met de opdracht `aio app get-url`. Zorg ervoor dat de URL-punten zich op de juiste werkruimte bevinden, op basis van de AEM as a Cloud Service omgeving.
   + __Serviceparameters__
      + Tikken __Parameter toevoegen__
         + Sleutel: `colorFamily`
         + Waarde: `pantone`
            + Ondersteunde waarden: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __MIME-typen__
      + __Omvat:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Dit zijn de enige MIME types die door de derde npm modules worden gesteund die worden gebruikt om de kleuren af te leiden.
      + __Omvat niet:__ `Leave blank`
1. Tikken __Opslaan__ rechtsboven
1. Verwerkingsprofiel toepassen op een AEM Assets-map als dit nog niet is gebeurd

### Metagegevensschema bijwerken{#metadata-schema}

Als u de metagegevens van kleuren wilt bekijken, wijst u twee nieuwe velden in het metagegevensschema van de afbeelding toe aan de nieuwe eigenschappen van de metagegevens die de worker vult.

![Metadataschema](./assets/metadata/metadata-schema.png)

1. Navigeer in de AEM-auteurservice naar __Gereedschappen > Elementen > Metagegevensschema&#39;s__
1. Navigeren naar __default__ en selecteren en bewerken __image__ en voeg alleen-lezen formuliervelden toe om de gegenereerde metagegevens in kleur weer te geven
1. Voeg een __Tekst met één regel__
   + __Veldlabel__: `Colors Family`
   + __Toewijzen aan eigenschap__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Regels > Veld > Bewerken uitschakelen__: Ingeschakeld
1. Voeg een __Meerdere waardetekst__
   + __Veldlabel__: `Colors`
   + __Toewijzen aan eigenschap__: `./jcr:content/metadata/wknd:colors`
1. Tikken __Opslaan__ rechtsboven

## Bezig met verwerken elementen

![Details van element](./assets/metadata/asset-details.png)

1. Navigeer in de AEM-auteurservice naar __Middelen > Bestanden__
1. Navigeer naar de map (of submap) waarop het verwerkingsprofiel is toegepast
1. Een nieuwe afbeelding (JPEG, PNG, GIF of SVG) uploaden naar de map of bestaande afbeeldingen opnieuw verwerken met de bijgewerkte [Profiel verwerken](#processing-profile)
1. Selecteer het element als de verwerking is voltooid en tik op __eigenschappen__ in de bovenste actiebalk om de metagegevens van de balk weer te geven
1. Controleer de `Colors Family` en `Colors` [metagegevensvelden](#metadata-schema) voor de metagegevens die zijn teruggeschreven van de aangepaste Asset compute-metagegevensworker.

Als de metagegevens voor kleuren naar de metagegevens van het element zijn geschreven, kunt u de `[dam:Asset]/jcr:content/metadata` bron, deze meta-gegevens wordt geïndexeerd verhoogde activa ontdekkingsvermogen gebruikend deze termijnen via onderzoek, en zij kunnen zelfs aan het binaire getal van de activa worden geschreven als dan __DAM-metagegevensterugkoppeling__ de workflow wordt erop aangeroepen.

### Metagegevensuitvoering in AEM Assets

![AEM Assets-bestand voor metagegevensuitvoering](./assets/metadata/cqdam-metadata-rendition.png)

Het werkelijke XMP dat door de metagegevensworker van de Asset compute wordt gegenereerd, wordt ook opgeslagen als een aparte uitvoering op het element. Dit bestand wordt over het algemeen niet gebruikt, maar de toegepaste waarden op het metagegevensknooppunt van het element worden gebruikt, maar de onbewerkte XML-uitvoer van de worker is beschikbaar in AEM.

## metadata-colors worker code on Github

De definitieve `metadata-colors/index.js` is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

De definitieve `test/asset-compute/metadata-colors` testsuite beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
