---
title: Server-naar-server Node.js app - AEM Voorbeeld zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze server-zijtoepassing Node.js toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted query's.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 1%

---

# Server-naar-server Node.js-app

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze server-aan-server toepassing toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen en druk het op terminal.

![Server-naar-server Node.js-app met AEM Headless](./assets/server-to-server-app/server-to-server-app.png)

De weergave van [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM

De toepassing Node.js werkt met de volgende AEM plaatsingsopties. Alle implementaties vereisen de [WKND-site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) te installeren.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Optioneel [servicegegevens](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) als u aanvragen autoriseert (bijvoorbeeld verbinding maken met de AEM Author-service).

Deze Node.js-toepassing kan verbinding maken met AEM-auteur of AEM-publicatie op basis van de opdrachtregelparameters.

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

   Zo kunt u de app bijvoorbeeld zonder toestemming uitvoeren op AEM Publish:

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   De app uitvoeren op AEM-auteur met toestemming:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. Een JSON-lijst met avonturen van de WKND-referentiesite moet in de terminal worden afgedrukt.

## De code

Hieronder volgt een overzicht van hoe de server-aan-server toepassing Node.js wordt gebouwd, hoe het met AEM Headless verbindt om inhoud terug te winnen gebruikend GraphQL voortgeduurde vragen, en hoe dat gegeven wordt voorgesteld. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app).

De meest gebruikte methode voor server-naar-server-AEM Headless-toepassingen is het synchroniseren van gegevens van inhoudsfragmenten van AEM naar andere systemen. Deze toepassing is echter opzettelijk eenvoudig en drukt de JSON-resultaten af van de hardnekkige query.

### Blijvende query&#39;s

Na AEM Beste praktijken zonder hoofd, gebruikt de toepassing AEM GraphQL voortgeduurde vragen om avontuurgegevens te vragen. De toepassing gebruikt twee voortgeduurde vragen:

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


### Vraag GrafiekQL blijft uitvoeren

AEM voortgeduurde vragen worden uitgevoerd over de GET van HTTP en zo, [AEM Headless-client voor Node.js](https://github.com/adobe/aem-headless-client-nodejs) wordt gebruikt om [Voer de voortgezette vragen GraphQL uit](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) tegen AEM en wint de adventure inhoud terug.

De voortgezette vraag wordt aangehaald door te roepen `aemHeadlessClient.runPersistedQuery(...)`, en het overgaan van de persisted GraphQL vraagnaam. Zodra GraphQL de gegevens terugkeert, ga het tot vereenvoudigd over `doSomethingWithDataFromAEM(..)` -functie, die de resultaten afdrukt, maar doorgaans de gegevens naar een ander systeem verzendt, of enige uitvoer genereert op basis van de opgehaalde gegevens.

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