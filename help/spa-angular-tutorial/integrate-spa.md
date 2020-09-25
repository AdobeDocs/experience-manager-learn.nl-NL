---
title: Een SPA integreren | Aan de slag met de AEM SPA-editor en hoekig
description: Begrijp hoe de broncode voor een Enige die Toepassing van de Pagina (SPA) in Hoekig wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals het CLI hulpmiddel van Angular te gebruiken, om het KUUROORD tegen het modelAPI van AEM JSON snel te ontwikkelen.
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 0%

---


# Een SPA integreren {#integrate-spa}

Begrijp hoe de broncode voor een Enige die Toepassing van de Pagina (SPA) in Hoekig wordt geschreven met een Project van Adobe Experience Manager (AEM) kan worden geïntegreerd. Leer om moderne front-end hulpmiddelen, zoals een webpack dev server, te gebruiken om SPA tegen het modelAPI van AEM JSON snel te ontwikkelen.

## Doelstelling

1. Begrijp hoe het project van het KUUROORD met AEM met cliënt-zijbibliotheken geïntegreerd is.
2. Leer hoe u een lokale ontwikkelingsserver gebruikt voor speciale front-end ontwikkeling.
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
   $ git checkout Angular/integrate-spa-start
   ```

2. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als u [AEM 6.x](overview.md#compatibility) gebruikt, voegt u het `classic` profiel toe:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `Angular/integrate-spa-solution`.

## Integratiebenadering {#integration-approach}

In het kader van het AEM-project werden twee modules gemaakt: `ui.apps` en `ui.frontend`.

De `ui.frontend` module is een [webpack](https://webpack.js.org/) project dat alle broncode van het KUUROORD bevat. Een meerderheid van de ontwikkeling en het testen van SPA zal in het webpack project worden gedaan. Wanneer een productie bouwt wordt teweeggebracht, wordt het KUUROORD gebouwd en gecompileerd gebruikend webpack. De gecompileerde artefacten (CSS en Javascript) worden gekopieerd in de `ui.apps` module die dan aan AEM runtime wordt opgesteld.

![ui.frontend architectuur op hoog niveau](assets/integrate-spa/ui-frontend-architecture.png)

*Een afbeelding op hoog niveau van de integratie van SPA.*

Hier [](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)vindt u aanvullende informatie over de front-end build.

## Inspect de SPA-integratie {#inspect-spa-integration}

Daarna, inspecteer de `ui.frontend` module om het KUUROORD te begrijpen dat door het [AEM archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)van het Project auto-geproduceerd is.

1. In winde van uw keus open omhoog het AEM Project voor het KND SPA. Dit leerprogramma zal IDE van de Code van [Visual Studio gebruiken](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA Project](./assets/integrate-spa/vscode-ide-openproject.png)

2. Vouw de `ui.frontend` map uit en inspecteer deze. Open the file `ui.frontend/package.json`

3. Onder het `dependencies` scherm ziet u een aantal dingen die te maken hebben met `@angular`:

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

   De `ui.frontend` module is een [Hoektoepassing](https://angular.io) die door het [](https://angular.io/cli) HoekhulpmiddelCLI wordt geproduceerd te gebruiken dat het verpletteren omvat.

4. Er zijn ook drie gebiedsdelen vooraf bepaald met `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   De bovengenoemde modules maken omhoog de [AEM Redacteur JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) van het KUUROORD en verstrekken de functionaliteit om het mogelijk te maken om de Componenten van het KUUROORD aan AEM Componenten in kaart te brengen.

