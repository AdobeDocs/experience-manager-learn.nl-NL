---
title: Server-naar-server Node.js app - AEM Voorbeeld zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze server-side toepassing Node.js toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM zonder hoofd as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 145
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Server-naar-server Node.js-app

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze server-aan-server toepassing toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen en het op terminal te drukken.

![Server-naar-server Node.js-app met AEM Headless](./assets/server-to-server-app/server-to-server-app.png)

De weergave [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [Node.js v18](https://nodejs.org/en)
+ [Git](https://git-scm.com/)

## AEM

De toepassing Node.js werkt met de volgende AEM plaatsingsopties. Alle implementaties vereisen de [WKND-site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) te installeren.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Optioneel [servicegegevens](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) als u aanvragen autoriseert (bijvoorbeeld verbinding maken met AEM Auteur-service).

Deze toepassing Node.js kan met AEM Auteur verbinden of AEM publiceren die op de bevel-lijn parameters wordt gebaseerd.

## Hoe wordt het gebruikt

1. Klonen met `adobe/aem-guides-wknd-graphql` opslagplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Open een terminal en voer de opdrachten uit:

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. De app kan worden uitgevoerd met de opdracht:

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   Zo kunt u de app bijvoorbeeld uitvoeren op AEM Publiceren zonder toestemming te geven:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   De app uitvoeren tegen AEM auteur met toestemming:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Een JSON-lijst met avonturen van de WKND-referentiesite moet in de terminal worden afgedrukt.

## De code

Hieronder volgt een overzicht van hoe de server-aan-server toepassing Node.js wordt gebouwd, hoe het met AEM Headless verbindt om inhoud terug te winnen gebruikend GraphQL persisted vragen, en hoe die gegevens worden voorgesteld. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

De meest gebruikte methode voor server-naar-server-AEM Headless-toepassingen is het synchroniseren van gegevens van inhoudsfragmenten van AEM naar andere systemen. Deze toepassing is echter opzettelijk eenvoudig en drukt de JSON-resultaten af van de hardnekkige query.

### Blijvende query&#39;s

Na AEM Beste praktijken zonder hoofd, gebruikt de toepassing AEM GraphQL persisted query&#39;s om avontuurgegevens te vragen. De toepassing gebruikt twee voortgeduurde vragen:

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

### AEM headless-client maken

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### GraphQL-query uitgevoerd

AEM voortgeduurde vragen worden uitgevoerd over de GET van HTTP en zo, [AEM Headless-client voor Node.js](https://github.com/adobe/aem-headless-client-nodejs) wordt gebruikt om [Geef de doorlopende GraphQL-query&#39;s op](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) tegen AEM en wint de adventure inhoud terug.

De voortgezette vraag wordt aangehaald door te roepen `aemHeadlessClient.runPersistedQuery(...)`en geeft u de naam van de aanhoudende GraphQL-query door. Wanneer de GraphQL de gegevens heeft geretourneerd, geeft u deze door aan de vereenvoudigde procedure `doSomethingWithDataFromAEM(..)` -functie, die de resultaten afdrukt, maar doorgaans de gegevens naar een ander systeem verzendt, of enige uitvoer genereert op basis van de opgehaalde gegevens.

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
