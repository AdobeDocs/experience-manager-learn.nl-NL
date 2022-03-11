---
title: Een SPA integreren | Aan de slag met de AEM SPA Editor en reageren
description: Begrijp hoe de broncode voor een toepassing van de Enige Pagina (SPA) die in React wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals een webpack dev server, te gebruiken om de SPA tegen AEM JSON model API snel te ontwikkelen.
sub-product: sites
feature: SPA Editor
version: Cloud Service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '1840'
ht-degree: 0%

---

# De SPA integreren {#developer-workflow}

Begrijp hoe de broncode voor een toepassing van de Enige Pagina (SPA) die in React wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals een webpack dev server, te gebruiken om de SPA tegen AEM JSON model API snel te ontwikkelen.

## Doelstelling

1. Begrijp hoe het SPA project met AEM met cliënt-zijbibliotheken geïntegreerd is.
2. Leer hoe u een webpack-ontwikkelingsserver gebruikt voor speciale front-end ontwikkeling.
3. Het gebruik van een **proxy** en statisch **molen** bestand voor ontwikkeling op basis van de AEM JSON-model-API.

## Wat u gaat maken

In dit hoofdstuk brengt u enkele kleine wijzigingen aan in de SPA om te begrijpen hoe deze is geïntegreerd met AEM.
In dit hoofdstuk wordt een eenvoudig hoofdstuk toegevoegd `Header` aan de SPA. In het kader van de opbouw van deze **static** `Header` er zullen verschillende benaderingen voor AEM SPA ontwikkeling worden gebruikt .

![Nieuwe koptekst in AEM](./assets/integrate-spa/final-header-component.png)

*De SPA wordt uitgebreid om een statisch `Header` component*

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment). Dit hoofdstuk is een voortzetting van het [Project maken](create-project.md) hoofdstuk, nochtans om langs alles te volgen u wenst is een werkend SPA-toegelaten AEM project.

## Integratiebenadering {#integration-approach}

In het kader van het AEM-project werden twee modules gemaakt: `ui.apps` en `ui.frontend`.

