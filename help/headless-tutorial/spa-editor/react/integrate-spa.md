---
title: Een SPA integreren | Aan de slag met de AEM SPA Editor en Reageren
description: Begrijp hoe de broncode voor een Enige die Toepassing van de Pagina (SPA) in React wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals een webpack dev server, te gebruiken om SPA tegen het modelAPI van AEM JSON snel te ontwikkelen.
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
duration: 409
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1689'
ht-degree: 0%

---

# De SPA integreren {#developer-workflow}

{{spa-editor-deprecation}}

Begrijp hoe de broncode voor een Enige die Toepassing van de Pagina (SPA) in React wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals een webpack dev server, te gebruiken om SPA tegen het modelAPI van AEM JSON snel te ontwikkelen.

## Doelstelling

1. Begrijp hoe het project van het KUUROORD met AEM met cliënt-zijbibliotheken geïntegreerd is.
2. Leer hoe u een webpack-ontwikkelingsserver gebruikt voor speciale front-end ontwikkeling.
3. Onderzoek het gebruik van a **volmacht** en statisch **mock** dossier voor het ontwikkelen tegen AEM JSON model API.

## Wat u gaat maken

In dit hoofdstuk zult u verscheidene kleine veranderingen in het KUUROORD aanbrengen om te begrijpen hoe het met AEM wordt geïntegreerd.
Dit hoofdstuk zal een eenvoudige `Header` component aan het KUUROORD toevoegen. In het proces om deze **statische** `Header` component uit te bouwen worden verscheidene benaderingen aan de ontwikkeling van AEM SPA gebruikt.

![ Nieuwe Kopbal in AEM ](./assets/integrate-spa/final-header-component.png)

*het KUUROORD wordt uitgebreid om een statische `Header` component* toe te voegen

## Vereisten

