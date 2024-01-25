---
title: Next.js - Voorbeeld AEM zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze toepassing Next.js toont hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted query's.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM zonder hoofd as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 264
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Next.js-app

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze toepassing Next.js toont hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted query&#39;s. De AEM Headless-client voor JavaScript wordt gebruikt om de GraphQL-query&#39;s uit te voeren die de toepassing blijven activeren.

![De app Next.js met AEM Headless](./assets/next-js/next-js.png)

De weergave [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM

De app Next.js werkt met de volgende AEM implementatieopties. Alle implementaties vereisen [WKND Shared v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) of [WKND-site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) in de AEM as a Cloud Service omgeving te installeren.

De volgende voorbeeldtoepassing is ontworpen om verbinding te maken met __AEM publiceren__ service.

### Vereisten AEM auteur

Next.js wordt ontworpen om met te verbinden __AEM publiceren__ en hebt toegang tot niet-beveiligde inhoud. Next.js kan worden gevormd om met AEM Auteur via te verbinden `.env` hieronder beschreven eigenschappen. Voor afbeeldingen die worden aangeboden door AEM auteur is verificatie vereist. De gebruiker die de Next.js-app opent, moet daarom ook zijn aangemeld bij AEM auteur.

## Hoe wordt het gebruikt

1. Klonen met `adobe/aem-guides-wknd-graphql` opslagplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bewerk de `aem-guides-wknd-graphql/next-js/.env.local` bestand en set `NEXT_PUBLIC_AEM_HOST` aan de AEM.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   Als u verbinding maakt met AEM service Auteur, moet de verificatie worden opgegeven omdat AEM service Auteur standaard is beveiligd.

   Een lokale AEM gebruiken `AEM_AUTH_METHOD=basic` en geef de gebruikersnaam en het wachtwoord op in het dialoogvenster `AEM_AUTH_USER` en `AEM_AUTH_PASSWORD` eigenschappen.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   Als u een [AEM token voor as a Cloud Service lokale ontwikkeling](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` en geef de volledige dev-tokenwaarde op in het dialoogvenster `AEM_AUTH_DEV_TOKEN` eigenschap.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. Bewerk de `aem-guides-wknd-graphql/next-js/.env.local` bestand en valideren  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` wordt ingesteld op het juiste AEM GraphQL-eindpunt.

   Wanneer u [WKND gedeeld](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) of [WKND-site](https://github.com/adobe/aem-guides-wknd/releases/latest), gebruikt u de `wknd-shared` GraphQL API-eindpunt.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. Open een bevelherinnering en begin volgende.js app gebruikend de volgende bevelen:

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. Een nieuw browservenster opent de toepassing Next.js op [http://localhost:3000](http://localhost:3000)
1. De app Next.js geeft een lijst met avonturen weer. Als u een avontuur selecteert, worden de details op een nieuwe pagina weergegeven.

## De code

Hieronder volgt een overzicht van de manier waarop de toepassing Next.js is gemaakt, hoe deze verbinding maakt met AEM Headless om inhoud op te halen met behulp van GraphQL persisted query&#39;s en hoe deze gegevens worden gepresenteerd. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### Blijvende query&#39;s

Na AEM Beste praktijken zonder hoofd, gebruikt de toepassing Next.js AEM GraphQL voortgezette vragen om avontuurgegevens te vragen. De toepassing gebruikt twee doorlopende query&#39;s:

+ `wknd/adventures-all` persisted query, die alle avonturen in AEM met een verkorte set eigenschappen retourneert. Deze hardnekkige vraag drijft de aanvankelijke lijst van het avontuur van de mening.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug` persistente query, die één avontuur retourneert door `slug` (een douanebezit dat uniek een avontuur identificeert) met een volledige reeks eigenschappen. Dit bleef vraagbevoegdheden de meningen van het avontuurdetail.

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### GraphQL-query uitgevoerd

AEM voortgeduurde vragen worden uitgevoerd over de GET van HTTP en zo, [AEM headless client voor JavaScript](https://github.com/adobe/aem-headless-client-js) wordt gebruikt om [Geef de doorlopende GraphQL-query&#39;s op](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) tegen AEM en laad de adventure-inhoud in de app.

Elke voortgezette query heeft een bijbehorende functie in `src/lib//aem-headless-client.js`, dat het eindpunt van AEM GraphQL roept, en de avontuurgegevens terugkeert.

Elke functie roept op zijn beurt de `aemHeadlessClient.runPersistedQuery(...)`, wordt de doorlopende GraphQL-query uitgevoerd.

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### Pagina&#39;s

De app Next.js gebruikt twee pagina&#39;s om de avontuurgegevens te presenteren.

+ `src/pages/index.js`

  Gebruiksmiddelen [getServerSideProps() van Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) om te bellen `getAllAdventures()` en geeft elk avontuur als kaart weer.

  Het gebruik van `getServerSiteProps()` staat voor Server-zij Rendering van deze pagina Next.js toe.

+ `src/pages/adventures/[...slug].js`

  A [Next.js Dynamic Route](https://nextjs.org/docs/routing/dynamic-routes) dat de details van één enkel avontuur toont. Deze dynamische route prefetches de gegevens van elk avontuur gebruikend [getStaticProps() van Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) via een oproep aan `getAdventureBySlug(slug, queryVariables)` met de `slug` param werd via de avontuurselectie op de `adventures/index.js` pagina, en `queryVariables` om de indeling, breedte en kwaliteit van de afbeelding te bepalen.

  De dynamische route kan de details voor alle avonturen vooraf halen door te gebruiken [getStaticPaths() van Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) en het bevolken van alle mogelijke routepermutaties die op de volledige lijst van avonturen worden gebaseerd die door de vraag van GraphQL zijn teruggekeerd  `getAdventurePaths()`

  Het gebruik van `getStaticPaths()` en `getStaticProps(..)` stond de Statische Generatie van de Plaats van deze pagina&#39;s toe Next.js.

## Implementatieconfiguratie

Voor Next.js-apps, met name in de context van server-side rendering (SSR) en server-side generation (SSG), zijn geen geavanceerde beveiligingsconfiguraties vereist, zoals Cross-origin Resource Sharing (CORS).

Nochtans, als Next.js HTTP- verzoeken aan AEM van de context van de cliënt doet, kunnen de veiligheidsconfiguraties in AEM worden vereist. Controleer de [Zelfstudie over de implementatie van apps voor één pagina zonder hoofd AEM](../deployment/spa.md) voor meer informatie .
