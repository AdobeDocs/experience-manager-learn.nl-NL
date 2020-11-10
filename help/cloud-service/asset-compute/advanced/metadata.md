---
title: Een Asset compute metagegevensworker ontwikkelen
description: Leer hoe u een Asset compute-metagegevensworker maakt die de meest gebruikte kleuren in een afbeeldingselement afleidt en de namen van de kleuren in AEM terugschrijft naar de metagegevens van het element.
feature: asset-compute
topics: metadata, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6448
thumbnail: 327313.jpg
translation-type: tm+mt
source-git-commit: c2a8e6c3ae6dcaa45816b1d3efe569126c6c1e60
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 0%

---


# Een Asset compute metagegevensworker ontwikkelen

De arbeiders van de Asset compute van de douane kunnen XMP (XML) gegevens produceren die terug naar AEM worden verzonden en als meta-gegevens op een middel worden opgeslagen.

Vaak voorkomende gevallen van gebruik zijn:

+ Integraties met systemen van derden, zoals een PIM (Product Information Management System), waar aanvullende metagegevens moeten worden opgehaald en opgeslagen op het middel
+ Integratie met Adobe-services, zoals Content and Commerce AI, om metagegevens over elementen te vergroten met extra leerkenmerken voor computers
+ Metagegevens over het element afgeleid van zijn binair getal en opslaan als metagegevens van elementen in AEM als Cloud Service

## Wat u gaat doen

