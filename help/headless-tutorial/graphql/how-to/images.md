---
title: Geoptimaliseerde afbeeldingen gebruiken met AEM Headless
description: Leer hoe u geoptimaliseerde afbeeldings-URL's kunt aanvragen met AEM Headless.
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 300
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---

# Geoptimaliseerde afbeeldingen met AEM Headless {#images-with-aem-headless}

De beelden zijn een kritisch aspect van [ het ontwikkelen van rijke, dwingende AEM headless ervaringen ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html). AEM Headless ondersteunt het beheer van afbeeldingselementen en de geoptimaliseerde levering ervan.

Inhoudsfragmenten die worden gebruikt in AEM Headless-inhoudsmodellering, verwijzen vaak naar afbeeldingselementen die zijn bedoeld voor weergave in de headless-ervaring. AEM GraphQL-query&#39;s kunnen worden geschreven om URL&#39;s aan te bieden voor afbeeldingen op basis van de locatie waar naar de afbeelding wordt verwezen.

Het type `ImageRef` heeft vier URL-opties voor inhoudsverwijzingen:

+ `_path` is het pad waarnaar wordt verwezen in AEM en bevat geen AEM-oorsprong (hostnaam)
+ `_dynamicUrl` is de URL voor levering van afbeeldingselementen die voor het web zijn geoptimaliseerd.
   + `_dynamicUrl` bevat geen AEM-oorsprong. Het domein (AEM-auteur of AEM-publicatieservice) moet daarom door de clienttoepassing worden opgegeven.
+ `_authorUrl` is de volledige URL naar het afbeeldingselement op AEM Author
   + [ de Auteur van AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) kan worden gebruikt om een voorproefervaring van de hoofdloze toepassing te verstrekken.
+ `_publishUrl` is de volledige URL naar het afbeeldingselement in AEM Publish
   + [ AEM publiceert ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) is typisch waar de productieleiding van de hoofdloze toepassing beelden van toont.

`_dynamicUrl` is de aanbevolen URL voor het leveren van afbeeldingselementen en moet, waar mogelijk, het gebruik van `_path` , `_authorUrl` en `_publishUrl` vervangen.

|                                | AEM as a Cloud Service | AEM as a Cloud Service RDE | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| Ondersteunt webgeoptimaliseerde afbeeldingen? | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Afbeeldingen met AEM Headless"
>abstract="Leer hoe AEM Headless het beheer van afbeeldingselementen en de geoptimaliseerde levering ervan ondersteunt."

## Inhoudsfragmentmodel

Verzeker het gebied van het Fragment van de Inhoud dat de beeldverwijzing bevat van het __gegevenstype van de inhoudsverwijzing__ is.

De types van gebied worden herzien in het [ Model van het Fragment van de Inhoud ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html), door het gebied te selecteren, en het __3} lusje van Eigenschappen {op het recht te inspecteren.__

![ Model van het Fragment van de Inhoud met inhoudsverwijzing naar een beeld ](./assets/images/content-fragment-model.jpeg)

## GraphQL-query voortgezet

Retourneert het veld in de GraphQL-query als het `ImageRef` -type en vraagt het `_dynamicUrl` -veld aan. Bijvoorbeeld, die een avontuur in het [ project van de Plaats van WKND ](https://github.com/adobe/aem-guides-wknd) vragen en het omvatten beeld URL voor de verwijzingen van beeldactiva in zijn `primaryImage` gebied, kan met een nieuwe voortgeduurde vraag `wknd-shared/adventure-image-by-path` worden gedaan die als wordt bepaald:

```graphql {highlight="11"}
query($path: String!, $imageFormat: AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int, $imageQuality: Int) {
  adventureByPath(
    _path: $path
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
    }
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### Query-variabelen

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "imageFormat": "JPG",
  "imageWidth": 1000,
}
```

Voor de variabele `$path` die in het filter `_path` wordt gebruikt, is het volledige pad naar het inhoudsfragment vereist (bijvoorbeeld `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp` ).

In `_assetTransform` wordt gedefinieerd hoe `_dynamicUrl` wordt samengesteld om de weergegeven afbeelding te optimaliseren. URL&#39;s met webgeoptimaliseerde afbeeldingen kunnen ook op de client worden aangepast door de queryparameters van de URL te wijzigen.

| GraphQL, parameter | Beschrijving | Vereist | Variabele GraphQL-waarden |
|-------------------|------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------|
| `format` | De indeling van het afbeeldingselement. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`, `WEBP`, `WEBPLL`, `WEBPLY` |
| `seoName` | Naam van bestandssegment in URL. Indien niet opgegeven, wordt de naam van het afbeeldingselement gebruikt. | ✘ | Alphanumeric, `-` of `_` |
| `crop` | Het uitsnijdkader dat uit de afbeelding is genomen, moet binnen de grootte van de afbeelding vallen | ✘ | Positieve gehele getallen die een snijgebied binnen de grenzen van de afmetingen van de oorspronkelijke afbeelding definiëren |
| `size` | Grootte van de uitvoerafbeelding (zowel hoogte als breedte) in pixels. | ✘ | Positieve gehele getallen |
| `rotation` | Rotatie van de afbeelding in graden. | ✘ | `R90`, `R180`, `R270` |
| `flip` | Draai de afbeelding om. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` |
| `quality` | Afbeeldingskwaliteit in procenten van oorspronkelijke kwaliteit. | ✘ | 1-100 |
| `width` | Breedte van de uitvoerafbeelding in pixels. Wanneer `size` wordt opgegeven, wordt `width` genegeerd. | ✘ | Positief geheel getal |
| `preferWebP` | Als `true` en AEM een WebP dienen als de browser het, ongeacht `format` steunt. | ✘ | `true`, `false` |


## GraphQL-reactie

Het resulterende JSON-antwoord bevat de aangevraagde velden met de voor het web geoptimaliseerde URL naar de afbeeldingselementen.

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&width=1000&quality=80"
        }
      }
    }
  }
}
```

Als u de webgeoptimaliseerde afbeelding van de referentieafbeelding in uw toepassing wilt laden, gebruikt u de `_dynamicUrl` van de `primaryImage` als bron-URL van de afbeelding.

In React ziet het weergeven van een webgeoptimaliseerde afbeelding in AEM Publish er als volgt uit:

```jsx
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

