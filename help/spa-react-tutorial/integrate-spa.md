---
title: Een SPA integreren | Aan de slag met de AEM SPA Editor en Reageren
description: Begrijp hoe de broncode voor een Enige die Toepassing van de Pagina (SPA) in React wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals een webpack dev server, te gebruiken om SPA tegen het modelAPI van AEM JSON snel te ontwikkelen.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '2104'
ht-degree: 0%

---


# Een SPA integreren {#integrate-spa}

Begrijp hoe de broncode voor een Enige die Toepassing van de Pagina (SPA) in React wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals een webpack dev server, te gebruiken om SPA tegen het modelAPI van AEM JSON snel te ontwikkelen.

## Doelstelling

1. Begrijp hoe het project van het KUUROORD met AEM met cliënt-zijbibliotheken geïntegreerd is.
2. Leer hoe u een webpack-ontwikkelingsserver gebruikt voor speciale front-end ontwikkeling.
3. Het gebruik van een **proxy** en een statisch **mock** -bestand verkennen voor ontwikkeling met behulp van de AEM JSON-model-API

## Wat u gaat maken

Dit hoofdstuk zal een eenvoudige `Header` component aan SPA toevoegen. In het proces om deze statische `Header` component uit te bouwen zullen verscheidene benaderingen van AEM ontwikkeling van het KUUROORD worden gebruikt.

![Nieuwe koptekst in AEM](./assets/integrate-spa/final-header-component.png)

*Het KUUROORD wordt uitgebreid om een statische`Header`component toe te voegen*

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