>[!VIDEO](https://video.tv.adobe.com/v/327313?quality=12&learn=on)

In deze zelfstudie maken we een metagegevensworker voor de Asset compute die de meest gebruikte kleuren in een afbeeldingselement afleidt, en schrijven we de namen van de kleuren terug naar de metagegevens van het element in AEM. Hoewel de worker zelf standaard is, wordt deze zelfstudie gebruikt om te verkennen hoe Asset compute-workers kunnen worden gebruikt om metagegevens terug te schrijven naar elementen in AEM als Cloud Service.

## Logische stroom van de aanroeping van een de meta-gegevensarbeider van een Asset compute

De aanroeping van de arbeiders van de meta-gegevens van de Asset compute is bijna identiek aan die van [binaire vertoning die arbeiders](../develop/worker.md)produceert, met het belangrijkste verschil is het terugkeertype een XMP (XML) vertoning de waarvan waarden ook aan de meta-gegevens van de activa worden geschreven.

asset compute workers implementeren het Asset compute SDK worker API-contract, in de `renditionCallback(...)` functie, die conceptueel is:

+ __Invoer:__ De oorspronkelijke binaire en verwerkingsprofielparameters van een AEM element
+ __Uitvoer:__ Een XMP (XML)-uitvoering is als een uitvoering aan het AEM-element vastgehouden en de metagegevens van het element

![Logische stroom van de worker voor metagegevens van asset compute](./assets/metadata/logical-flow.png)

1. De dienst van de Auteur AEM roept de de meta-gegevensarbeider van de Asset compute aan, die het originele binaire __(1a)__ binaire element van het element verstrekt, en __(1b)__ om het even welke die parameters in het Profiel van de Verwerking worden bepaald.
1. De Asset compute SDK organiseert de uitvoering van de `renditionCallback(...)` functie van de worker voor metagegevens van de aangepaste Asset compute, waarbij een XMP (XML)-uitvoering wordt afgeleid, op basis van de binaire __(1a)__ waarde van het element en alle verwerkingsparameters __(1b)__.
1. De Asset compute worker slaat de XMP (XML)-weergave op `rendition.path`.
1. De XMP (XML) gegevens waarnaar `rendition.path` wordt geschreven, worden via de Asset compute SDK naar AEM Author Service verzonden en worden als __(4a)__ een tekstuitvoering en __(4b)__ die tot het metagegevensknooppunt van het element blijven bestaan, beschikbaar gesteld.

## Vorm manifest.yml{#manifest}

Alle werknemers van de Asset compute moeten in [manifest.yml](../develop/manifest.md)worden geregistreerd.

Open het project `manifest.yml` en voeg een arbeidersingang toe die de nieuwe worker vormt, in dit geval `metadata-colors`.

_Onthoud `.yml` dat witruimte gevoelig is._

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

`function` verwijst naar de implementatie van de worker die in de [volgende stap](#metadata-worker)is gemaakt. De arbeiders van de naam semantisch (bijvoorbeeld, `actions/worker/index.js` zou de beter genoemde `actions/rendition-circle/index.js`) kunnen geweest zijn, aangezien deze in URL [van de](#deploy) worker tonen en ook de de versiemap [van de](#test)worker van de testsuite bepalen.

De configuratie `limits` en `require-adobe-auth` wordt op discrete wijze per worker uitgevoerd. In deze worker wordt `512 MB` geheugen toegewezen terwijl de code (mogelijk) grote binaire afbeeldingsgegevens inspecteert. De andere `limits` worden verwijderd om standaardwaarden te gebruiken.

## Een metagegevensworker ontwikkelen{#metadata-worker}

Maak een nieuw JavaScript-bestand voor een metagegevensworker in het Asset compute-project op het pad [gedefinieerd manifest.yml voor de nieuwe worker](#manifest)op `/actions/metadata-colors/index.js`

### Npm-modules installeren

Installeer de extra npm-modules ([@adobe/asset-compute-xmp](https://www.npmjs.com/package/@adobe/asset-compute-xmp?activeTab=versions), [get-image-colors](https://www.npmjs.com/package/get-image-colors)en [color-namer](https://www.npmjs.com/package/color-namer)) die in deze Asset compute-worker worden gebruikt.

```
$ npm install @adobe/asset-compute-xmp
$ npm install get-image-colors
$ npm install color-namer
```

### Code metagegevensworker

Deze worker ziet er ongeveer hetzelfde uit als de worker [die de](../develop/worker.md)uitvoering genereert. Het belangrijkste verschil is dat deze worker XMP (XML) gegevens naar de worker schrijft `rendition.path` om terug te worden opgeslagen naar AEM.


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
      // These namespaces will be automatically registered in AEM if they do not yet exist
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

Omdat ons Asset compute project twee arbeiders (de vorige [cirkelvertoning](../develop/worker.md) en deze `metadata-colors` arbeider) bevat, maakt de het profieldefinitie van het Hulpmiddel van de Ontwikkeling van de [Asset compute](../develop/development-tool.md) een lijst van uitvoeringsprofielen voor beide arbeiders. De tweede profieldefinitie verwijst naar de nieuwe `metadata-colors` worker.

![XML-metagegevensuitvoering](./assets/metadata/metadata-rendition.png)

1. Van de wortel van het project van de Asset compute
1. Uitvoeren `aio app run` om het Hulpmiddel van de Ontwikkeling van de Asset compute te beginnen
1. In het dialoogvenster Een bestand __selecteren...__ vervolgkeuzelijst, kies een [voorbeeldafbeelding](../assets/samples/sample-file.jpg) om te verwerken
1. In de tweede profieldefinitieconfiguratie, die naar de `metadata-colors` worker wijst, werkt u bij `"name": "rendition.xml"` terwijl deze worker een XMP (XML)-uitvoering genereert. Voeg desgewenst een `colorsFamily` parameter toe (ondersteunde waarden `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`).

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
1. Tik op __Uitvoeren__ en wacht tot de XML-uitvoering is gegenereerd
   + Aangezien beide workers worden vermeld in de profieldefinitie, genereren beide uitvoeringen. Optioneel kan de definitie van het bovenste profiel die naar de worker [voor](../develop/worker.md) cirkeluitvoering verwijst, worden verwijderd om te voorkomen dat deze vanuit het ontwikkelingsgereedschap wordt uitgevoerd.
1. In de sectie __Uitvoeringen__ wordt een voorvertoning van de gegenereerde uitvoering weergegeven. Tik op de koppeling `rendition.xml` om deze te downloaden en open deze in VS-code (of in uw favoriete XML-/teksteditor) om deze te bekijken.

## De worker testen{#test}

Metagegevensworkers kunnen worden getest met [hetzelfde testframework voor Asset compute als binaire uitvoeringen](../test-debug/test.md). Het enige verschil is dat het `rendition.xxx` bestand in het testgeval de verwachte XMP (XML)-uitvoering moet zijn.

1. Maak de volgende structuur in het project Asset compute:

   ```
   /test/asset-compute/metadata-colors/success-pantone/
   
       file.jpg
       params.json
       rendition.xml
   ```

2. Gebruik het [voorbeeldbestand](../assets/samples/sample-file.jpg) als testcase `file.jpg`.
3. Voeg de volgende JSON toe aan de `params.json`.

   ```
   {
       "fmt": "xml",
       "colorsFamily": "pantone"
   }
   ```

   Merk op `"fmt": "xml"` `.xml` wordt vereist om de testreeks op te dragen om een op tekst-gebaseerde vertoning te produceren.

4. Geef de verwachte XML in het `rendition.xml` bestand op. Dit kan worden verkregen door:
   + Het testinvoerbestand uitvoeren via het Development Tool en de (gevalideerde) XML-uitvoering opslaan.

   ```
   <?xml version="1.0" encoding="UTF-8"?><rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:wknd="https://wknd.site/assets/1.0/"><rdf:Description><wknd:colors><rdf:Seq><rdf:li>Silver</rdf:li><rdf:li>Black</rdf:li><rdf:li>Outer Space</rdf:li></rdf:Seq></wknd:colors><wknd:colorsFamily>pantone</wknd:colorsFamily></rdf:Description></rdf:RDF>
   ```

5. Voer `aio app test` van de wortel van het project van de Asset compute uit om alle testsuites uit te voeren.

### De worker distribueren naar Adobe I/O Runtime{#deploy}

Om deze nieuwe meta-gegevensarbeider van AEM Assets aan te halen, moet het aan Adobe I/O Runtime worden opgesteld, gebruikend het bevel:

```
$ aio app deploy
```

![Implementatie van een aio-app](./assets/metadata/aio-app-deploy.png)

Merk op dit alle arbeiders in het project zal opstellen. Herzie de [unabriafloop plaatsingsinstructies](../deploy/runtime.md) voor hoe te om aan werkruimten van het Stadium en van de Productie op te stellen.

### Integreren met AEM verwerkingsprofielen{#processing-profile}

Roep de worker van AEM aan door een nieuwe aangepaste verwerkingsprofielservice te maken of door een bestaande aangepaste verwerkingsprofielservice te wijzigen die deze geïmplementeerde worker activeert.

![Profiel verwerken](./assets/metadata/processing-profile.png)

1. Aanmelden bij AEM als Cloud Service Author-service als __AEM beheerder__
1. Ga naar __Gereedschappen > Middelen > Profielen verwerken__
1. __Een__ nieuw verwerkingsprofiel maken of een bestaand verwerkingsprofiel __bewerken__
1. Tik op het tabblad __Aangepast__ en tik op Nieuw __toevoegen__
1. De nieuwe service definiëren
   + __Metagegevensuitvoering__ maken: Schakelen naar actief
   + __Eindpunt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/metadata-colors`
      + Dit is de URL voor de worker die tijdens de [implementatie](#deploy) of het gebruik van de opdracht is verkregen `aio app get-url`. Zorg ervoor dat de URL-punten zich in de juiste werkruimte bevinden, op basis van de AEM als een Cloud Service-omgeving.
   + __Serviceparameters__
      + Tik op parameter __toevoegen__
         + Sleutel: `colorFamily`
         + Waarde: `pantone`
            + Ondersteunde waarden: `basic`, `hex`, `html`, `ntc`, `pantone`, `roygbiv`
   + __MIME-typen__
      + __Omvat:__ `image/jpeg`, `image/png`, `image/gif`, `image/svg`
         + Dit zijn de enige MIME types die door de derde npm modules worden gesteund die worden gebruikt om de kleuren af te leiden.
      + __Omvat niet:__ `Leave blank`
1. Tik op __Opslaan__ rechtsboven
1. Verwerkingsprofiel toepassen op een AEM Assets-map als dit nog niet is gebeurd

### Metagegevensschema bijwerken{#metadata-schema}

Als u de metagegevens van kleuren wilt bekijken, wijst u twee nieuwe velden in het metagegevensschema van de afbeelding toe aan de nieuwe eigenschappen van de metagegevens die de worker vult.

![Metadataschema](./assets/metadata/metadata-schema.png)

1. Navigeer in de AEM-auteurservice naar __Gereedschappen > Middelen > Metagegevensschema&#39;s__
1. Ga naar de __standaard__ en selecteer en bewerk de __afbeelding__ en voeg alleen-lezen formuliervelden toe om de gegenereerde metagegevens in de kleur weer te geven
1. Tekst met __één regel toevoegen__
   + __Veldlabel__: `Colors Family`
   + __Toewijzen aan eigenschap__: `./jcr:content/metadata/wknd:colorsFamily`
   + __Regels > Veld > Bewerken__ uitschakelen: Ingeschakeld
1. Een __meerwaardetekst toevoegen__
   + __Veldlabel__: `Colors`
   + __Toewijzen aan eigenschap__: `./jcr:content/metadata/wknd:colors`
1. Tik op __Opslaan__ rechtsboven

## Bezig met verwerken elementen

![Details van element](./assets/metadata/asset-details.png)

1. Navigeer in de AEM-auteurservice naar __Middelen > Bestanden__
1. Navigeer naar de map (of submap) waarop het verwerkingsprofiel is toegepast
1. Een nieuwe afbeelding (JPEG, PNG, GIF of SVG) uploaden naar de map of bestaande afbeeldingen opnieuw verwerken met het bijgewerkte [verwerkingsprofiel](#processing-profile)
1. Wanneer de verwerking is voltooid, selecteert u het element en tikt u op de __eigenschappen__ in de bovenste actiebalk om de metagegevens van het element weer te geven
1. Controleer de velden `Colors Family` en `Colors` metagegevens [](#metadata-schema) voor de metagegevens die zijn geschreven van de aangepaste Asset compute-metagegevensworker.

Met de kleurenmeta-gegevens die aan de meta-gegevens van de activa worden geschreven, op het `[dam:Asset]/jcr:content/metadata` middel, wordt deze meta-gegevens geïndexeerd verhoogde de ontdekkingscapaciteit van activa gebruikend deze termijnen via onderzoek, en zij kunnen zelfs aan het binaire getal van de activa worden geschreven als toen de __werkschema van de Terugverwijzing__ van Meta-gegevens DAM op het wordt aangehaald.

### Metagegevensuitvoering in AEM Assets

![AEM Assets-bestand voor metagegevensuitvoering](./assets/metadata/cqdam-metadata-rendition.png)

Het werkelijke XMP dat door de metagegevensworker van de Asset compute wordt gegenereerd, wordt ook opgeslagen als een aparte uitvoering op het element. Dit bestand wordt over het algemeen niet gebruikt, maar de toegepaste waarden op het metagegevensknooppunt van het element worden gebruikt, maar de onbewerkte XML-uitvoer van de worker is beschikbaar in AEM.

## metadata-colors worker code on Github

De finale `metadata-colors/index.js` is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/actions/metadata-colors/index.js](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/actions/metadata-colors/index.js)

De laatste `test/asset-compute/metadata-colors` testsuite is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute/test/asset-compute/metadata-colors](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/test/asset-compute/metadata-colors)
