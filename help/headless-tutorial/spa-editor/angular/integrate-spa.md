---
title: Een SPA integreren | Aan de slag met de AEM SPA Editor en Angular
description: Begrijp hoe de broncode voor een Toepassing van de Enige Pagina (SPA) die in Angular wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals het CLI hulpmiddel van de Angular te gebruiken, om de SPA tegen AEM JSON model API snel te ontwikkelen.
feature: SPA Editor
version: Cloud Service
jira: KT-5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: e9386885-86de-4e43-933c-2f0a2c04a2f2
duration: 536
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2045'
ht-degree: 0%

---

# Een SPA integreren {#integrate-spa}

Begrijp hoe de broncode voor een Toepassing van de Enige Pagina (SPA) die in Angular wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals een webpack dev server, te gebruiken om de SPA tegen AEM JSON model API snel te ontwikkelen.

## Doelstelling

1. Begrijp hoe het SPA project met AEM met cliënt-zijbibliotheken geïntegreerd is.
2. Leer hoe u een lokale ontwikkelingsserver gebruikt voor speciale front-end ontwikkeling.
3. Het gebruik van een **proxy** en statisch **molen** bestand voor ontwikkeling tegen de AEM JSON-model-API

## Wat u gaat maken

Dit hoofdstuk voegt een eenvoudig `Header` aan de SPA. Tijdens het ontwikkelen van deze statische `Header` wordt gebruik gemaakt van verschillende benaderingen voor AEM SPA ontwikkeling.

![Nieuwe koptekst in AEM](./assets/integrate-spa/final-header-component.png)

*De SPA wordt uitgebreid om een statisch `Header` component*

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment).

