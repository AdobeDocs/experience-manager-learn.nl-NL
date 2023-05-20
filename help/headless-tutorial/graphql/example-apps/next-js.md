---
title: Next.js - Voorbeeld AEM zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze toepassing Next.js toont hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted query's.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 0%

---

# Next.js-app

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze toepassing Next.js toont hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted query&#39;s. De AEM Headless-client voor JavaScript wordt gebruikt om de GraphQL-query&#39;s uit te voeren die de toepassing blijven activeren.

![De app Next.js met AEM Headless](./assets/next-js/next-js.png)

De weergave van [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM

De app Next.js werkt met de volgende AEM implementatieopties. Alle implementaties vereisen [WKND Shared v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) of [WKND-site v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) in de AEM as a Cloud Service omgeving te installeren.

De volgende voorbeeldtoepassing is ontworpen om verbinding te maken met __AEM-publicatie__ service.

### Vereisten voor AEM-auteur

Next.js wordt ontworpen om met te verbinden __AEM-publicatie__ en hebt toegang tot niet-beveiligde inhoud. Next.js kan worden gevormd om met AEM Auteur via te verbinden `.env` hieronder beschreven eigenschappen. Voor afbeeldingen die worden aangeboden door AEM-auteur is verificatie vereist. De gebruiker die de app Next.js opent, moet daarom ook worden aangemeld bij AEM-auteur.

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

   Als u verbinding maakt met de AEM Author-service, moet de verificatie worden opgegeven omdat de AEM Author-service standaard is beveiligd.

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
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` persistente query, die één avontuur retourneert door `slug` (een douanebezit dat uniek een avontuur identificeert) met een volledige reeks eigenschappen. Dit bleef vraagbevoegdheden de meningen van het avontuurdetail.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### Pagina&#39;s

De app Next.js gebruikt twee pagina&#39;s om de avontuurgegevens te presenteren.

+ `src/pages/index.js`

   Gebruiksmiddelen [getServerSideProps() van Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) om `getAllAdventures()` en geeft elk avontuur als kaart weer.

   Het gebruik van `getServerSiteProps()` staat voor Server-zij Rendering van deze pagina Next.js toe.

+ `src/pages/adventures/[...slug].js`

   A [Next.js Dynamic Route](https://nextjs.org/docs/routing/dynamic-routes) dat de details van één enkel avontuur toont. Deze dynamische route prefetches de gegevens van elk avontuur gebruikend [getStaticProps() van Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) via een oproep aan `getAdventureBySlug(..)` met de `slug` param werd via de avontuurselectie op de `adventures/index.js` pagina.

   De dynamische route kan de details voor alle avonturen vooraf halen door te gebruiken [getStaticPaths() van Next.js](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) en het bevolken van alle mogelijke routepermutaties die op de volledige lijst van avonturen worden gebaseerd die door de vraag van GraphQL zijn teruggekeerd  `getAdventurePaths()`

   Het gebruik van `getStaticPaths()` en `getStaticProps(..)` stond de Statische Generatie van de Plaats van deze pagina&#39;s toe Next.js.

## Implementatieconfiguratie

Voor Next.js-apps, met name in de context van server-side rendering (SSR) en server-side generation (SSG), zijn geen geavanceerde beveiligingsconfiguraties vereist, zoals Cross-origin Resource Sharing (CORS).

Nochtans, als Next.js HTTP- verzoeken aan AEM van de context van de cliënt doet, kunnen de veiligheidsconfiguraties in AEM worden vereist. Controleer de [Zelfstudie over de implementatie van apps voor één pagina zonder hoofd AEM](../deployment/spa.md) voor meer informatie .
