---
title: Reageren toepassing - Voorbeeld AEM zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze React toepassing toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 4b8f7d9c6def8c121f3c975218b7214d271fa77f
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 0%

---

# App Reageren

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze React toepassing toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen. De AEM Headless-client voor JavaScript wordt gebruikt om de aanhoudende query&#39;s van GraphQL uit te voeren die de toepassing van stroom voorzien.

![Reageer app met AEM headless](./assets/react-app/react-app.png)

De weergave van [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [stapsgewijze zelfstudie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html) beschrijf hoe deze React-app is gemaakt.

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [11 JDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM

De React toepassing werkt met de volgende AEM plaatsingsopties. Alle implementaties vereisen de [WKND-site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) te installeren.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Lokale instelling met [de SDK van AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

De React toepassing wordt ontworpen om met een __AEM-publicatie__ milieu, nochtans kan het inhoud van auteur AEM als de authentificatie in de configuratie van de toepassing van React wordt verstrekt.

## Hoe wordt het gebruikt

1. Klonen met `adobbe/aem-guides-wknd-graphql` opslagplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Bewerk de `aem-guides-wknd-graphql/react-app/.env.development` bestand en set `REACT_APP_HOST_URI` om naar uw AEM te wijzen.

   Werk de authentificatiemethode bij als het verbinden met een auteursinstantie.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/_cq_graphql/wknd-shared/endpoint.json
   
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

Hieronder volgt een overzicht van hoe de React toepassing wordt gebouwd, hoe het met AEM Headless verbindt om inhoud terug te winnen gebruikend GraphQL voortgeduurde vragen, en hoe dat gegeven wordt voorgesteld. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### Blijvende query&#39;s

Na AEM Beste praktijken zonder hoofd, gebruikt de React toepassing AEM GraphQL voortgeduurde vragen om avontuurgegevens te vragen. De toepassing gebruikt twee voortgeduurde vragen:

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

### Vraag GrafiekQL blijft uitvoeren

AEM voortgeduurde vragen worden uitgevoerd over de GET van HTTP en zo, [AEM headless client voor JavaScript](https://github.com/adobe/aem-headless-client-js) wordt gebruikt om [Voer de voortgezette vragen GraphQL uit](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) tegen AEM en laad de adventure-inhoud in de app.

Elke voortgezette query heeft een bijbehorende functie in `src/api/persistedQueries.js`, die asynchroon het eindpunt van de GET van AEM HTTP roept, en de avontuurgegevens terugkeert.

Elke functie roept op zijn beurt de `aemHeadlessClient.runPersistedQuery(...)`, die de voortgezette vraag GraphQL uitvoeren.

```js
// src/api/persistedQueries.js

/**
 * Queries a list of all Adventures using the persisted path "wknd-shared/adventures-all"
 * @returns {data, errors}
 */
export const getAllAdventures = async function() {
    return executePersistedQuery('wknd-shared/adventures-all');
}

...

/**
 * Uses the AEM Headless SDK to execute a query besed on a persistedQueryPath and optional query variables
 * @param {*} persistedQueryPath 
 * @param {*} queryVariables 
 * @returns 
 */
 const executePersistedQuery = async function(persistedQueryPath, queryVariables) {

    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

### Weergaven

De React toepassing gebruikt twee meningen om de avontuurgegevens in de Webervaring voor te stellen.

+ `src/components/Adventures.js`

   Oproepen `getAllAdventures()` van `src/api/persistedQueries.js`  en geeft de geretourneerde avonturen weer in een lijst.

+ `src/components/AdventureDetail.js`

   Roept de `getAdventureBySlug(..)` met de `slug` param werd via de avontuurselectie op de `Adventures` en geeft de details van één avontuur weer.


### Omgevingsvariabelen

Meerdere [omgevingsvariabelen](https://create-react-app.dev/docs/adding-custom-environment-variables) worden gebruikt om verbinding te maken met een AEM. Standaard maakt verbinding met AEM-publicatie die wordt uitgevoerd op `http://localhost:4503`. Als u de AEM verbinding wilt wijzigen, werkt u de `.env.development` bestand:

+ `REACT_APP_HOST_URI=http://localhost:4502`: Instellen op AEM doelhost
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: Plaats de de eindpuntweg GraphQL. Dit wordt niet gebruikt door deze React-app, omdat deze app alleen voortgezette query&#39;s gebruikt.
+ `REACT_APP_AUTH_METHOD=`: De voorkeursverificatiemethode. Optioneel wordt standaard geen verificatie gebruikt.
   + `service-token`: De Verantwoordelijkheden van de Dienst van het gebruik om een toegangstoken op AEM as a Cloud Service te verkrijgen
   + `dev-token`: Ontwikkelingstoken gebruiken voor lokale ontwikkeling op AEM as a Cloud Service
   + `basic`: Gebruiker/pas gebruiken voor lokale ontwikkeling met lokale AEM-auteur
   + Leeg laten om verbinding te maken met AEM zonder verificatie
+ `REACT_APP_AUTHORIZATION=admin:admin`: Stel basisverificatiegegevens in die u wilt gebruiken als u verbinding maakt met een AEM-auteuromgeving (alleen voor ontwikkeling). Als u verbinding maakt met een publicatieomgeving, is deze instelling niet nodig.
+ `REACT_APP_DEV_TOKEN`: Dev token string. Naast Basic-auth (user:pass) kunt u naast de externe instantie een verbinding maken met behulp van Betover-auth met DEV-token vanuit de Cloud-console
+ `REACT_APP_SERVICE_TOKEN`: Pad naar bestand met servicereferenties. Als u verbinding wilt maken met een externe instantie, kan verificatie ook worden uitgevoerd met het servicetoken (download het bestand vanuit de Developer Console).

### AEM

Bij gebruik van de webpack-ontwikkelingsserver (`npm start`) het project berust op een [proxyinstelling](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) gebruiken `http-proxy-middleware`. Het bestand is geconfigureerd op [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) en is gebaseerd op verschillende aangepaste omgevingsvariabelen die zijn ingesteld op `.env` en `.env.development`.

Als u verbinding maakt met een AEM auteursomgeving, gaat u naar [de authentificatiemethode moet worden gevormd](#environment-variables).

### Delen van bronnen van oorsprong (CORS)

Deze React toepassing baseert zich op een op AEM-Gebaseerde configuratie CORS die op het doel AEM milieu loopt en veronderstelt dat React app loopt op `http://localhost:3000` in de ontwikkelingsmodus. De [CORS-configuratie](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) maakt deel uit van de [WKND-site](https://github.com/adobe/aem-guides-wknd).

![CORS-configuratie](assets/react-app/cross-origin-resource-sharing-configuration.png)