### De code ophalen

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als u [AEM 6,x](overview.md#compatibility) voeg toe `classic` profiel:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) of controleer de code plaatselijk door aan de tak over te schakelen `Angular/integrate-spa-solution`.

## Integratiebenadering {#integration-approach}

In het kader van het AEM-project werden twee modules gemaakt: `ui.apps` en `ui.frontend`.

De `ui.frontend` module is een [webpack](https://webpack.js.org/) project dat alle SPA broncode bevat. Het grootste deel van de SPA ontwikkeling en tests vindt plaats in het webpack-project. Wanneer een productiebouwstijl wordt teweeggebracht, wordt het SPA gebouwd en gecompileerd gebruikend webpack. De gecompileerde artefacten (CSS en Javascript) worden gekopieerd in `ui.apps` die dan aan AEM runtime wordt opgesteld.

![ui.frontend architectuur op hoog niveau](assets/integrate-spa/ui-frontend-architecture.png)

*Een afbeelding op hoog niveau van de SPA integratie.*

Aanvullende informatie over de front-end build kan [hier gevonden](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect de SPA integratie {#inspect-spa-integration}

Controleer vervolgens de `ui.frontend` om de SPA te begrijpen die automatisch door de [Project archetype AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. In winde van uw keus open omhoog het AEM Project voor de SPA WKND. In deze zelfstudie worden de [Visual Studio Code IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

2. De opdracht Uitvouwen en inspecteren `ui.frontend` map. Het bestand openen `ui.frontend/package.json`

3. Onder de `dependencies` u dient verschillende meldingen te zien die betrekking hebben op `@angular`:

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   De `ui.frontend` module is een [Toepassing angular](https://angular.io) gegenereerd door gebruik te maken van de [Angular CLI, gereedschap](https://angular.io/cli) dat het verpletteren omvat.

4. Er zijn ook drie afhankelijkheden voorafgegaan `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   De bovenstaande modules vormen de [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) en bieden de functionaliteit om het mogelijk te maken SPA componenten aan AEM Componenten toe te wijzen.

5. In de `package.json` meerdere bestanden `scripts` worden gedefinieerd:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Deze scripts zijn gebaseerd op algemene [Angular CLI-opdrachten](https://angular.io/cli/build) maar zijn enigszins gewijzigd om met het grotere AEM project te werken .

   `start` - voert de Angular-app lokaal uit met een lokale webserver. Deze is bijgewerkt om de inhoud van een lokale AEM-instantie te profileren.

   `build` - stelt de Angular-app voor productiedistributie samen. De toevoeging van `&& clientlib` is verantwoordelijk voor het kopiëren van de gecompileerde SPA naar de `ui.apps` als client-side bibliotheek tijdens een build. De npm-module [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) wordt gebruikt om dit te vergemakkelijken.

   Meer informatie over de beschikbare scripts vindt u [hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Het bestand Inspect `ui.frontend/clientlib.config.js`. Dit configuratiebestand wordt gebruikt door [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) om te bepalen hoe de clientbibliotheek moet worden gegenereerd.

7. Het bestand Inspect `ui.frontend/pom.xml`. Dit bestand transformeert de `ui.frontend` in een map [Gemaakt](https://maven.apache.org/guides/mini/guide-multiple-modules.html). De `pom.xml` bestand is bijgewerkt om het [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) tot **test** en **build** de SPA tijdens een Maven-build.

8. Het bestand Inspect `app.component.ts` om `ui.frontend/src/app/app.component.ts`:

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` is het ingangspunt van de SPA. `ModelManager` wordt geleverd door de AEM SPA Editor JS SDK. Het is verantwoordelijk voor het oproepen en injecteren van de `pageModel` (de JSON-inhoud) in de toepassing.

## Een koptekstcomponent toevoegen {#header-component}

Vervolgens voegt u een nieuwe component aan de SPA toe en implementeert u de wijzigingen in een lokale AEM-instantie om de integratie te zien.

1. Open een nieuw terminalvenster en ga naar het `ui.frontend` map:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Installeren [ANGULAR CLI](https://angular.io/cli#installing-angular-cli) globaal wordt dit gebruikt om de componenten van de Angular te produceren evenals de toepassing van de Angular te bouwen en te dienen via **ng** gebruiken.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > De versie van **@angular/cli** wordt gebruikt door dit project **9.1.7.**. Het wordt aanbevolen de Angular CLI-versies synchroon te houden.

3. Een nieuwe `Header` component door de Angular CLI in werking te stellen `ng generate component` opdracht vanuit de `ui.frontend` map.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Hiermee wordt een skelet gemaakt voor de nieuwe Angular Header-component op `ui.frontend/src/app/components/header`.

4. Open de `aem-guides-wknd-spa` project in winde van uw keus. Ga naar de `ui.frontend/src/app/components/header` map.

   ![Pad van koptekstcomponent in IDE](assets/integrate-spa/header-component-path.png)

5. Het bestand openen `header.component.html` en vervang de inhoud door:

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Merk op dit vertoningen statische inhoud, zodat vereist deze component van Angular geen aanpassingen van het gebrek geproduceerd `header.component.ts`.

6. Het bestand openen **app.component.html** om  `ui.frontend/src/app/app.component.html`. Voeg de `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Hieronder vallen ook de `header` boven alle pagina-inhoud.

7. Open een nieuwe terminal en navigeer in de `ui.frontend` en voer de `npm run build` opdracht:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Ga naar de `ui.apps` map. Beneath `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` u zou moeten zien de gecompileerde SPA dossiers van zijn gekopieerd`ui.frontend/build` map.

   ![Clientbibliotheek gegenereerd in ui.apps](assets/integrate-spa/compiled-spa-uiapps.png)

9. Ga terug naar de terminal en navigeer in de `ui.apps` map. Voer het volgende Geweven bevel uit:

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

   Hierdoor wordt de `ui.apps` verpakken naar een lokale actieve instantie van AEM.

10. Open een browsertabblad en navigeer naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). U moet nu de inhoud van de `Header` die in de SPA worden weergegeven.

   ![Eerste koptekstimplementatie](assets/integrate-spa/initial-header-implementation.png)

   Stappen **7-9** worden automatisch uitgevoerd wanneer het teweegbrengen van een Maven bouwstijl van de wortel van het project (d.w.z. `mvn clean install -PautoInstallSinglePackage`). U zou nu de grondbeginselen van de integratie tussen de SPA en AEM cliënt-zijbibliotheken moeten begrijpen. U kunt nog steeds bewerkingen uitvoeren en toevoegen `Text` in AEM `Header` kan niet worden bewerkt.

## Webpack Dev Server - Proxy de JSON API {#proxy-json}

Zoals u in de vorige oefeningen ziet, duurt het maken van een build en het synchroniseren van de clientbibliotheek naar een lokale AEM enkele minuten. Dit is acceptabel voor de uiteindelijke test, maar niet ideaal voor het grootste deel van de SPA ontwikkeling.

A [webpack-ontwikkelserver](https://webpack.js.org/configuration/dev-server/) kan worden gebruikt om de SPA snel te ontwikkelen. De SPA wordt aangedreven door een JSON-model dat door AEM wordt gegenereerd. In deze oefening is de JSON-inhoud van een actief exemplaar van AEM **geproxeerd** in de ontwikkelingsserver die door [Angular-project](https://angular.io/guide/build).

1. Terugkeren naar IDE en het bestand openen **proxy.conf.json** om `ui.frontend/proxy.conf.json`.

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   De [Angular-app](https://angular.io/guide/build#proxying-to-a-backend-server) biedt een eenvoudig mechanisme voor proxy-API-aanvragen. De patronen die zijn opgegeven in `context` worden uitgebreid tot `localhost:4502`, de lokale AEM.

2. Het bestand openen **index.html** om `ui.frontend/src/index.html`. Dit is het basisbestand voor HTML dat door de ontwikkelserver wordt gebruikt.

   Er is een item voor `base href="/"`. De [basislabel](https://angular.io/guide/deployment#the-base-tag) is essentieel voor de toepassing om relatieve URL&#39;s op te lossen.

   ```html
   <base href="/">
   ```

3. Open een terminalvenster en navigeer naar de `ui.frontend` map. De opdracht uitvoeren `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. Open een nieuw browsertabblad (indien nog niet geopend) en ga naar [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Webpack-ontwikkelserver - proxyjson](assets/integrate-spa/webpack-dev-server-1.png)

   U moet dezelfde inhoud zien als in AEM, maar zonder dat een van de ontwerpmogelijkheden is ingeschakeld.

5. Terugkeer aan winde en creeer een nieuwe omslag genoemd `img` om `ui.frontend/src/assets`.
6. Download en voeg het volgende WKND-logo toe aan de `img` map:

   ![WKND-logo](./assets/integrate-spa/wknd-logo-dk.png)

7. Openen **header.component.html** om `ui.frontend/src/app/components/header/header.component.html` en het logo vermelden:

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   De wijzigingen opslaan in **header.component.html**.

8. Ga terug naar de browser. De wijzigingen in de app worden meteen weerspiegeld.

   ![Logo toegevoegd aan koptekst](assets/integrate-spa/added-logo-localhost.png)

   U kunt doorgaan met het bijwerken van inhoud in **AEM** en deze zichtbaar maken in **webpack-ontwikkelserver**, aangezien we de inhoud aan het proxen zijn. De wijzigingen in de inhoud zijn alleen zichtbaar in het dialoogvenster **webpack-ontwikkelserver**.

9. De lokale webserver stoppen met `ctrl+c` in de terminal.

## Webpack Dev Server - Mock JSON API {#mock-json}

Een andere manier om snel te ontwikkelen is het gebruik van een statisch JSON-bestand als JSON-model. Door de JSON te &#39;rokken&#39; verwijderen we de afhankelijkheid van een lokale AEM. Het staat ook een front-end ontwikkelaar toe om het model JSON bij te werken om functionaliteit te testen en veranderingen in JSON API te drijven die dan door een achterste-eindontwikkelaar zou worden uitgevoerd.

De eerste configuratie van het model-JSON doet het volgende **een lokale AEM-instantie vereisen**.

1. Blader in de browser naar [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Dit is de JSON die wordt geëxporteerd door AEM die de toepassing stuurt. Kopieer de JSON-uitvoer.

2. Terugkeer aan winde navigeert om `ui.frontend/src` en nieuwe mappen toevoegen met de naam **mocks** en **json** in overeenstemming met de volgende mappenstructuur:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Een nieuw bestand maken met de naam **en.model.json** beneide `ui.frontend/public/mocks/json`. De JSON-uitvoer plakken vanuit **Stap 1** hier.

   ![JSON-bestand Mock Model](assets/integrate-spa/mock-model-json-created.png)

4. Een nieuw bestand maken **proxy.mock.conf.json** beneide `ui.frontend`. Vul het bestand met het volgende:

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   Deze volmachtsconfiguratie zal verzoeken herschrijven die met beginnen `/content/wknd-spa-angular/us` with `/mocks/json` en het overeenkomstige statische JSON-bestand gebruiken, bijvoorbeeld:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Het bestand openen **angular.json**. Een nieuwe toevoegen **dev** configuratie met een bijgewerkte **elementen** array die verwijst naar de **mocks** gemaakte map.

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![Angular JSON Dev Assets Update Folder](assets/integrate-spa/dev-assets-update-folder.png)

   Een speciale toepassing maken **dev** de configuratie ervoor zorgt dat **mocks** de map wordt alleen gebruikt tijdens de ontwikkeling en wordt nooit geïmplementeerd om te AEM in een productiebuild.

6. In de **angular.json** bestand, volgende update **browserTarget** configuratie om de nieuwe **dev** configuratie:

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![Angular JSON build dev-update](assets/integrate-spa/angular-json-build-dev-update.png)

7. Het bestand openen `ui.frontend/package.json` en voeg een nieuwe **start:mock** gebruiken om naar de **proxy.mock.conf.json** bestand.

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   Door een nieuwe opdracht toe te voegen, kunt u gemakkelijk schakelen tussen de proxyconfiguraties.

8. Als het programma op dat moment wordt uitgevoerd, moet u het **webpack-ontwikkelserver**. Start de **webpack-ontwikkelserver** met de **start:mock** script:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navigeren naar [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) en u zou het zelfde SPA moeten zien maar de inhoud wordt nu getrokken uit **molen** JSON-bestand

9. Breng een kleine wijziging aan in de **en.model.json** eerder gemaakt bestand. De bijgewerkte inhoud moet onmiddellijk worden weergegeven in de **webpack-ontwikkelserver**.

   ![model json-update](./assets/integrate-spa/webpack-mock-model.gif)

   Als u het JSON-model kunt manipuleren en de effecten op een live SPA kunt bekijken, kan een ontwikkelaar de JSON-model-API beter begrijpen. Het maakt ook zowel front-end als back-end ontwikkeling parallel mogelijk.

## Stijlen met klasse toevoegen

Vervolgens wordt een bijgewerkte stijl toegevoegd aan het project. Dit project voegt [Soort](https://sass-lang.com/) ondersteuning voor enkele handige functies, zoals variabelen.

1. Open een terminalvenster en stop het **webpack-ontwikkelserver** indien gestart. Van binnen `ui.frontend` Voer de volgende opdracht in om de Angular-app bij te werken naar proceskleuren **.scss** bestanden.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Hiermee werkt u de `angular.json` bestand met een nieuwe vermelding onder aan het bestand:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. Installeren `normalize-scss` om de stijlen in verschillende browsers te normaliseren:

   ```shell
   $ npm install normalize-scss --save
   ```

3. Terugkeer naar winde en onderaan `ui.frontend/src` een nieuwe map maken met de naam `styles`.
4. Nieuw bestand maken onder `ui.frontend/src/styles` benoemd `_variables.scss` en vult deze met de volgende variabelen:

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
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. Geef de extensie van het bestand een nieuwe naam **styles.css** om `ui.frontend/src/styles.css` tot **styles.scss**. Vervang de inhoud door het volgende:

   ```scss
   /* styles.scss * /
   
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
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. Bijwerken **angular.json** en hernoem alle verwijzingen naar **style.css** with **styles.scss**. Er moeten drie verwijzingen zijn.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Koptekststijlen bijwerken

Voeg vervolgens een aantal merkspecifieke stijlen toe aan de **Koptekst** component die Sass gebruikt.

1. Start de **webpack-ontwikkelserver** om de stijlen te zien die in real time bijwerken:

   ```shell
   $ npm run start:mock
   ```

2. Onder `ui.frontend/src/app/components/header` naam wijzigen **header.component.css** tot **header.component.scss**. Vul het bestand met het volgende:

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. Bijwerken **header.component.ts** verwijzing **header.component.scss**:

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. Ga terug naar de browser en de **webpack-ontwikkelserver**:

   ![Opgemaakte koptekst - webpack-ontwikkelserver](assets/integrate-spa/styled-header.png)

   De bijgewerkte stijlen worden nu toegevoegd aan het dialoogvenster **Koptekst** component.

## SPA updates voor AEM implementeren

De wijzigingen in de **Koptekst** zijn momenteel alleen zichtbaar via **webpack-ontwikkelserver**. Implementeer de bijgewerkte SPA om de wijzigingen te AEM.

1. Stop de **webpack-ontwikkelserver**.
2. Ga naar de hoofdmap van het project `/aem-guides-wknd-spa` en stel het project in om AEM te gebruiken Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Navigeren naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). U moet de bijgewerkte **Koptekst** waarop logo en stijlen zijn toegepast:

   ![Bijgewerkte koptekst in AEM](assets/integrate-spa/final-header-component.png)

   Nu de bijgewerkte SPA in AEM is, kan het ontwerpen worden voortgezet.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt de SPA bijgewerkt en de integratie met AEM onderzocht! U kent nu twee verschillende manieren om de SPA te ontwikkelen tegen de AEM JSON-model-API met behulp van een **webpack-ontwikkelserver**.

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) of controleer de code plaatselijk door aan de tak over te schakelen `Angular/integrate-spa-solution`.

### Volgende stappen {#next-steps}

[SPA componenten toewijzen aan AEM componenten](map-components.md) - Leer hoe u Angulars aan Adobe Experience Manager-componenten (AEM) kunt toewijzen met de AEM SPA Editor JS SDK. Met componenttoewijzing kunnen auteurs dynamische updates uitvoeren naar SPA componenten in de AEM SPA Editor, net als bij traditionele AEM ontwerpen.