5. In het `package.json` bestand zijn verschillende `scripts` gedefinieerd:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   Deze manuscripten zijn gebaseerd op gemeenschappelijke [Hoekige CLI bevelen](https://angular.io/cli/build) maar lichtjes gewijzigd om met het grotere AEM project te werken.

   `start` - voert de hoekige app lokaal uit met een lokale webserver. Deze is bijgewerkt om de inhoud van een lokale AEM-instantie te profileren.

   `build` - stelt de hoekige app voor distributie van de productie samen. De toevoeging van `&& clientlib` is verantwoordelijk voor het kopiëren van gecompileerde SPA in de `ui.apps` module als cliënt-zijbibliotheek tijdens een bouwstijl. De npm module [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) wordt gebruikt om dit te vergemakkelijken.

   Meer informatie over de beschikbare scripts vindt u [hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspect het bestand `ui.frontend/clientlib.config.js`. Dit configuratiedossier wordt gebruikt door [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) om te bepalen hoe te om de cliëntbibliotheek te produceren.

7. Inspect het bestand `ui.frontend/pom.xml`. Dit bestand transformeert de `ui.frontend` map naar een [Maven-module](http://maven.apache.org/guides/mini/guide-multiple-modules.html). Het `pom.xml` dossier is bijgewerkt om [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) te gebruiken om het KUUROORD tijdens een Maven bouwt te **testen** en te **bouwen** .

8. Inspect het bestand `app.component.ts` op `ui.frontend/src/app/app.component.ts`:

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

   `app.component.js` is het ingangspunt van de SPA. `ModelManager` wordt verstrekt door de AEM redacteur van het KUUROORD JS SDK. Het is verantwoordelijk voor het aanroepen en injecteren van de `pageModel` (JSON-inhoud) in de toepassing.

## Een koptekstcomponent toevoegen {#header-component}

Daarna, voeg een nieuwe component aan SPA toe en stel de veranderingen in een lokale AEM instantie op om de integratie te zien.

1. Open een nieuw terminalvenster en navigeer naar de `ui.frontend` map:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. Installeer [HoekCLI](https://angular.io/cli#installing-angular-cli) globaal Dit wordt gebruikt om Hoekcomponenten te produceren evenals om de Hoektoepassing via het **ng** bevel te bouwen en te dienen.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > De versie van **@angular/cli** die in dit project wordt gebruikt, is **9.1.7**. Het wordt aanbevolen de hoekversies van de CLI synchroon te houden.

3. Creeer een nieuwe `Header` component door het Hoekbevel CLI van binnen de `ng generate component` `ui.frontend` omslag in werking te stellen.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   Hierdoor wordt een skelet gemaakt voor de nieuwe component Hoekkop bij `ui.frontend/src/app/components/header`.

4. Open het `aem-guides-wknd-spa` project in winde van uw keus. Navigate to the `ui.frontend/src/app/components/header` folder.

   ![Pad van koptekstcomponent in IDE](assets/integrate-spa/header-component-path.png)

5. Open het bestand `header.component.html` en vervang de inhoud door:

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   Op deze manier wordt statische inhoud weergegeven. Voor deze component Angular is dus geen aanpassing van de gegenereerde standaard vereist `header.component.ts`.

6. Open het bestand **app.component.html** op `ui.frontend/src/app/app.component.html`. Voeg de `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   Dit omvat de `header` component boven alle pagina-inhoud.

7. Open een nieuwe terminal en navigeer in de `ui.frontend` map en voer de `npm run build` opdracht uit:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. Navigate to the `ui.apps` folder. Onder `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` u zou moeten zien de gecompileerde dossiers van het KUUROORD van de omslag zijn gekopieerd`ui.frontend/build` .

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

   Hiermee wordt het `ui.apps` pakket geïmplementeerd in een lokale actieve instantie van AEM.

10. Open een browsertabblad en navigeer naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). U zou nu de inhoud van de `Header` component moeten zien die in het KUUROORD wordt getoond.

   ![Eerste koptekstimplementatie](assets/integrate-spa/initial-header-implementation.png)

   De stappen **7-9** worden automatisch uitgevoerd wanneer het teweegbrengen van een Geweven bouwt van de wortel van het project (d.w.z. `mvn clean install -PautoInstallSinglePackage`). U zou nu de grondbeginselen van de integratie tussen het KUUROORD en AEM cliënt-zijbibliotheken moeten begrijpen. U kunt wel `Text` componenten bewerken en toevoegen in AEM, maar de `Header` component kan niet worden bewerkt.

## Webpack Dev Server - Proxy de JSON API {#proxy-json}

Zoals u in de vorige oefeningen ziet, duurt het maken van een build en het synchroniseren van de clientbibliotheek naar een lokale AEM enkele minuten. Dit is aanvaardbaar voor het definitieve testen, maar is niet ideaal voor de meerderheid van de ontwikkeling van SPA.

Een [webpack dev server](https://webpack.js.org/configuration/dev-server/) kan worden gebruikt om SPA snel te ontwikkelen. Het KUUROORD wordt gedreven door een model JSON dat door AEM wordt geproduceerd. In deze oefening zal de inhoud JSON van een lopende instantie van AEM **proxied** in de ontwikkelingsserver zijn die door het [Hoekproject](https://angular.io/guide/build)wordt gevormd.

1. Keer terug naar winde en open het dossier **proxy.conf.json** bij `ui.frontend/proxy.conf.json`.

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

   De [hoekige app](https://angular.io/guide/build#proxying-to-a-backend-server) biedt een eenvoudig mechanisme voor proxy-API-aanvragen. De patronen die in `context` worden gespecificeerd zijn proxied door `localhost:4502`, lokale AEM quickstart.

2. Open het bestand **index.html** op `ui.frontend/src/index.html`. Dit is het hoofdHTML- dossier dat door de dev server wordt gebruikt.

   Er is een vermelding voor `base href="/"`. De [basistag](https://angular.io/guide/deployment#the-base-tag) is essentieel voor de toepassing om relatieve URL&#39;s op te lossen.

   ```html
   <base href="/">
   ```

3. Open een terminalvenster en navigeer naar de `ui.frontend` map. Voer de opdracht uit `npm start`:

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

5. Keer terug naar winde en creeer een nieuwe omslag genoemd `img` bij `ui.frontend/src/assets`.
6. Download en voeg het volgende WKND-logo toe aan de `img` map:

   ![WKND-logo](./assets/integrate-spa/wknd-logo-dk.png)

7. Open **header.component.html** op `ui.frontend/src/app/components/header/header.component.html` en neem het logo op:

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   Sla de wijzigingen in **header.component.html** op.

8. Ga terug naar de browser. De wijzigingen in de app worden meteen weerspiegeld.

   ![Logo toegevoegd aan koptekst](assets/integrate-spa/added-logo-localhost.png)

   U kunt inhoud-updates blijven uitvoeren in **AEM** en deze weergeven in de **webpack-ontwikkelserver**, omdat de inhoud wordt uitgebreid. De wijzigingen in de inhoud zijn alleen zichtbaar in de **webpack-ontwikkelserver**.

9. Stop de lokale Webserver met `ctrl+c` in de terminal.

## Webpack Dev Server - Mock JSON API {#mock-json}

Een andere manier om snel te ontwikkelen is het gebruik van een statisch JSON-bestand als JSON-model. Door de JSON te &#39;rokken&#39; verwijderen we de afhankelijkheid van een lokale AEM. Het staat ook een front-end ontwikkelaar toe om het model JSON bij te werken om functionaliteit te testen en veranderingen in JSON API te drijven die dan door een achterste-eindontwikkelaar zou worden uitgevoerd.

Voor de eerste configuratie van het model-JSON is **wel een lokale AEM-instantie** vereist.

1. Navigeer in de browser naar [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   Dit is de JSON die wordt geëxporteerd door AEM die de toepassing stuurt. Kopieer de JSON-uitvoer.

2. Keer terug naar winde navigeer aan `ui.frontend/src` en voeg nieuwe omslagen genoemd **mocks** en **json** toe om de volgende omslagstructuur aan te passen:

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. Maak een nieuw bestand met de naam **en.model.json** `ui.frontend/public/mocks/json`. Plak de JSON-uitvoer uit **stap 1** hier.

   ![JSON-bestand Mock Model](assets/integrate-spa/mock-model-json-created.png)

4. Maak een nieuw bestand **proxy.mock.conf.json** onder `ui.frontend`. Vul het bestand met het volgende:

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

   Deze proxyconfiguratie herschrijft aanvragen die beginnen met `/content/wknd-spa-angular/us` `/mocks/json` en het overeenkomstige statische JSON-bestand leveren, bijvoorbeeld:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. Open het bestand **angular.json**. Voeg een nieuwe **dev** configuratie met een bijgewerkte **middelenserie** toe om naar de gemaakte **mocks** omslag te verwijzen.

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

   ![Updatemap van hoekige JSON Dev Assets](assets/integrate-spa/dev-assets-update-folder.png)

   Het creëren van een specifieke **dev** configuratie zorgt ervoor dat de **mocks** omslag slechts tijdens ontwikkeling wordt gebruikt en nooit aan AEM in een productie wordt opgesteld bouwt.

6. In het **angular.json** - dossier, werk dan de **browserTarget** configuratie bij om de nieuwe **dev** configuratie te gebruiken:

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

   ![Hoekversie van JSON build dev-update](assets/integrate-spa/angular-json-build-dev-update.png)

7. Open het bestand `ui.frontend/package.json` en voeg een nieuwe **opdracht start:mock** toe om naar het bestand **proxy.mock.conf.json** te verwijzen.

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

8. Als deze actief is, stopt u de **webpack-ontwikkelserver**. Start de **webpack Dev-server** met het **script start:mock** :

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   Navigeer naar [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) en u zou het zelfde KUUROORD moeten zien maar de inhoud wordt nu getrokken uit het **mock** JSON dossier.

9. Breng een kleine wijziging aan in het bestand **en.model.json** dat u eerder hebt gemaakt. De bijgewerkte inhoud moet direct worden weerspiegeld in de **webpack-ontwikkelserver**.

   ![model json-update](./assets/integrate-spa/webpack-mock-model.gif)

   Als u het JSON-model kunt manipuleren en de effecten op een live-SPA kunt zien, kan een ontwikkelaar de JSON-model-API beter begrijpen. Het maakt ook zowel front-end als back-end ontwikkeling mogelijk.

## Stijlen met klasse toevoegen

Vervolgens wordt een bijgewerkte stijl toegevoegd aan het project. Dit project voegt ondersteuning voor [Voldoende](https://sass-lang.com/) functies, zoals variabelen, toe.

1. Open een terminalvenster en stop de **webpack-ontwikkelserver** als deze wordt gestart. Voer vanuit de `ui.frontend` map de volgende opdracht in om de hoektoepassing bij te werken naar de bestanden **.scss** .

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   Hiermee wordt het `angular.json` bestand bijgewerkt met een nieuwe vermelding onder aan het bestand:

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

3. Keer terug naar winde en creeer onderaan een nieuwe omslag genoemd `ui.frontend/src` `styles`.
4. Maak een nieuw bestand onder de `ui.frontend/src/styles` naam `_variables.scss` en vult het met de volgende variabelen:

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

5. Wijzig de naam van de extensie van het bestand **styles.css** bij `ui.frontend/src/styles.css` naar **styles.scss**. Vervang de inhoud door:

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

6. Werk **angular.json** bij en wijzig de naam van alle verwijzingen naar **style.css** met **styles.scss**. Er moeten drie verwijzingen zijn.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## Koptekststijlen bijwerken

Voeg vervolgens met Sass enkele merkspecifieke stijlen toe aan de **koptekstcomponent** .

1. Start de **webpack Dev-server** om de stijlen in real-time te bekijken:

   ```shell
   $ npm run start:mock
   ```

2. Onder `ui.frontend/src/app/components/header` re-name **header.component.css** aan **header.component.scss**. Vul het bestand met het volgende:

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

3. Werk **header.component.js** bij naar **header.component.scss**:

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

   De bijgewerkte stijlen worden nu toegevoegd aan de component **Koptekst** .

## Implementeer SPA-updates voor AEM

De wijzigingen die in de **koptekst** zijn aangebracht, zijn momenteel alleen zichtbaar via de **webpack-ontwikkelserver**. Stel het bijgewerkte KUUROORD op om de veranderingen te AEM te zien.

1. Stop de **webpack-ontwikkelserver**.
2. Navigeer aan de wortel van het project `/aem-guides-wknd-spa` en stel het project in om AEM te gebruiken Maven:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. Ga naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). De bijgewerkte **koptekst** met het toegepaste logo en de toegepaste stijlen worden weergegeven:

   ![Bijgewerkte koptekst in AEM](assets/integrate-spa/final-header-component.png)

   Nu het bijgewerkte KUUROORD in AEM is, kan het ontwerpen verdergaan.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt het KUUROORD bijgewerkt en de integratie met AEM onderzocht! U kent nu twee verschillende benaderingen voor het ontwikkelen van het KUUROORD tegen AEM JSON model API gebruikend een **webpack dev server**.

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `Angular/integrate-spa-solution`.

### Volgende stappen {#next-steps}

[De componenten van het KUUROORD van de kaart aan AEM componenten](map-components.md) - Leer hoe te om Hoekcomponenten aan de componenten van Adobe Experience Manager (AEM) met AEM Redacteur JS SDK van het KUUROORD in kaart te brengen. De afbeelding van de component laat auteurs toe om dynamische updates aan de componenten van het KUUROORD binnen de AEM Redacteur van het KUUROORD, gelijkend op traditioneel AEM te maken.
