---
title: Reageren toepassing - Voorbeeld AEM zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze React-toepassing laat zien hoe u inhoud kunt zoeken met behulp van AEM GraphQL API's met behulp van doorlopende query's.
version: Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM zonder hoofd as a Cloud Service" before-title="false"
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '898'
ht-degree: 0%

---

# App Reageren{#react-app}

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze React-toepassing laat zien hoe u inhoud kunt zoeken met behulp van AEM GraphQL API&#39;s met behulp van doorlopende query&#39;s. De AEM Headless-client voor JavaScript wordt gebruikt om de GraphQL-query&#39;s uit te voeren die de toepassing blijven activeren.

![Reageer app met AEM headless](./assets/react-app/react-app.png)

De weergave [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [stapsgewijze zelfstudie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html) beschrijf hoe deze React-app is gemaakt.

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM

De React toepassing werkt met de volgende AEM plaatsingsopties. Alle implementaties vereisen de [WKND-site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) te installeren.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Lokale instelling met [de SDK van AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
   + Vereisten [11 JDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)

De React toepassing wordt ontworpen om met een __AEM publiceren__ milieu, nochtans kan het inhoud van AEM Auteur als de authentificatie in de configuratie van de toepassing van React wordt verstrekt.

## Hoe wordt het gebruikt

1. Klonen met `adobe/aem-guides-wknd-graphql` opslagplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bewerk de `aem-guides-wknd-graphql/react-app/.env.development` bestand en set `REACT_APP_HOST_URI` om naar uw AEM te wijzen.

   Werk de authentificatiemethode bij als het verbinden met een auteursinstantie.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. Open een terminal en voer de opdrachten uit:

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Een nieuw browservenster moet worden geladen op [http://localhost:3000](http://localhost:3000)
1. Op de toepassing moet een lijst met avonturen van de WKND-referentiesite worden weergegeven.

## De code

Hieronder volgt een overzicht van hoe de React toepassing wordt gebouwd, hoe het met AEM Headless verbindt om inhoud terug te winnen gebruikend GraphQL persisted query&#39;s, en hoe die gegevens worden voorgesteld. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Blijvende query&#39;s

Na AEM Beste praktijken zonder hoofd, gebruikt de toepassing React AEM GraphQL voortgeduurde vragen om avontuurgegevens te vragen. De toepassing gebruikt twee voortgeduurde vragen:

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

Elke voortgezette vraag heeft het overeenkomstige Reageren [useEffect](https://reactjs.org/docs/hooks-effect.html) haak in `src/api/usePersistedQueries.js`, die asynchroon roept de AEM GET van HTTP bleef vraageindpunt, en keert de avontuurgegevens terug.

Elke functie roept op zijn beurt de `aemHeadlessClient.runPersistedQuery(...)`, wordt de doorlopende GraphQL-query uitgevoerd.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}
```

### Weergaven

De React toepassing gebruikt twee meningen om de avontuurgegevens in de Webervaring voor te stellen.

+ `src/components/Adventures.js`

  Oproepen `getAdventuresByActivity(..)` van `src/api/usePersistedQueries.js` en geeft de geretourneerde avonturen weer in een lijst.

+ `src/components/AdventureDetail.js`

  Roept de `getAdventureBySlug(..)` met de `slug` param werd via de avontuurselectie op de `Adventures` en geeft de details van één avontuur weer.

### Omgevingsvariabelen

Meerdere [omgevingsvariabelen](https://create-react-app.dev/docs/adding-custom-environment-variables) worden gebruikt om verbinding te maken met een AEM. Standaard maakt verbinding met AEM publiceren die wordt uitgevoerd op `http://localhost:4503`. Werk de `.env.development` bestand, om de AEM verbinding te wijzigen:

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`: Instellen op AEM doelhost
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Stel het pad naar het eindpunt van GraphQL in. Dit wordt niet gebruikt door deze React-app, omdat deze app alleen voortgezette query&#39;s gebruikt.
+ `REACT_APP_AUTH_METHOD=`: De voorkeursverificatiemethode. Optioneel wordt standaard geen verificatie gebruikt.
   + `service-token`: Gebruik Service Credentials om een toegangstoken te verkrijgen op AEM as a Cloud Service
   + `dev-token`: Gebruik dev-token voor lokale ontwikkeling op AEM as a Cloud Service
   + `basic`: Gebruik gebruiker/pas voor lokale ontwikkeling met lokale AEM auteur
   + Leeg laten om verbinding te maken met AEM zonder verificatie
+ `REACT_APP_AUTHORIZATION=admin:admin`: Stel de basisverificatiereferenties in die moeten worden gebruikt als u verbinding maakt met een AEM Auteur-omgeving (alleen voor ontwikkeling). Als u verbinding maakt met een publicatieomgeving, is deze instelling niet nodig.
+ `REACT_APP_DEV_TOKEN`: Dev token string. Naast Basisverificatie (user:pass) kunt u naast verificatie op afstand ook Vierdere verificatie gebruiken met DEV-token van de Cloud-console. Dit probleem is nu opgelost.
+ `REACT_APP_SERVICE_TOKEN`: Pad naar bestand met servicereferenties. Als u verbinding wilt maken met een externe instantie, kan verificatie ook worden uitgevoerd met het servicetoken (download het bestand vanuit de Developer Console).

### AEM

Bij gebruik van de webpack-ontwikkelingsserver (`npm start`) het project berust op een [proxyinstelling](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) gebruiken `http-proxy-middleware`. Het bestand is geconfigureerd op [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) en is gebaseerd op verschillende aangepaste omgevingsvariabelen die zijn ingesteld op `.env` en `.env.development`.

Als u verbinding maakt met een AEM auteursomgeving, gaat u naar [verificatiemethode moet worden geconfigureerd](#environment-variables).

### Delen van bronnen van oorsprong (CORS)

Deze React toepassing baseert zich op een op AEM-Gebaseerde configuratie CORS die op het doel AEM milieu loopt en veronderstelt dat React app loopt op `http://localhost:3000` in de ontwikkelingsmodus.  Controleer de[AEM documentatie voor implementatie zonder hoofd](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html) voor meer informatie om CORS te installeren en te vormen.