De `ui.frontend` module is een [webpack](https://webpack.js.org/) project dat alle SPA broncode bevat. Het grootste deel van de SPA ontwikkeling en tests zal worden uitgevoerd in het webpack-project. Wanneer een productiebouwstijl wordt teweeggebracht, wordt het SPA gebouwd en gecompileerd gebruikend webpack. De gecompileerde artefacten (CSS en Javascript) worden gekopieerd in `ui.apps` die dan aan AEM runtime wordt opgesteld.

![ui.frontend architectuur op hoog niveau](assets/integrate-spa/ui-frontend-architecture.png)

*Een afbeelding op hoog niveau van de SPA integratie.*

Aanvullende informatie over de front-end build kan [hier gevonden](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect de SPA integratie {#inspect-spa-integration}

Controleer vervolgens de `ui.frontend` om de SPA te begrijpen die automatisch door de [Project archetype AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. Open uw AEM Project in IDE van uw keuze. Deze zelfstudie gebruikt de [Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

1. Het deelvenster `ui.frontend` map. Het bestand openen `ui.frontend/package.json`

1. Onder de `dependencies` u dient verschillende meldingen te zien die betrekking hebben op `react` inclusief `react-scripts`

   De `ui.frontend` is een React toepassing gebaseerd op [React-app maken](https://create-react-app.dev/) of CRA voor kort. De `react-scripts` versie geeft aan welke versie van CRA wordt gebruikt.

1. Er zijn ook diverse afhankelijkheden voorafgegaan met `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   De bovenstaande modules vormen de [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) en bieden de functionaliteit om het mogelijk te maken SPA componenten aan AEM Componenten toe te wijzen.

   Ook inbegrepen [AEM WCM-componenten - React Core-implementatie](https://github.com/adobe/aem-react-core-wcm-components-base) en [AEM WCM-componenten - Spa-editor - React Core-implementatie](https://github.com/adobe/aem-react-core-wcm-components-spa). Dit is een reeks herbruikbare UI-componenten die uit de doos AEM componenten worden toegewezen. Deze zijn ontworpen om ongewijzigd te worden gebruikt en worden vormgegeven om aan de behoeften van uw project te voldoen.

1. In de `package.json` bestand er zijn verschillende `scripts` gedefinieerd:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Dit zijn standaardconstructiescripts die zijn gemaakt [beschikbaar](https://create-react-app.dev/docs/available-scripts) door de Create React App.

   Het enige verschil is de toevoeging van `&& clientlib` aan de `build` script. Deze extra instructie is verantwoordelijk voor het kopiëren van de gecompileerde SPA naar de `ui.apps` als client-side bibliotheek tijdens een build.

   De npm-module [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) wordt gebruikt om dit te vergemakkelijken.

1. Het bestand Inspect `ui.frontend/clientlib.config.js`. Dit configuratiebestand wordt gebruikt door [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) om te bepalen hoe de clientbibliotheek moet worden gegenereerd.

1. Het bestand Inspect `ui.frontend/pom.xml`. Dit bestand transformeert de `ui.frontend` in een map [Gemaakt](https://maven.apache.org/guides/mini/guide-multiple-modules.html). De `pom.xml` bestand is bijgewerkt om het [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) tot **test** en **build** de SPA tijdens een Maven-build.

1. Het bestand Inspect `index.js` om `ui.frontend/src/index.js`:

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

   `index.js` is het ingangspunt van de SPA. `ModelManager` wordt geleverd door de AEM SPA Editor JS SDK. Het is verantwoordelijk voor het oproepen en injecteren van de `pageModel` (de JSON-inhoud) in de toepassing.

1. Het bestand Inspect `import-component.js` om `ui.frontend/src/import-components.js`. Met dit bestand wordt de afbeelding uit het vak geïmporteerd **Core Components Reageren** en stelt deze ter beschikking van het project. We zullen de toewijzing van AEM inhoud aan SPA onderdelen in het volgende hoofdstuk controleren.

## Een statische SPA toevoegen {#static-spa-component}

Voeg vervolgens een nieuwe component aan de SPA toe en implementeer de wijzigingen in een lokale AEM. Dit is een eenvoudige wijziging, alleen om te laten zien hoe de SPA wordt bijgewerkt.

1. In de `ui.frontend` module, onder `ui.frontend/src/components` een nieuwe map maken met de naam `Header`.
1. Een bestand met de naam `Header.js` onder de `Header` map.

   ![Map en bestand Koptekst](assets/create-project/header-folder-js.png)

1. Vullen `Header.js` met het volgende:

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

1. Het bestand openen `ui.frontend/src/App.js`. Dit is het ingangspunt van de toepassing.
1. Voer de volgende updates uit om `App.js` om het statische `Header`:

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

1. Open een nieuwe terminal en navigeer in de `ui.frontend` en voer de `npm run build` opdracht:

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

1. Ga naar de `ui.apps` map. Beneath `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` u zou moeten zien de gecompileerde SPA dossiers van zijn gekopieerd`ui.frontend/build` map.

   ![Clientbibliotheek gegenereerd in ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

1. Ga terug naar de terminal en navigeer in de `ui.apps` map. Voer het volgende Geweven bevel uit:

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

   Hiermee wordt het `ui.apps` verpakken naar een lokale actieve instantie van AEM.

1. Open een browsertabblad en navigeer naar [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). U moet nu de inhoud van het dialoogvenster `Header` die in de SPA worden weergegeven.

   ![Eerste koptekstimplementatie](./assets/integrate-spa/initial-header-implementation.png)

   De bovenstaande stappen worden automatisch uitgevoerd wanneer een Maven-build wordt geactiveerd vanaf de basis van het project (dat wil zeggen `mvn clean install -PautoInstallSinglePackage`). U zou nu de grondbeginselen van de integratie tussen de SPA en AEM cliënt-zijbibliotheken moeten begrijpen. U kunt nog steeds bewerkingen uitvoeren en toevoegen `Text` componenten in AEM onder de statische `Header` component.

## Webpack Dev Server - Proxy de JSON API {#proxy-json}

Zoals u in de vorige oefeningen ziet, duurt het maken van een build en het synchroniseren van de clientbibliotheek naar een lokale AEM enkele minuten. Dit is acceptabel voor de uiteindelijke test, maar niet ideaal voor het grootste deel van de SPA ontwikkeling.

A [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) kan worden gebruikt om de SPA snel te ontwikkelen. De SPA wordt aangedreven door een JSON-model dat door AEM wordt gegenereerd. In deze oefening zal de inhoud JSON van een lopende instantie van AEM **geproxeerd** in de ontwikkelingsserver.

1. Terugkeren naar IDE en het bestand openen `ui.frontend/package.json`.

   Zoek een lijn als deze:

   ```json
   "proxy": "http://localhost:4502",
   ```

   De [React-app maken](https://create-react-app.dev/docs/proxying-api-requests-in-development) biedt een eenvoudig mechanisme voor proxy-API-aanvragen. Alle onbekende aanvragen worden doorlopen `localhost:4502`, de lokale AEM.

1. Open een terminalvenster en navigeer naar de `ui.frontend` map. De opdracht uitvoeren `npm start`:

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

1. Open een nieuw browsertabblad (indien nog niet geopend) en navigeer naar [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Webpack-ontwikkelserver - proxyjson](./assets/integrate-spa/webpack-dev-server-1.png)

   U moet dezelfde inhoud zien als in AEM, maar zonder dat een van de ontwerpmogelijkheden is ingeschakeld.

   >[!NOTE]
   >
   > Vanwege de beveiligingsvereisten voor AEM moet u zich in dezelfde browser maar op een ander tabblad aanmelden bij de lokale AEM (http://localhost:4502).

1. Terugkeren naar IDE en een bestand maken met de naam `Header.css` in de `src/components/Header` map.
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

   ![VSCode IDE](assets/integrate-spa/header-css-update.png)

1. Opnieuw openen `Header.js` en voeg de volgende referentielijn toe `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   Sla de wijzigingen op.

1. Navigeren naar [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) om automatisch de wijzigingen in de stijl te zien.

1. Het bestand openen `Page.css` om `ui.frontend/src/components/Page`. Breng de volgende wijzigingen aan om de opvulling te corrigeren:

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. Ga terug naar de browser op [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). De wijzigingen in de app worden meteen weerspiegeld.

   ![Stijl toegevoegd aan koptekst](assets/integrate-spa/added-logo-localhost.png)

   U kunt inhoud blijven bijwerken in AEM en deze weergeven in **webpack-dev-server**, aangezien we de inhoud aan het proxen zijn.

1. Stop de webpack-ontwikkelserver met `ctrl+c` in de terminal.

## SPA updates voor AEM implementeren

De wijzigingen in de `Header` zijn momenteel alleen zichtbaar via **webpack-dev-server**. Implementeer de bijgewerkte SPA om de wijzigingen te AEM.

1. Ga naar de hoofdmap van het project (`aem-guides-wknd-spa`) en het project te implementeren om het te AEM met Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Navigeren naar [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). U moet de bijgewerkte `Header` en toegepaste stijlen.

   ![Bijgewerkte koptekst in AEM](assets/integrate-spa/final-header-component.png)

   Nu de bijgewerkte SPA in AEM is, kan het ontwerpen worden voortgezet.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt de SPA bijgewerkt en de integratie met AEM onderzocht! U weet hoe u de SPA kunt ontwikkelen tegen de AEM JSON-model-API met behulp van een **webpack-dev-server**.

### Volgende stappen {#next-steps}

[SPA componenten toewijzen aan AEM componenten](map-components.md) - Leer hoe u React-componenten kunt toewijzen aan Adobe Experience Manager-componenten (AEM) met de AEM SPA Editor JS SDK. Met componenttoewijzing kunnen gebruikers dynamische updates uitvoeren naar SPA componenten in de AEM SPA Editor, net als bij traditionele AEM ontwerpen.

## (Bonus) Webpack Dev Server - Mock JSON API {#mock-json}

Een andere manier om snel te ontwikkelen is het gebruik van een statisch JSON-bestand als JSON-model. Door de JSON te &#39;rokken&#39; verwijderen we de afhankelijkheid van een lokale AEM. Het staat ook een front-end ontwikkelaar toe om het model JSON bij te werken om functionaliteit te testen en veranderingen in JSON API te drijven die dan door een achterste-eindontwikkelaar zou worden uitgevoerd.

De eerste configuratie van het model-JSON doet het volgende **een lokale AEM-instantie vereisen**.

1. Terugkeer aan winde en navigeer aan `ui.frontend/public` en voeg een nieuwe map toe met de naam `mock-content`.
1. Een nieuw bestand maken met de naam `mock.model.json` beneide `ui.frontend/public/mock-content`.
1. Blader in de browser naar [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Dit is de JSON die wordt geëxporteerd door AEM die de toepassing stuurt. Kopieer de JSON-uitvoer.

1. Plak de JSON-uitvoer uit de vorige stap in het bestand `mock.model.json`.

   ![JSON-bestand Mock Model](./assets/integrate-spa/mock-model-json-created.png)

1. Het bestand openen `index.html` om `ui.frontend/public/index.html`. Werk het meta-gegevensbezit voor het AEM paginamodel bij om aan een variabele te richten `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Een variabele gebruiken voor de waarde van de `cq:pagemodel_root_url` maakt het gemakkelijker om tussen de volmacht en mock json model te schakelen.

1. Het bestand openen `ui.frontend/.env.development` en voer de volgende updates uit om opmerkingen te maken over de vorige waarde voor `REACT_APP_PAGE_MODEL_PATH` en `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. Als het programma op dat moment wordt uitgevoerd, moet u het **webpack-dev-server**. Start de **webpack-dev-server** vanaf de terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Navigeren naar [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) en u moet de SPA met dezelfde inhoud zien die in de **proxy** json.

1. Breng een kleine wijziging aan in de `mock.model.json` eerder gemaakt bestand. De bijgewerkte inhoud wordt direct weergegeven in het dialoogvenster **webpack-dev-server**.

   ![model json-update](./assets/integrate-spa/webpack-mock-model.gif)

Als u het JSON-model kunt manipuleren en de effecten op een live SPA kunt bekijken, kan een ontwikkelaar de JSON-model-API beter begrijpen. Het maakt ook zowel front-end als back-end ontwikkeling mogelijk.

U kunt nu schakelen tussen de locatie waar de JSON-inhoud moet worden gebruikt door de items in het dialoogvenster `env.development` bestand:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
