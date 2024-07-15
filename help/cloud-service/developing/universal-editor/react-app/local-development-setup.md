---
title: Instelling voor lokale ontwikkeling
description: Leer hoe u een lokale ontwikkelomgeving instelt voor de Universal Editor, zodat u de inhoud van een voorbeeld van de React-app kunt bewerken.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 189
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 47bef697-5253-493a-b9f9-b26c27d2db56
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 0%

---

# Instelling voor lokale ontwikkeling

Leer hoe u een lokale ontwikkelomgeving instelt voor het bewerken van de inhoud van een React-app met de AEM Universal Editor.

## Vereisten

U hebt het volgende nodig om deze zelfstudie te volgen:

- HTML- en JavaScript-basisvaardigheden.
- De volgende gereedschappen moeten lokaal zijn geïnstalleerd:
   - [ Node.js ](https://nodejs.org/en/download/)
   - [ Git ](https://git-scm.com/downloads)
   - Een winde of coderedacteur, zoals [ Code van Visual Studio ](https://code.visualstudio.com/)
- Download en installeer het volgende:
   - [ SDK van AEM as a Cloud Service ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk): Het bevat QuickStart Jar die wordt gebruikt om AEM Auteur en Publish plaatselijk voor ontwikkelingsdoeleinden in werking te stellen.
   - [ Universele dienst van de Redacteur ](https://experienceleague.adobe.com/en/docs/experience-cloud/software-distribution/home): Een lokaal exemplaar van de Universele dienst van de Redacteur, heeft het een ondergroep van eigenschappen en kan van het portaal van de Distributie van de Software worden gedownload.
   - [ lokaal-ssl-volmacht ](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy): Een eenvoudige lokale SSL HTTP- volmacht die een zelf-ondertekend certificaat voor lokale ontwikkeling gebruikt. De AEM Universal Editor heeft de HTTPS-URL van de React-app nodig om deze in de editor te laden.

## Lokale instellingen

Voer de onderstaande stappen uit om de lokale ontwikkelomgeving in te stellen:

### AEM SDK

Installeer de volgende pakketten in de lokale AEM SDK om de inhoud voor de React-app van de WKND-teams op te geven.

- [ de Teams van WKND - het Pakket van de Inhoud ](./assets/basic-tutorial-solution.content.zip): Bevat de Modellen van het Fragment van de Inhoud, de Fragmenten van de Inhoud, en voortgeduurde vragen van GraphQL.
- [ de Teams van WKND - Pakket Config ](./assets/basic-tutorial-solution.ui.config.zip): bevat het Middel dat van de Cross-Origin (CORS) deelt en de Mantelbeveiliger van de Authentificatie van Symbolische. CORS vergemakkelijkt niet-AEM Web-eigenschappen om op browser-gebaseerde cliënt-zijvraag aan AEM GraphQL APIs te maken en de Symbolische Handler van de Authentificatie wordt gebruikt om elk verzoek aan AEM voor authentiek te verklaren.

  ![ de Teams van WKND - Pakketten ](./assets/wknd-teams-packages.png)

### Toepassingen Reageren

Voer de onderstaande stappen uit om de React-app voor WKND-teams in te stellen:

1. Kloon de [ WKND Teams Reageer app ](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) van de `basic-tutorial` oplossingstak.

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Navigeer naar de map `basic-tutorial` en open deze in de code-editor.

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. Installeer de afhankelijkheden en start de React-app.

   ```bash
   $ npm install
   $ npm start
   ```

1. Open de WKND Teams Reageer app in uw browser in [ http://localhost:3000 ](http://localhost:3000). Het toont een lijst van teamleden en hun details. De inhoud voor de React-app wordt geleverd door de lokale AEM SDK met behulp van GraphQL API&#39;s (`/graphql/execute.json/my-project/all-teams`), die u kunt verifiëren met het netwerktabblad van de browser.

   ![ WKND Teams - Reageer app ](./assets/wknd-teams-react-app.png)

### Universal Editor-service

Aan opstelling de **lokale** Universele dienst van de Redacteur, volg de stappen hieronder:

1. Download de recentste versie van de Universele dienst van de Redacteur van het [ Portaal van de Distributie van de Software ](https://experience.adobe.com/downloads).

   ![ Distributie van de Software - de Universele Dienst van de Redacteur ](./assets/universal-editor-service.png)

1. Pak het gedownloade ZIP-bestand uit en kopieer het `universal-editor-service.cjs` -bestand naar de nieuwe map `universal-editor-service` .

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. Maak een `.env` -bestand in de map `universal-editor-service` en voeg de volgende omgevingsvariabelen toe:

   ```bash
   # The port on which the Universal Editor service runs
   EXPRESS_PORT=8000
   # Disable SSL verification
   NODE_TLS_REJECT_UNAUTHORIZED=0
   ```

1. Start de lokale Universal Editor-service.

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

Met de bovenstaande opdracht start u de Universal Editor-service op poort `8000` en ziet u de volgende uitvoer:

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### Lokale SSL HTTP-proxy

De AEM Universal Editor vereist dat de React-app via HTTPS wordt aangeboden. Stel een lokale SSL HTTP-proxy in die een zelfondertekend certificaat gebruikt voor lokale ontwikkeling.

Voer de onderstaande stappen uit om de lokale SSL HTTP-proxy in te stellen en de AEM SDK en Universal Editor-service via HTTPS te bedienen:

1. Installeer het `local-ssl-proxy` -pakket globaal.

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. Start twee instanties van de lokale SSL HTTP-proxy voor de volgende services:

   - AEM lokale SSL HTTP-proxy van SDK op poort `8443` .
   - Universal Editor service local SSL HTTP proxy on port `8001` .

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### De React-app bijwerken voor het gebruik van HTTPS

Voer de onderstaande stappen uit om HTTPS voor de React-app voor WKND-teams in te schakelen:

1. Stop Reageren door `Ctrl + C` in de terminal te drukken.
1. Werk het bestand `package.json` bij om de omgevingsvariabele `HTTPS=true` in het script `start` op te nemen.

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. Werk `REACT_APP_HOST_URI` in het `.env.development` -bestand bij om het HTTPS-protocol en de lokale SSL HTTP-proxypoort van de AEM SDK te gebruiken.

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. Werk het `../src/proxy/setupProxy.auth.basic.js` -bestand bij als u ontspannen SSL-instellingen wilt gebruiken met de optie `secure: false` .

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. Start de React-app.

   ```bash
   $ npm start
   ```

## De installatie controleren

Nadat u de lokale ontwikkelomgeving hebt ingesteld met de bovenstaande stappen, controleren we de installatie.

### Lokale verificatie

Controleer of de volgende services lokaal via HTTPS worden uitgevoerd. Mogelijk moet u de beveiligingswaarschuwing in de browser voor het zelfondertekende certificaat accepteren:

1. WKND de Teams Reageren app op [ https://localhost:3000](https://localhost:3000)
1. AEM SDK op [ https://localhost:8443](https://localhost:8443)
1. De universele dienst van de Redacteur op [ https://localhost:8001](https://localhost:8001)

### WKND-teams laden Reageren app in Universal Editor

Laad de React-app voor de WKND-teams in de Universal Editor om de installatie te controleren:

1. Open de Universal Editor https://experience.adobe.com/#/aem/editor in uw browser. Meld u aan met uw Adobe ID als u hierom wordt gevraagd.

1. Voer in het invoerveld URL van site van de Universal Editor de URL van de WKND-teams React-app in en klik op `Open` .

   ![ Universele Redacteur - Plaats URL ](./assets/universal-editor-site-url.png)

1. De WKND Teams Reageren app laadt in de Universele Redacteur **maar u kunt niet de inhoud nog uitgeven**. U moet de React-app gebruiken om het bewerken van inhoud in te schakelen met de Universal Editor.

   ![ Universele Redacteur - WKND de Teams Reageren app ](./assets/universal-editor-wknd-teams.png)


## Volgende stap

Leer hoe te [ instrument Reageren app om de inhoud ](./instrument-to-edit-content.md) uit te geven.
