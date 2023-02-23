---
title: Afbeeldingen gebruiken met AEM zonder kop
description: Leer hoe u URL's met verwijzing naar afbeeldingsinhoud kunt aanvragen en aangepaste uitvoeringen kunt gebruiken met AEM Headless.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10253
thumbnail: KT-10253.jpeg
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
source-git-commit: ae49fb45db6f075a34ae67475f2fcc5658cb0413
workflow-type: tm+mt
source-wordcount: '1177'
ht-degree: 0%

---

# Afbeeldingen met AEM zonder kop {#images-with-aem-headless}

Afbeeldingen zijn een essentieel aspect van [ontwikkeling van rijke, dwingende AEM eindeloze ervaringen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html). AEM Headless ondersteunt het beheer van afbeeldingselementen en de geoptimaliseerde levering ervan.

Inhoudsfragmenten die worden gebruikt in AEM modellering van inhoud zonder kop, verwijzen vaak naar afbeeldingselementen die zijn bedoeld voor weergave in de headless-ervaring. AEM GraphQL-query&#39;s kunnen worden geschreven om URL&#39;s aan te bieden voor afbeeldingen op basis van de locatie waar naar de afbeelding wordt verwezen.

De `ImageRef` Het type heeft drie URL-opties voor inhoudsverwijzingen:

+ `_path` is het referenced weg in AEM, en omvat geen AEM oorsprong (gastheernaam)
+ `_authorUrl` is de volledige URL naar het afbeeldingselement op de AEM-auteur
   + [AEM-auteur](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) kan worden gebruikt om een voorvertoning van de toepassing zonder kop weer te geven.
+ `_publishUrl` is de volledige URL naar het afbeeldingselement in AEM Publish
   + [AEM-publicatie](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html) is typisch waar de productie plaatsing van de hoofdloze toepassing beelden van toont.

De velden kunnen het best worden gebruikt op basis van de volgende criteria:

| Afbeeldingsverwijzing-velden | Client-webtoepassing vanuit AEM | Client-app vraagt naar AEM-auteur | Vragen over clienttoepassingen publiceren in AEM |
|--------------------|:------------------------------:|:-----------------------------:|:------------------------------:|
| `_path` | ✔ | ✔ (App moet host opgeven in URL) | ✔ (App moet host opgeven in URL) |
| `_authorUrl` | ✘ | ✔ | ✘ |
| `_publishUrl` | ✘ | ✘ | ✔ |

Gebruik van `_authorUrl` en `_publishUrl` moet worden uitgelijnd op het eindpunt van AEM GraphQL dat wordt gebruikt om de GraphQL-respons te verkrijgen.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="Afbeeldingen met AEM zonder kop"
>abstract="Leer hoe AEM Headless het beheer van beeldactiva en hun geoptimaliseerde levering steunt."

## Inhoudsfragmentmodel

Zorg ervoor dat het veld Inhoudsfragment dat de afbeeldingsverwijzing bevat, zich bevindt in het veld __inhoudsverwijzing__ gegevenstype.

Veldtypen worden gecontroleerd in het dialoogvenster [Inhoudsfragmentmodel](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)door het veld te selecteren en de __Eigenschappen__ rechts.

![Inhoudsfragmentmodel met inhoudsverwijzing naar een afbeelding](./assets/images/content-fragment-model.jpeg)

## GraphQL-query voortgezet

