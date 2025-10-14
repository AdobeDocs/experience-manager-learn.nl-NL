---
title: Server-naar-server Node.js app - AEM Headless Voorbeeld
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze server-side toepassing Node.js toont hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 135
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# Server-naar-server Node.js-app

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze server-aan-server toepassing toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen en het op terminal te drukken.

![&#x200B; Server-aan-server Node.js app met AEM Headless &#x200B;](./assets/server-to-server-app/server-to-server-app.png)

Bekijk de [&#x200B; broncode op GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [&#x200B; Node.js v18 &#x200B;](https://nodejs.org/en)
+ [&#x200B; Git &#x200B;](https://git-scm.com/)

## AEM-vereisten

De toepassing Node.js werkt met de volgende AEM-implementatieopties. Alle plaatsingen vereisen de [&#x200B; Plaats van WKND v3.0.0+ &#x200B;](https://github.com/adobe/aem-guides-wknd/releases/latest) om worden geïnstalleerd.

+ [&#x200B; AEM as a Cloud Service &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=nl-NL)
+ Naar keuze, [&#x200B; de dienstgeloofsbrieven &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=nl-NL) als het machtigen van verzoeken (bijvoorbeeld, die met de dienst van de Auteur van AEM verbinden).

Deze Node.js-toepassing kan verbinding maken met AEM Author of AEM Publish op basis van opdrachtregelparameters.

## Hoe wordt het gebruikt

1. De gegevensopslagruimte `adobe/aem-guides-wknd-graphql` klonen:

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

   Zo kunt u de app bijvoorbeeld uitvoeren op AEM Publish zonder toestemming:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   De app uitvoeren tegen AEM Author met machtiging:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Een JSON-lijst met avonturen van de WKND-referentiesite moet in de terminal worden afgedrukt.

## De code

Hieronder volgt een overzicht van hoe de toepassing server-naar-server Node.js wordt gebouwd, hoe het met AEM Headless verbindt om inhoud terug te winnen gebruikend GraphQL persisted vragen, en hoe die gegevens worden voorgesteld. De volledige code kan op [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server) worden gevonden.

AEM Headless-apps voor servers worden vaak gebruikt voor het synchroniseren van gegevens van inhoudsfragmenten van AEM naar andere systemen. Deze toepassing is echter opzettelijk eenvoudig en drukt de JSON-resultaten af van de hardnekkige query.

### Blijvende query&#39;s

Na de beste praktijken van AEM Headless, gebruikt de toepassing AEM GraphQL voortgezette vragen om avontuurgegevens te vragen. De toepassing gebruikt twee voortgeduurde vragen:

+ `wknd/adventures-all` bleef query uitvoeren, die alle avonturen in AEM retourneert met een verkorte set eigenschappen. Deze hardnekkige vraag drijft de aanvankelijke lijst van het avontuur van de mening.

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

### AEM Headless-client maken

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

AEM voortgeduurde vragen worden uitgevoerd over HTTP GET en zo, wordt de [&#x200B; Hoofdloze cliënt van AEM voor Node.js &#x200B;](https://github.com/adobe/aem-headless-client-nodejs) gebruikt om [&#x200B; de voortgeduurde vragen van GraphQL &#x200B;](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) tegen AEM uit te voeren en de avontuurinhoud terug te winnen.

De voortgezette query wordt aangeroepen door `aemHeadlessClient.runPersistedQuery(...)` aan te roepen en de naam van de voortgezette GraphQL-query door te geven. Wanneer de GraphQL de gegevens heeft geretourneerd, geeft u deze door aan de vereenvoudigde functie `doSomethingWithDataFromAEM(..)` , die de resultaten afdrukt. De gegevens worden echter doorgaans naar een ander systeem verzonden of er wordt uitvoer gegenereerd op basis van de opgehaalde gegevens.

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