Vergeet niet dat `_dynamicUrl` geen AEM-domein bevat. U moet dus de gewenste oorsprong opgeven voor de afbeeldings-URL die moet worden opgelost.

## Responsieve URL&#39;s

In het bovenstaande voorbeeld ziet u hoe u een afbeelding van één formaat gebruikt, maar in webervaringen zijn responsieve afbeeldingssets vaak vereist. De responsieve beelden kunnen worden uitgevoerd gebruikend [ img srcsets ](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) of [ beeldelementen ](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset). Het volgende codefragment laat zien hoe u de `_dynamicUrl` als basis kunt gebruiken. `width` is een URL-parameter die u vervolgens aan `_dynamicUrl` kunt toevoegen voor het inschakelen van verschillende responsieve weergaven.

```javascript
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

## Voorbeeld Reageren

Laten wij een eenvoudige React toepassing tot stand brengen die Web-geoptimaliseerde beelden na [ ontvankelijke beeldpatronen ](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/) toont. Er zijn twee hoofdpatronen voor responsieve afbeeldingen:

+ [ img element met srcset ](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) voor verhoogde prestaties
+ [ element van het Beeld ](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) voor ontwerpcontrole

### Img-element met script

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

[ img elementen met srcset ](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) worden gebruikt met het `sizes` attribuut om verschillende beeldactiva voor verschillende het schermgrootte te verstrekken. Img-sets zijn handig wanneer u verschillende afbeeldingselementen voor verschillende schermgrootten aanbiedt.

### Figuurelement

[ de elementen van het Beeld ](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture) worden gebruikt met veelvoudige `source` elementen om verschillende beeldactiva voor verschillende het schermgrootte te verstrekken. Afbeeldingselementen zijn handig voor verschillende afbeeldingsuitvoeringen voor verschillende schermgrootten.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### Voorbeeldcode

Deze eenvoudige React app gebruikt [ AEM Headless SDK ](./aem-headless-sdk.md) om AEM Headless APIs voor een inhoud van het Avontuur te vragen, en toont het web-geoptimaliseerde beeld gebruikend [ img element met srcset ](#img-element-with-srcset) en [ beeldelement ](#picture-element). De `srcset` en `sources` gebruiken een aangepaste `setParams` functie om de voor het web geoptimaliseerde leverquery-parameter toe te voegen aan `_dynamicUrl` van de afbeelding, dus wijzig de geleverde afbeeldingsuitvoering op basis van de behoeften van de webclient.

Het vragen tegen AEM wordt uitgevoerd in de haak van het douaneantwoord [ useAdventureByPath die AEM Headless SDK ](./aem-headless-sdk.md#graphql-persisted-queries) gebruikt.

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} imageUrl the image URL
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setOptimizedImageUrlParams(imageUrl, params) {
    let url = new URL(imageUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { imageFormat: "JPG" }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  const alt = data.adventureByPath.item.title;
  const imageUrl =  AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setOptimizedImageUrlParams(imageUrl, { width: 1000 })}
        srcSet={
            `${setOptimizedImageUrlParams(imageUrl, { width: 1000 })} 1000w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 1600 })} 1600w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setOptimizedImageUrlParams(imageUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