### De code ophalen

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als u [AEM 6.x](overview.md#compatibility) gebruikt, voegt u het `classic` profiel toe:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `React/integrate-spa-solution`.

## Integratiebenadering {#integration-approach}

In het kader van het AEM-project werden twee modules gemaakt: `ui.apps` en `ui.frontend`.

De `ui.frontend` module is een [webpack](https://webpack.js.org/) project dat alle broncode van het KUUROORD bevat. Een meerderheid van de ontwikkeling en het testen van SPA zal in het webpack project worden gedaan. Wanneer een productie bouwt wordt teweeggebracht, wordt het KUUROORD gebouwd en gecompileerd gebruikend webpack. De gecompileerde artefacten (CSS en Javascript) worden gekopieerd in de `ui.apps` module die dan aan AEM runtime wordt opgesteld.

![ui.frontend architectuur op hoog niveau](assets/integrate-spa/ui-frontend-architecture.png)

*Een afbeelding op hoog niveau van de integratie van SPA.*

Hier [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)vindt u aanvullende informatie over de front-end build.

## Inspect de SPA-integratie {#inspect-spa-integration}

Daarna, inspecteer de `ui.frontend` module om het KUUROORD te begrijpen dat door het [AEM archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)van het Project auto-geproduceerd is.

1. In winde van uw keus open omhoog het AEM Project voor het KND SPA. Dit leerprogramma zal IDE van de Code van [Visual Studio gebruiken](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

2. Vouw de `ui.frontend` map uit en inspecteer deze. Open the file `ui.frontend/package.json`

3. Onder de `dependencies` dosis ziet u een aantal dingen die te maken hebben met `react` inclusief `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   Het `ui.frontend` is een React toepassing die op Create React App [](https://create-react-app.dev/) of CRA voor kort wordt gebaseerd. De `react-scripts` versie geeft aan welke versie van CRA wordt gebruikt.

4. Er zijn ook drie gebiedsdelen vooraf bepaald met `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   De bovengenoemde modules maken omhoog de [AEM Redacteur JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) van het KUUROORD en verstrekken de functionaliteit om het mogelijk te maken om de Componenten van het KUUROORD aan AEM Componenten in kaart te brengen.

5. In het `package.json` bestand zijn er verschillende `scripts` gedefinieerde:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   Dit zijn standaardconstructiescripts die [beschikbaar](https://create-react-app.dev/docs/available-scripts) worden gesteld in de Create React App.

   Het enige verschil is de toevoeging van `&& clientlib` het `build` script. Deze extra instructie is verantwoordelijk voor het kopiëren van het gecompileerde KUUROORD in de `ui.apps` module als cliënt-zijbibliotheek tijdens een bouwstijl.

   De npm module [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) wordt gebruikt om dit te vergemakkelijken.

6. Inspect het bestand `ui.frontend/clientlib.config.js`. Dit configuratiedossier wordt gebruikt door [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) om te bepalen hoe te om de cliëntbibliotheek te produceren.

7. Inspect het bestand `ui.frontend/pom.xml`. Dit bestand transformeert de `ui.frontend` map naar een [Maven-module](http://maven.apache.org/guides/mini/guide-multiple-modules.html). Het `pom.xml` dossier is bijgewerkt om [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) te gebruiken om het KUUROORD tijdens een Maven bouwt te **testen** en te **bouwen** .

8. Inspect het bestand `index.js` op `ui.frontend/src/index.js`:

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

   `index.js` is het ingangspunt van de SPA. `ModelManager` wordt verstrekt door de AEM redacteur van het KUUROORD JS SDK. Het is verantwoordelijk voor het aanroepen en injecteren van de `pageModel` (JSON-inhoud) in de toepassing.

## Een koptekstcomponent toevoegen {#header-component}

Daarna, voeg een nieuwe component aan SPA toe en stel de veranderingen in een lokale AEM instantie op.

1. Maak in de `ui.frontend` module hieronder `ui.frontend/src/components` een nieuwe map met de naam `Header`.
2. Maak een bestand met de naam `Header.js` onder de `Header` map.

   ![Map en bestand Koptekst](assets/integrate-spa/header-folder-js.png)

3. Vul `Header.js` met het volgende:

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

4. Open the file `ui.frontend/src/App.js`. Dit is het ingangspunt van de toepassing.
5. Voer de volgende updates uit `App.js` om het statische element op te nemen `Header`:

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

6. Open een nieuwe terminal en navigeer in de `ui.frontend` map en voer de `npm run build` opdracht uit:

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

7. Navigate to the `ui.apps` folder. Onder `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` u zou moeten zien de gecompileerde dossiers van het KUUROORD van de omslag zijn gekopieerd`ui.frontend/build` .

   ![Clientbibliotheek gegenereerd in ui.apps](./assets/integrate-spa/compiled-spa-uiapps.png)

8. Ga terug naar de terminal en navigeer in de `ui.apps` map. Voer het volgende Geweven bevel uit:

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

   Hiermee wordt het `ui.apps` pakket geïmplementeerd in een lokale actieve instantie van AEM.

9. Open een browsertabblad en navigeer naar [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). U zou nu de inhoud van de `Header` component moeten zien die in het KUUROORD wordt getoond.

   ![Eerste koptekstimplementatie](./assets/integrate-spa/initial-header-implementation.png)

   Stappen 6-8 worden automatisch uitgevoerd wanneer het teweegbrengen van een Geleiende bouwstijl van de wortel van het project (d.w.z. `mvn clean install -PautoInstallSinglePackage`). U zou nu de grondbeginselen van de integratie tussen het KUUROORD en AEM cliënt-zijbibliotheken moeten begrijpen. U kunt `Text` componenten nog steeds bewerken en toevoegen in AEM onder de statische `Header` component.

## Webpack Dev Server - Proxy de JSON API {#proxy-json}

Zoals u in de vorige oefeningen ziet, duurt het maken van een build en het synchroniseren van de clientbibliotheek naar een lokale AEM enkele minuten. Dit is aanvaardbaar voor het definitieve testen, maar is niet ideaal voor de meerderheid van de ontwikkeling van SPA.

Een [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) kan worden gebruikt om SPA snel te ontwikkelen. Het KUUROORD wordt gedreven door een model JSON dat door AEM wordt geproduceerd. In deze oefening zal de inhoud JSON van een lopende instantie van AEM **proxied** in de ontwikkelingsserver zijn.

1. Ga terug naar winde en open het dossier `ui.frontend/package.json`.

   Zoek een lijn als deze:

   ```json
   "proxy": "http://localhost:4502",
   ```

   De [Create React App](https://create-react-app.dev/docs/proxying-api-requests-in-development) verstrekt een gemakkelijk mechanisme aan volmacht API verzoeken. Alle onbekende aanvragen worden doorlopen, `localhost:4502`de lokale AEM quickstart.

2. Open een terminalvenster en navigeer naar de `ui.frontend` map. Voer de opdracht uit `npm start`:

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

3. Open een nieuw browsertabblad (indien nog niet geopend) en ga naar [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Webpack-ontwikkelserver - proxyjson](./assets/integrate-spa/webpack-dev-server-1.png)

   U moet dezelfde inhoud zien als in AEM, maar zonder dat een van de ontwerpmogelijkheden is ingeschakeld.

   >[!NOTE]
   >
   > Vanwege de beveiligingsvereisten voor AEM moet u zich in dezelfde browser maar op een ander tabblad aanmelden bij de lokale AEM (http://localhost:4502).

4. Keer terug naar winde en creeer een nieuwe omslag genoemd `media` bij `ui.frontend/src/media`.
5. Download en voeg het volgende WKND-logo toe aan de `media` map:

   ![WKND-logo](./assets/integrate-spa/wknd-logo-dk.png)

6. Openen `Header.js` bij `ui.frontend/src/components/Header/Header.js` en het logo importeren:

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. Voer de volgende updates uit `Header.js` om het logo op te nemen in de koptekst:

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   Sla de wijzigingen op in `Header.js`.

8. Ga terug naar de browser op [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). De wijzigingen in de app worden meteen weerspiegeld.

   ![Logo toegevoegd aan koptekst](./assets/integrate-spa/added-logo-localhost.png)

   U kunt doorgaan met het uitvoeren van content-updates in AEM en deze weergeven in **webpack-dev-server**, omdat we de inhoud proxying.

9. Stop de webpack dev server met `ctrl+c` in de terminal.

## Webpack Dev Server - Mock JSON API {#mock-json}

Een andere manier om snel te ontwikkelen is het gebruik van een statisch JSON-bestand als JSON-model. Door de JSON te &#39;rokken&#39; verwijderen we de afhankelijkheid van een lokale AEM. Het staat ook een front-end ontwikkelaar toe om het model JSON bij te werken om functionaliteit te testen en veranderingen in JSON API te drijven die dan door een achterste-eindontwikkelaar zou worden uitgevoerd.

Voor de eerste configuratie van het model-JSON is **wel een lokale AEM-instantie** vereist.

1. Navigeer in de browser naar [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   Dit is de JSON die wordt geëxporteerd door AEM die de toepassing stuurt. Kopieer de JSON-uitvoer.

2. Terugkeer aan winde navigeert aan `ui.frontend/public` en voegt een nieuwe omslag genoemd toe `mock-content`.
3. Maak een nieuw bestand met de naam `mock.model.json` eronder `ui.frontend/public/mock-content`. Plak de JSON-uitvoer uit **stap 1** hier.

   ![JSON-bestand Mock Model](./assets/integrate-spa/mock-model-json-created.png)

4. Open het bestand `index.html` om `ui.frontend/public/index.html`. Werk het meta-gegevensbezit voor het AEM paginamodel bij om aan een variabele te richten `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   Door een variabele voor de waarde van de eigenschap te gebruiken, `cq:pagemodel_root_url` kunt u gemakkelijker schakelen tussen de proxy en het model van de modeljson.

5. Open het bestand `ui.frontend/.env.development` en voer de volgende updates uit om een opmerking te maken over de vorige waarde voor `REACT_APP_PAGE_MODEL_PATH`:

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. Als deze actief is, stopt u de **webpack-dev-server**. Start de **webpack-dev-server** vanaf de terminal:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   Navigeer aan [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) en u zou het KUUROORD met de zelfde inhoud moeten zien die in volmachtsjson wordt gebruikt **** .

7. Breng een kleine wijziging aan in het eerder gemaakte `mock.model.json` bestand. De bijgewerkte inhoud wordt direct weergegeven in de **webpack-dev-server**.

   ![model json-update](./assets/integrate-spa/webpack-mock-model.gif)

Als u het JSON-model kunt manipuleren en de effecten op een live-SPA kunt zien, kan een ontwikkelaar de JSON-model-API beter begrijpen. Het maakt ook zowel front-end als back-end ontwikkeling mogelijk.

U kunt nu schakelen tussen de locatie waar de JSON-inhoud moet worden gebruikt door de items in het `env.development` bestand in en uit te schakelen:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Stijlen met klasse toevoegen

De beste manier om te reageren is elke component modulair en zelfstandig te houden. Een algemene aanbeveling is om te voorkomen dat dezelfde CSS-klassenaam wordt gebruikt in verschillende componenten, waardoor het gebruik van preprocessoren minder krachtig is. In dit project wordt [Sass](https://sass-lang.com/) gebruikt voor enkele nuttige functies, zoals variabelen. Dit project zal ook losjes [SUIT CSS noemende overeenkomsten](https://github.com/suitcss/suit/blob/master/doc/components.md)volgen. SUIT is een variatie van BEM-notatie, Block Element Modifier, die wordt gebruikt om consistente CSS-regels te maken.

1. Open een terminalvenster en stop de **webpack-dev-server** als deze wordt gestart. Voer vanuit de `ui.frontend` map de volgende opdracht in om Sass te [installeren](https://create-react-app.dev/docs/adding-a-sass-stylesheet):

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   Installeren `sass` als peer-afhankelijkheid:

   ```shell
   $ npm install sass --save
   ```

2. Installeren `normalize-scss` om de stijlen in verschillende browsers te normaliseren:

   ```shell
   $ npm install normalize-scss
   ```

3. Start de **webpack-dev-server** zodat we de stijlen in real-time kunnen zien bijwerken:

   ```shell
   $ npm start
   ```

   Gebruik een van de methoden Proxy of Mock voor de verwerking van de JSON-model-API.

4. Keer terug naar winde en creeer onderaan een nieuwe omslag genoemd `ui.frontend/src` `styles`.
5. Maak een nieuw bestand onder de `ui.frontend/src/styles` naam `_variables.scss` en vult het met de volgende variabelen:

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. Wijzig de naam van de extensie van het bestand `index.css` bij `ui.frontend/src/index.css` naar **`index.scss`**. Vervang de inhoud door:

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. Bijwerken `ui.frontend/src/index.js` om de opnieuw benoemde `index.scss`:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. Maak een nieuw bestand met de naam `Header.scss` eronder `ui.frontend/src/components/Header`. Vul het bestand met het volgende:

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. Opnemen `Header.scss` door bijwerken `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. Ga terug naar de browser en de **webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![Opgemaakte koptekst - webpack-ontwikkelserver](./assets/integrate-spa/styled-header.png)

   De bijgewerkte stijlen worden nu toegevoegd aan de `Header` component.

## Implementeer SPA-updates voor AEM

De wijzigingen die in het bestand zijn aangebracht, `Header` zijn momenteel alleen zichtbaar via de **webpack-dev-server**. Stel het bijgewerkte KUUROORD op om de veranderingen te AEM te zien.

1. Navigeer aan de wortel van het project (`aem-guides-wknd-spa`) en stel het project in om AEM te gebruiken Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. Ga naar [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). De update `Header` met het logo en de toegepaste stijlen wordt weergegeven.

   ![Bijgewerkte koptekst in AEM](./assets/integrate-spa/final-header-component.png)

   Nu het bijgewerkte KUUROORD in AEM is, kan het ontwerpen verdergaan.

## Fout bij oplossen van problemen in knooppuntklasse

Tijdens het ontwikkelen kan de volgende fout optreden:

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

Dit kan voorkomen wanneer de lokale versie van **Node.js** en **npm** verschillend zijn dan wat door de [frontend-beven-stop](https://github.com/eirslett/frontend-maven-plugin)wordt gebruikt. Als u de opdracht uitvoert, `npm rebuild node-sass` kunt u het probleem tijdelijk verhelpen of de `ui.frontend/node_modules` map verwijderen en opnieuw installeren.

Er zijn ook een paar manieren om dit op een meer permanente manier aan te pakken.

* Zorg ervoor dat de lokale versie van npm en Node.js de versies aanpast die door [Maven worden gebruikt bouwt](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* Voeg de volgende uitvoeringsstap toe aan de `ui.frontend/pom.xml` voor de `npm run build` stap:

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt het KUUROORD bijgewerkt en de integratie met AEM onderzocht! U kent nu twee verschillende benaderingen voor het ontwikkelen van het KUUROORD tegen AEM JSON model API gebruikend een **webpack-dev-server**.

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `React/integrate-spa-solution`.

### Volgende stappen {#next-steps}

[De componenten van het KUUROORD van de kaart aan AEM componenten](map-components.md) - Leer hoe te om componenten van het Reageren aan de componenten van Adobe Experience Manager (AEM) met de AEM Redacteur JS SDK van het KUUROORD in kaart te brengen. De afbeelding van de component laat gebruikers toe om dynamische updates aan de componenten van het KUUROORD binnen de AEM Redacteur van het KUUROORD, gelijkend op traditioneel AEM te maken.