Herzie het vereiste tooling en de instructies voor vestiging a [ lokale ontwikkelomgeving ](overview.md#local-dev-environment). Dit hoofdstuk is een voortzetting van [ creeer het hoofdstuk van het Project ](create-project.md), nochtans om langs allen te volgen u een werkend SPA-Toegelaten project van AEM nodig hebt.

## Integratiebenadering {#integration-approach}

Er zijn twee modules gemaakt als onderdeel van het AEM-project: `ui.apps` en `ui.frontend` .

De `ui.frontend` module is a [ webpack ](https://webpack.js.org/) project dat alle broncode van het KUUROORD bevat. Een meerderheid van de ontwikkeling en het testen van SPA wordt gedaan in het webpack project. Wanneer een productie bouwt wordt teweeggebracht, wordt het KUUROORD gebouwd en gecompileerd gebruikend webpack. De gecompileerde artefacten (CSS en Javascript) worden gekopieerd in de module `ui.apps` die dan aan runtime van AEM wordt opgesteld.

![ ui.frontend high-level architectuur ](assets/integrate-spa/ui-frontend-architecture.png)

*A high-level afbeelding van de integratie van het KUUROORD.*

De extra informatie over het front-end bouwt kan [ hier ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) worden gevonden.

## Inspecteer de integratie van SPA {#inspect-spa-integration}

Daarna, inspecteer de `ui.frontend` module om het KUUROORD te begrijpen dat door het [ archetype van het Project van AEM ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) auto-geproduceerd is.

1. Open uw AEM-project in de IDE van uw keuze. Dit leerprogramma zal [ winde van de Code van Visual Studio ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code) gebruiken.

   ![ VSCode - AEM WKND Project van het KUUROORD ](./assets/integrate-spa/vscode-ide-openproject.png)

1. Vouw de map `ui.frontend` uit en inspecteer deze. Het bestand openen `ui.frontend/package.json`

1. Onder `dependencies` ziet u verschillende koppelingen die verwant zijn aan `react` inclusief `react-scripts`

   `ui.frontend` is een React toepassing die op [ wordt gebaseerd Create React App ](https://create-react-app.dev/) of CRA voor kort. De versie `react-scripts` geeft aan welke versie van CRA wordt gebruikt.

1. Er zijn ook diverse afhankelijkheden die zijn voorafgegaan door `@adobe` :

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   De bovengenoemde modules maken omhoog de [ Redacteur JS SDK van AEM SPA ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) en verstrekken de functionaliteit om het mogelijk te maken om de Componenten van het KUUROORD aan de Componenten van AEM in kaart te brengen.

   Ook inbegrepen zijn [ de Componenten van AEM WCM - Reageer de implementatie van de Kern ](https://github.com/adobe/aem-react-core-wcm-components-base) en [ componenten WCM van AEM - de redacteur van de SPA - Reageer de implementatie van de Kern ](https://github.com/adobe/aem-react-core-wcm-components-spa). Dit is een set herbruikbare UI-componenten die worden toegewezen aan AEM-componenten die buiten de box vallen. Deze zijn ontworpen om ongewijzigd te worden gebruikt en worden vormgegeven om aan de behoeften van uw project te voldoen.

1. In het `package.json` -bestand zijn er verschillende `scripts` gedefinieerd:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Dit zijn standaard bouwt manuscripten die [ beschikbaar ](https://create-react-app.dev/docs/available-scripts) door Create React App worden gemaakt.

   Het enige verschil is de toevoeging van `&& clientlib` aan het `build` -script. Deze extra instructie is de oorzaak van het kopiëren van gecompileerde SPA in de `ui.apps` module als cliënt-zijbibliotheek tijdens een bouwstijl.

   De npm module [ aem-clientlib-generator ](https://github.com/wcm-io-frontend/aem-clientlib-generator) wordt gebruikt om dit te vergemakkelijken.

1. Controleer het bestand `ui.frontend/clientlib.config.js` . Dit configuratiedossier wordt gebruikt door [ aem-clientlib-generator ](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) om te bepalen hoe te om de cliëntbibliotheek te produceren.

1. Controleer het bestand `ui.frontend/pom.xml` . Dit dossier zet de `ui.frontend` omslag in a [ Gemaakt module ](https://maven.apache.org/guides/mini/guide-multiple-modules.html) om. Het `pom.xml` dossier is bijgewerkt om [ te gebruiken front-maven-plugin ](https://github.com/eirslett/frontend-maven-plugin) aan **test** en **bouwt** het KUUROORD tijdens een Gemaakt bouwt.

1. Inspecteer het bestand `index.js` op `ui.frontend/src/index.js` :

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js` is het ingangspunt van het KUUROORD. `ModelManager` wordt geleverd door de AEM SPA Editor JS SDK. Het is verantwoordelijk voor het aanroepen en injecteren van `pageModel` (de JSON-inhoud) in de toepassing.

1. Inspecteer het bestand `import-components.js` op `ui.frontend/src/components/import-components.js` . Dit dossier voert uit de doos **Reageert de Componenten van de Kern** in en stelt hen beschikbaar aan het project. Wij zullen de afbeelding van de inhoud van AEM aan de componenten van SPA in het volgende hoofdstuk inspecteren.

## Een statische SPA-component toevoegen {#static-spa-component}

Daarna, voeg een nieuwe component aan SPA toe en stel de veranderingen in een lokale instantie van AEM op. Dit is een eenvoudige verandering, enkel om te illustreren hoe het KUUROORD wordt bijgewerkt.

1. Maak onder `ui.frontend/src/components` in de module `ui.frontend` een nieuwe map met de naam `Header` .
1. Maak een bestand met de naam `Header.js` onder de map `Header` .

   ![ omslag en dossier van de Kopbal ](assets/create-project/header-folder-js.png)

1. Vul `Header.js` met het volgende:

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   Hierboven bevindt zich een standaardcomponent React die een statische tekstreeks uitvoert.

1. Open het bestand `ui.frontend/src/App.js` . Dit is het ingangspunt van de toepassing.
1. Voer de volgende updates uit in `App.js` om het statische object `Header` op te nemen:

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

1. Open een nieuwe terminal, navigeer naar de map `ui.frontend` en voer de opdracht `npm run build` uit:

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

1. Navigeer naar de map `ui.apps` . Onder `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` zou u de gecompileerde dossiers van het KUUROORD van de `ui.frontend/build` omslag moeten zien zijn gekopieerd.

   ![ de bibliotheek van de Cliënt die in ui.apps ](./assets/integrate-spa/compiled-spa-uiapps.png) wordt geproduceerd

1. Ga terug naar de terminal en navigeer naar de map `ui.apps` . Voer het volgende Geweven bevel uit:

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   Hiermee wordt het `ui.apps` -pakket geïmplementeerd in een lokale actieve instantie van AEM.

1. Open een browser lusje en navigeer aan [ http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html ](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). U zou nu de inhoud van de `Header` component moeten zien die in SPA wordt getoond.

   ![ Aanvankelijke kopbalimplementatie ](./assets/integrate-spa/initial-header-implementation.png)

   De bovenstaande stappen worden automatisch uitgevoerd wanneer u een Maven-build activeert vanuit de hoofdmap van het project (dat wil zeggen `mvn clean install -PautoInstallSinglePackage` ). U zou nu de grondbeginselen van de integratie tussen het KUUROORD en de cliënt-zijbibliotheken van AEM moeten begrijpen. U kunt in AEM nog steeds `Text` -componenten bewerken en toevoegen onder de statische `Header` -component.

## Webpack Dev Server - Proxy de JSON API {#proxy-json}

Zoals u in de vorige oefeningen hebt gezien, duurt het maken van een build en het synchroniseren van de clientbibliotheek naar een lokale instantie van AEM enkele minuten. Dit is aanvaardbaar voor het definitieve testen, maar is niet ideaal voor de meerderheid van de ontwikkeling van SPA.

A [ webpack-dev-server ](https://webpack.js.org/configuration/dev-server/) kan worden gebruikt om het KUUROORD snel te ontwikkelen. De SPA wordt aangestuurd door een JSON-model dat door AEM wordt gegenereerd. In deze oefening is de inhoud JSON van een lopende instantie van AEM **proxied** in de ontwikkelingsserver.

1. Keer terug naar winde en open het dossier `ui.frontend/package.json`.

   Zoek een lijn als deze:

   ```json
   "proxy": "http://localhost:4502",
   ```

   [ creeer React App ](https://create-react-app.dev/docs/proxying-api-requests-in-development) een gemakkelijk mechanisme aan volmacht API verzoeken. Alle onbekende aanvragen worden verlengd tot en met `localhost:4502` , de lokale AEM quickstart.

1. Open een terminalvenster en navigeer naar de map `ui.frontend` . Voer de opdracht `npm start` uit:

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

1. Open een nieuw browser lusje (als niet reeds geopend) en navigeer aan [ http://localhost:3000/content/wknd-spa-react/us/en/home.html ](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![ Webpack dev server - volmacht json ](./assets/integrate-spa/webpack-dev-server-1.png)

   Dezelfde inhoud moet worden weergegeven als in AEM, maar zonder dat een van de ontwerpmogelijkheden is ingeschakeld.

   >[!NOTE]
   >
   > Vanwege de beveiligingsvereisten van AEM moet u zich in dezelfde browser maar op een ander tabblad aanmelden bij het lokale AEM-exemplaar (http://localhost:4502).

1. Ga terug naar de IDE en maak een bestand met de naam `Header.css` in de map `src/components/Header` .
1. Vul de `Header.css` met het volgende:

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![ winde VSCode ](assets/integrate-spa/header-css-update.png)

1. Open `Header.js` opnieuw en voeg de volgende regel toe waarnaar moet worden verwezen `Header.css` :

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   Sla de wijzigingen op.

1. Navigeer aan [ http://localhost:3000/content/wknd-spa-react/us/en/home.html ](http://localhost:3000/content/wknd-spa-react/us/en/home.html) om de stijlveranderingen automatisch weerspiegeld te zien.

1. Open het bestand `Page.css` om `ui.frontend/src/components/Page` . Breng de volgende wijzigingen aan om de opvulling te herstellen:

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. Terugkeer aan browser in [ http://localhost:3000/content/wknd-spa-react/us/en/home.html ](http://localhost:3000/content/wknd-spa-react/us/en/home.html). De wijzigingen in de app worden meteen weerspiegeld.

   ![ Stijl die aan kopbal ](assets/integrate-spa/added-logo-localhost.png) wordt toegevoegd

   U kunt inhoudsupdates in AEM blijven maken en hen zien die in **worden weerspiegeld webpack-dev-server**, aangezien wij de inhoud proxying.

1. Stop de webpack-ontwikkelserver met `ctrl+c` in de terminal.

## SPA-updates voor AEM implementeren

De veranderingen die aan `Header` worden aangebracht zijn momenteel slechts zichtbaar door **webpack-dev-server**. Stel het bijgewerkte KUUROORD aan AEM op om de veranderingen te zien.

1. Navigeer aan de wortel van het project (`aem-guides-wknd-spa`) en stel het project aan AEM op gebruikend Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigeer aan [ http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html ](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). De bijgewerkte `Header` en toegepaste stijlen worden weergegeven.

   ![ Bijgewerkte Kopbal in AEM ](assets/integrate-spa/final-header-component.png)

   Nu het bijgewerkte KUUROORD in AEM is, kan het ontwerpen verdergaan.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt de SPA bijgewerkt en de integratie met AEM verkend! U weet nu hoe te om het KUUROORD tegen het modelAPI van AEM te ontwikkelen JSON gebruikend a **webpack-dev-server**.

### Volgende stappen {#next-steps}

[ de componenten van het KaartKUUROORD aan de componenten van AEM ](map-components.md) - Leer hoe te om componenten van het Antwoord aan de componenten van Adobe Experience Manager (AEM) met de Redacteur JS SDK van AEM SPA in kaart te brengen. De afbeelding van de component laat gebruikers toe om dynamische updates aan de componenten van het KUUROORD binnen de Redacteur van AEM te maken SPA, gelijkend op traditionele het auteursrecht van AEM.

## (Bonus) Webpack Dev Server - Mock JSON API {#mock-json}

Een andere manier om snel te ontwikkelen is het gebruik van een statisch JSON-bestand als JSON-model. Door de JSON te &#39;rokken&#39; verwijderen we de afhankelijkheid van een lokale AEM-instantie. Het staat ook een front-end ontwikkelaar toe om het model JSON bij te werken om functionaliteit te testen en veranderingen in JSON API te drijven die dan door een achterste-eindontwikkelaar zou worden uitgevoerd.

De aanvankelijke opstelling van het model JSON **vereist een lokale instantie van AEM**.

1. Ga terug naar de IDE en navigeer naar `ui.frontend/public` en voeg een nieuwe map toe met de naam `mock-content` .
1. Maak een nieuw bestand met de naam `mock.model.json` onder `ui.frontend/public/mock-content` .
1. In browser navigeer aan [ http://localhost:4502/content/wknd-spa-react/us/en.model.json ](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Dit is de JSON die door AEM wordt geëxporteerd en die de toepassing stuurt. Kopieer de JSON-uitvoer.

1. Plak de JSON-uitvoer uit de vorige stap in het bestand `mock.model.json` .

   ![ Van het ModelJSON- dossier van het Model van het Mock ](./assets/integrate-spa/mock-model-json-created.png)

1. Open het bestand `index.html` om `ui.frontend/public/index.html` . Werk de eigenschap metadata voor het AEM-paginamodel bij zodat deze naar een variabele verwijst `%REACT_APP_PAGE_MODEL_PATH%` :

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Als u een variabele gebruikt voor de waarde van de `cq:pagemodel_root_url` , is het gemakkelijker om te schakelen tussen het proxymodel en het mock-jsmodel.

1. Open het bestand `ui.frontend/.env.development` en voer de volgende updates uit om een opmerking te maken over de vorige waarde voor `REACT_APP_PAGE_MODEL_PATH` en `REACT_APP_API_HOST` :

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. Als momenteel lopend, stop **webpack-dev-server**. Begin **webpack-dev-server** van de terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Navigeer aan [ http://localhost:3000/content/wknd-spa-react/us/en/home.html ](http://localhost:3000/content/wknd-spa-react/us/en/home.html) en u zou het KUUROORD met de zelfde inhoud moeten zien die in de **volmacht** json wordt gebruikt.

1. Breng een kleine wijziging aan in het `mock.model.json` -bestand dat u eerder hebt gemaakt. U zou de bijgewerkte inhoud onmiddellijk moeten zien die in **wordt weerspiegeld webpack-dev-server**.

   ![ modelmodel json update ](./assets/integrate-spa/webpack-mock-model.gif)

Als u het JSON-model kunt manipuleren en de effecten op een live-SPA kunt zien, kan een ontwikkelaar de JSON-model-API beter begrijpen. Het maakt ook zowel front-end als back-end ontwikkeling parallel mogelijk.

U kunt nu schakelen tussen de locatie waar de JSON-inhoud moet worden gebruikt door de items in het `env.development` -bestand in en uit te schakelen:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