Retourneer het veld in de GraphQL-query als de `ImageRef` typen en de juiste velden opvragen `_path`, `_authorUrl`, of `_publishUrl` vereist door uw toepassing. U kunt bijvoorbeeld een avontuur opvragen in het dialoogvenster [WKND-siteproject](https://github.com/adobe/aem-guides-wknd) en de afbeeldings-URL opnemen voor de verwijzingen naar afbeeldingselementen in de `primaryImage` veld, kan worden uitgevoerd met een nieuwe, voortgezette query `wknd-shared/adventure-image-by-path` gedefinieerd als:

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

De `$path` in de `_path` filter vereist het volledige pad naar het inhoudsfragment (bijvoorbeeld `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`).

## GraphQL-reactie

Het resulterende JSON-antwoord bevat de gevraagde velden met de URL&#39;s naar de afbeeldingselementen.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
          "_authorUrl": "https://author-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
          "_publishUrl": "https://publish-p123-e789.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"
        }
      }
    }
  }
}
```

Gebruik het desbetreffende veld om de afbeelding waarnaar wordt verwezen, in uw toepassing te laden. `_path`, `_authorUrl`, of `_publishUrl` van de `adventurePrimaryImage` als de bron-URL van de afbeelding.

De domeinen van `_authorUrl` en `_publishUrl` worden automatisch gedefinieerd door AEM as a Cloud Service met behulp van de [ExternalAlizer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/externalizer.html).

In React ziet het weergeven van de afbeelding uit AEM Publish er als volgt uit:

```html
<img src={ data.adventureByPath.item.primaryImage._publishUrl } />
```

## Afbeeldingsuitvoeringen

Afbeeldingselementen ondersteunen aanpasbare [vertoningen](../../../assets/authoring/renditions.md), dit zijn alternatieve weergaven van het oorspronkelijke element. Aangepaste uitvoeringen kunnen helpen een headless-ervaring te optimaliseren. In plaats van het oorspronkelijke afbeeldingselement, dat vaak een groot bestand met hoge resolutie is, aan te vragen, kunnen geoptimaliseerde uitvoeringen worden aangevraagd door de toepassing zonder kop.

### Uitvoeringen maken

AEM Assets-beheerders definiëren de aangepaste uitvoeringen met behulp van Profielen verwerken. De verwerkingsprofielen kunnen vervolgens rechtstreeks op specifieke mapstructuren of middelen worden toegepast om de uitvoeringen voor die elementen te genereren.

#### Profielen verwerken

Specificaties voor de uitvoering van elementen worden gedefinieerd in [Profielen verwerken](../../../assets/configuring/processing-profiles.md) door AEM Assets-beheerders.

Maak een verwerkingsprofiel of werk een verwerkingsprofiel bij en voeg renderdefinities toe voor de afbeeldingsformaten die vereist zijn voor de toepassing zonder kop. Uitvoeringen kunnen een willekeurige naam hebben, maar moeten een semantische naam hebben.

![AEM geoptimaliseerde uitvoeringen zonder koptekst](./assets/images/processing-profiles.png)

In dit voorbeeld worden drie uitvoeringen gemaakt:

| Naam van vertoning | Extensie | Max. breedte |
|-----------------------|:---------:|----------:|
| web-optimized-large | webben | 1200 px |
| web-optimized-medium | webben | 900 px |
| web-optimized-small | webben | 600 px |

De kenmerken die in de bovenstaande tabel worden genoemd, zijn belangrijk:

+ __Naam van vertoning__ wordt gebruikt om de vertoning aan te vragen.
+ __Extensie__ is de extensie die wordt gebruikt om de __naam van vertoning__. Voorkeur `webp` uitvoeringen zoals deze zijn geoptimaliseerd voor weergave op het web.
+ __Max. breedte__ wordt gebruikt om de ontwikkelaar te informeren welke vertoning moet worden gebruikt op basis van het gebruik ervan in de headless toepassing.

De definities van de vertoning hangen van de behoeften van uw toepassing zonder kop af, zodat zorg ervoor om de optimale vertoning te bepalen die voor uw gebruiksgeval wordt geplaatst en semantisch over hoe zij worden gebruikt genoemd.

#### Elementen opnieuw verwerken{#reprocess-assets}

Als het verwerkingsprofiel is gemaakt (of bijgewerkt), verwerkt u de elementen opnieuw om de nieuwe uitvoeringen te genereren die zijn gedefinieerd in het verwerkingsprofiel. Nieuwe uitvoeringen bestaan pas als de elementen met het verwerkingsprofiel zijn verwerkt.

+ Bij voorkeur [Het verwerkingsprofiel is toegewezen aan een map](../../../assets/configuring//processing-profiles.md) Alle nieuwe elementen die naar deze map zijn geüpload, genereren dus automatisch de uitvoeringen. Bestaande activa moeten opnieuw worden verwerkt volgens de hierna beschreven ad-hocaanpak.

+ Of, ad hoc, door een omslag of een middel te selecteren, het selecteren __Elementen opnieuw verwerken__ en selecteert u de nieuwe naam van het verwerkingsprofiel.

   ![Ad-hocherverwerkingsmiddelen](./assets/images/ad-hoc-reprocess-assets.jpg)

#### Uitvoeringen bekijken

Uitvoeringen kunnen worden gevalideerd door [de weergave Uitvoeringen van een element openen](../../../assets/authoring/renditions.md)en selecteert u de nieuwe uitvoeringen die u wilt voorvertonen in de renditions rail. Als de uitvoeringen ontbreken, [ervoor zorgen dat de elementen worden verwerkt met behulp van het verwerkingsprofiel](#reprocess-assets).

![Uitvoeringen bekijken](./assets/images/review-renditions.png)

#### Elementen publiceren

Zorg ervoor dat de elementen met de nieuwe uitvoeringen [(opnieuw) gepubliceerd](../../../assets/sharing/publish.md) zodat de nieuwe uitvoeringen toegankelijk zijn in AEM Publish.

### Toegang tot uitvoeringen

Uitvoeringen zijn rechtstreeks toegankelijk door het toevoegen van de __vertoningsnamen__ en __renditie-extensies__ gedefinieerd in het verwerkingsprofiel naar de URL van het element.

| Element-URL | Subpad van uitvoeringen | Naam van vertoning | Vertoningsextensie |  | URL van vertoning |
|-----------|:------------------:|:--------------:|--------------------:|:--:|---|
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr_content/renditions/ | web-optimized-large | .webp | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/web-optimized-large.webp |
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr_content/renditions/ | web-optimized-medium | .webp | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/web-optimized-medium.webp |
| https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg | /_jcr_content/renditions/ | web-optimized-small | .webp | → | https://publish-p123-e789.adobeaemcloud.com/content/dam/example.jpeg/_jcr_content/renditions/web-optimized-small.webp |

{style=&quot;table-layout:auto&quot;}

### GraphQL-query{#renditions-graphl-query}

AEM GraphQL heeft extra syntaxis nodig voor het aanvragen van afbeeldingsuitvoeringen. In plaats daarvan [afbeeldingen worden gevraagd](#images-graphql-query) op de gebruikelijke manier en de gewenste uitvoering wordt in de code opgegeven. Het is belangrijk dat [ervoor zorgen dat afbeeldingselementen die door de toepassing zonder kop worden gebruikt, dezelfde rendities hebben](#reprocess-assets).

### Voorbeeld Reageren

Laten we een eenvoudige React-toepassing maken die drie uitvoeringen van één afbeelding weergeeft, web-geoptimaliseerd-klein, web-geoptimaliseerd-medium en web-geoptimaliseerd-groot.

![Uitvoeringen afbeeldingselementen Reageren, voorbeeld](./assets/images/react-example-renditions.jpg)

#### Afbeeldingscomponent maken{#react-example-image-component}

Maak een React-component waarmee de afbeeldingen worden gerenderd. Deze component accepteert vier eigenschappen:

+ `assetUrl`: De URL van het afbeeldingselement zoals deze wordt opgegeven in het antwoord van de GraphQL-query.
+ `renditionName`: De naam van de vertoning die moet worden geladen.
+ `renditionExtension`: De extensie van de vertoning die moet worden geladen.
+ `alt`: De alt-tekst voor de afbeelding; toegankelijkheid is belangrijk .

Deze component construeert het [URL van uitvoering met de indeling die wordt beschreven in __Toegang tot uitvoeringen__](#access-renditions). An `onError` De handler is ingesteld om het oorspronkelijke element weer te geven als de vertoning ontbreekt.

In dit voorbeeld wordt de URL van het oorspronkelijke element gebruikt als fallback in het dialoogvenster `onError` in de gebeurtenis ontbreekt een vertoning.

```javascript
// src/Image.js

