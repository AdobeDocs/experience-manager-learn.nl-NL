---
title: AEM-inhoudsbewerkingen versnellen met de Content MCP-server
description: Leer hoe u de AEM Content MCP Server van uw voorkeursIDE op AI-basis gebruikt, zoals Cursor, om uw AEM-inhoudsbewerkingen te stroomlijnen en te versnellen, de handmatige inspanning te verminderen en de productiviteit te verhogen.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: tutorial
duration: null
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20474
exl-id: 843209cb-2f31-466c-b5b1-a9fb26965bc0
source-git-commit: 6a0eb6e8f5fa9d7152f46d6b8054dc89ff656507
workflow-type: tm+mt
source-wordcount: '850'
ht-degree: 0%

---

# Bewerkingen voor AEM-inhoud versnellen met de Content MCP-server

Gebruik de **Server MCP van de Inhoud** van een AI-Gedreven winde zoals [ winde van de Curseur ](https://www.cursor.com/) om met de inhoud van AEM in natuurlijke taal, geen laag-niveau API code of navigatie UI te werken.

In dit leerprogramma herziet u _de fragmentdetails van de Adventure-inhoud,_ update _een fragment (bijvoorbeeld, de prijs van een avontuur), en_ verifieert _de verandering in de_ WKND avonturen Reageren app [ allen van uw winde tegen a ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) lagere milieu van AEM _(RDE of Ontwikkeling) zonder de stroom te verlaten MCP._

>[!VIDEO](https://video.tv.adobe.com/v/3480895/?learn=on&enablevpops)

## Overzicht

AEM as a Cloud Service verstrekt _MCP Servers_ zodat kan uw winde of praatjeapp veilig met AEM werken. De **Server MCP van de Inhoud** steunt pagina&#39;s, fragmenten, en activa. Zie [ MCP Servers in AEM ](./overview.md) voor meer informatie.

## Hoe ontwikkelaars het kunnen gebruiken

Verbind [ winde van de Curseur ](https://www.cursor.com/) met de Server van Inhoud MCP en stel het scenario hieronder in werking.

### Setup - Content MCP Server in Cursor

Opstelling de Server MCP van de Inhoud in Curseur met deze stappen.

1. Open de cursor op uw computer.

1. Ga naar **Montages** > **de Montages van de Curseur** van het menu van de Curseur om het montagevenster te openen.
   ![ de Montages van de Curseur ](../assets/content-mcp-server/cursor-settings.png)

1. In linkerzijbalk, klik **Hulpmiddelen &amp; MCP** om dat paneel te openen.
   ![ Hulpmiddelen &amp; MCP ](../assets/content-mcp-server/tools-mcp.png)

1. Klik **Voeg Douane MCP** of **Nieuwe Server MCP** toe om `mcp.json` te openen, dan deeg in deze configuratie:

   ```json
   {
       "mcpServers": {
           // Use this for create, read, update, and delete operations
           "AEM-RDE-Content": {
               "url": "https://mcp.adobeaemcloud.com/adobe/mcp/content"
           },
           //Use this for read-only operations
           "AEM-RDE-Content-Read-Only": {
               "url": "https://mcp.adobeaemcloud.com/adobe/mcp/content-readonly"
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > Voor leerprogramma doel, voegt de bovengenoemde configuratie zowel **Inhoud** als **Inhoud (read-only)** voor dit leerprogramma toe. In praktijk, **omvat de Inhoud** reeds alles **Inhoud (read-only)** aanbiedingen, plus creeer/werk/schrap hulpmiddelen.
   >
   >
   > Als u om het even welke mogelijkheid wilt vermijden om, inhoud tot stand te brengen te wijzigen of te schrappen, slechts **Inhoud (read-only) vormen** (`/content-readonly`) en **Inhoud** weglaten (`/content`). Op die manier vermijdt u onbedoelde veranderingen.

   ![ voeg de Server van AEM MCP ](../assets/content-mcp-server/mcp-json-file.png) toe

1. Van het venster van de Montages van de Curseur, klik **verbinden** om het authentificatieproces in werking te stellen. Het gebruikt de stroom van OAuth 2.0 PKCE om het **Token van de Toegang van de Gebruiker Specifieke** te krijgen om tot de Server van AEM toegang te hebben MCP.
   ![ verbind met de Server van AEM MCP ](../assets/content-mcp-server/connect-to-aem-mcp-server.png)

1. Meld u aan met uw Adobe ID en kom vervolgens terug naar het venster Cursorinstellingen.
   ![ Login met Adobe ID ](../assets/content-mcp-server/login-with-adobe-id.png)

1. Bevestig dat **AEM-RDE-Content-Read-Only** en **AEM-RDE-Inhoud** als verbonden tonen. U kunt elke server uitbreiden om zijn hulpmiddelen te zien.

   ![ de Servers van AEM MCP ](../assets/content-mcp-server/connected-aem-mcp-servers.png)

### Setup - WKND Adventures React App

Daarna, opstelling de [ WKND Adventures React App ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) in Cursor.

1. Deze twee reacties klonen op uw computer:

   ```bash
   ## WKND GraphQL repo, the `react-app` folder is the WKND Adventures app
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   
   ## WKND Site repo, you deploy this to RDE so the app can use its content fragments data via GraphQL
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Stel het [ 1} project van de Plaats WKND {aan uw RDE op. ](https://github.com/adobe/aem-guides-wknd) Voor gedetailleerde stappen, zie [ hoe te om het Snelle Milieu van de Ontwikkeling ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use#deploy-aem-artifacts-using-the-aem-rde-plugin) te gebruiken.

1. Open de map `react-app` in uw IDE.

1. Bewerken `.env.development` en instellen:
   - `REACT_APP_HOST_URI`: de URL van uw RDE-auteur
   - `REACT_APP_AUTH_METHOD`: moet `basic`
   - `REACT_APP_BASIC_AUTH_USER` en `REACT_APP_AEM_AUTH_PASSWORD` : moet `aem-headless` zijn (maak deze gebruiker in RDE en voeg deze toe aan de `administrators` -groep)

1. Van de terminal van winde, looppas:

   ```bash
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. In uw browser, ga naar [ http://localhost :3000 ](http://localhost:3000) om WKND te bekijken avonturen app.

   ![ Reageer app - WKND avonturen ](../assets/content-mcp-server/react-app-wknd-adventures.png)

### Productiviteitsscenario - AEM Content review en update

Veronderstel u a _HOT_ banner van de VERKLARING van de VERKLARING moet tonen {wanneer een eenvoudige regel wordt voldaan. De gebruikelijke aanpak zou zijn:

- Bekijk de de componentencode van Adventure cards
- De logica toevoegen voor wanneer de banner moet worden weergegeven
- Het fragmentmodel voor Adventure-inhoud in AEM controleren
- Wijzig een of meer eigenschappen van het Adventure-fragment om de regel te testen

Om dingen eenvoudig te houden, laten we de _HOT DEAL_ banner tonen wanneer de prijs van het avontuur onder $100 is.

Omdat React app zijn gegevens van uw milieu van RDE krijgt, moet u het model van het het fragmentfragment van de inhoud van het Avontuur kennen en dan de juiste fragmenteigenschappen bijwerken. Dat is precies wat de AEM Content MCP Server kan helpen. Zo ziet het eruit.

1. Open in Cursor een nieuwe chat en typ:

   ```text
   I want to review my Content Fragment Models from AEM RDE, can you list the Adventure Content Fragment details.
   ```

   ![ Modellen van het Fragment van de Inhoud van het Overzicht ](../assets/content-mcp-server/review-content-fragment-models-prompt-response.png)


   Alvorens de Server van Inhoud MCP aan te halen, vraagt het om bevestiging om te werk te gaan. Zo blijft u de inhoudsbewerkingen beheren.

   AI gebruikt de Server van de Inhoud MCP om de gegevens te halen en dan het op een duidelijke, gestructureerde manier voor te stellen. Het bevat details van het inhoudsfragmentmodel, het aantal fragmenten en samenvattingsgegevens.

1. Om _HOT te teweegbrengen DEAL_ banner, update één prijs van het avontuur. Probeer in dezelfde chat:

   ```text
   Can you update adventure Beervana in Portland's price to 99.99
   ```

   ![ Prijs van het Adventure van de Update ](../assets/content-mcp-server/update-adventure-price-prompt-response.png)

   Op dezelfde manier vraagt de AI om bevestiging om te werk te gaan alvorens de inhoud bij te werken. Hierin wordt ook een overzicht gegeven van de inhoudsbewerking voor en na de update.

1. In React app, bevestig dat de kaart Beervana nu de _BALANS VAN DE HOT_ banner toont.

   ![ verifieer HOT DEAL Banner ](../assets/content-mcp-server/verify-hot-deal-banner.png)

### Aanvullende vragen

Probeer deze inhoud geconcentreerde herinneringen in uw winde (met de Verbonden Server van de Inhoud MCP) om meer werkschema&#39;s en eigenschappen te onderzoeken.

- Inhoud detecteren:

  ```text
  List all content fragments in the WKND Adventures folder
  
  List all WKND Site pages from US English site
  
  Can you give me page metadata for Tahoe Skiing English page? 
  
  List assets of Bali Surf camp
  
  What Content Fragment models are available in this environment?
  ```

- Inhoud zoeken:

  ```text
  Search for content fragments that mention 'cycling'
  
  Do we have a magazine page in US English site with "Camping" in it
  ```

- Inhoud bijwerken:

  ```text
  In WKND US English create a copy of Downhill Skiing Wyoming as "Test Downhill Skiing Wyoming"
  
  In newly created "Test Downhill Skiing Wyoming" please change title to "Duplicated Page"
  ```

- Publiceren of publicatie ongedaan maken:

  ```text
  Can you publish the page at /us/en/adventures/test-downhill-skiing-wyoming and give me publish page URL
  
  Can you unpublish the test-downhill-skiing-wyoming page
  ```

## Samenvatting

U stelt de AEM Content MCP Server in Cursor in en koppelt deze aan uw RDE (of Development)-omgeving. Vervolgens hebt u de React-app voor WKND-avonturen gebruikt en in de natuurlijke taal gebabeld om de fragmentdetails van Adventure-inhoud te bekijken. U hebt de prijs van een fragment ook bijgewerkt met de vraag van de AI om bevestiging vóór elke inhoudsbewerking. U hebt de wijziging in de actieve app geverifieerd. U kunt de zelfde mens-centric stroom van uw winde gebruiken om, AEM inhoud te herzien bij te werken en tot stand te brengen zonder op de UI van AEM over te schakelen of laag-vlakke API code te schrijven.