export default function Image({ assetUrl, renditionName, renditionExtension, alt }) {
  // Construct the rendition Url in the format:
  //   <ASSET URL>/_jcr_content/renditions<RENDITION NAME>.<RENDITION EXTENSION>
  const renditionUrl = `${assetUrl}/_jcr_content/renditions/${renditionName}.${renditionExtension}`;

  // Load the original image asset in the event the named rendition is missing
  const handleOnError = (e) => { e.target.src = assetUrl; }

  return (
    <>
      <img src={renditionUrl} 
            alt={alt} 
            onError={handleOnError}/>
    </>
  );
}
```

#### Definieer de `App.js`{#app-js}

Eenvoudig `App.js` vraagt AEM voor een beeld van het Avontuur, en toont dan de drie vertoningen van dat beeld: web-optimized-small, web-optimized-medium, en web-optimized-large.

Het vragen tegen AEM wordt uitgevoerd in de haak van de douane React [useAdventureByPath die de AEM Headless SDK gebruikt](./aem-headless-sdk.md#graphql-persisted-queries).

De resultaten van de query en de specifieke renderingsparameters worden doorgegeven aan de [Component Image React](#react-example-image-component).

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'
import Image from "./Image";

function App() {

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  let { data, error } = useAdventureByPath("/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp");

  // Wait for GraphQL to provide data
  if (!data) { return <></> }

  return (
    <div className="app">
      
      <h2>Small rendition</h2>
      {/* Render the web-optimized-small rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="web-optimized-small"
        renditionExtension="webp"
        alt={data.adventureByPath.item.title}
      />

      <hr />

      <h2>Medium rendition</h2>
      {/* Render the web-optimized-medium rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="web-optimized-medium"
        renditionExtension="webp"
        alt={data.adventureByPath.item.title}
      />

      <hr />

      <h2>Large rendition</h2>
      {/* Render the web-optimized-large rendition for the Adventure Primary Image */}
      <Image
        assetUrl={data.adventureByPath.item.primaryImage._publishUrl}
        renditionName="web-optimized-large"
        renditionExtension="webp"
        alt={data.adventureByPath.item.title}
      />
    </div>
  );
}

export default App;
```
