---
title: Client-Side Libraries en front-end workflow
description: Leer hoe Client-Side Bibliotheken of clientlibs worden gebruikt om CSS en JavaScript voor een implementatie van Adobe Experience Manager (AEM) Plaatsen op te stellen en te beheren. Dit leerprogramma zal ook behandelen hoe de module ui.frontend, een webpack project, in het bouwstijlproces van begin tot eind kan worden geïntegreerd.
sub-product: sites
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 0%

---


# Client-Side Libraries en front-end workflow {#client-side-libraries}

Leer hoe Client-Side Bibliotheken of clientlibs worden gebruikt om CSS en JavaScript voor een implementatie van Adobe Experience Manager (AEM) Plaatsen op te stellen en te beheren. Dit leerprogramma zal ook behandelen hoe de module [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) , een losgekoppeld [webpack](https://webpack.js.org/) project, in het bouwstijlproces van begin tot eind kan worden geïntegreerd.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

Het wordt ook aanbevolen de zelfstudie [Componentbeginselen](component-basics.md#client-side-libraries) te raadplegen om inzicht te krijgen in de basisbeginselen van bibliotheken en AEM aan de clientzijde.

### Starter-project

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Clone the [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repository.
1. De `client-side-libraries/start` vertakking uitchecken

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. Stel codebasis aan een lokale AEM instantie op gebruikend uw Maven vaardigheden:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `client-side-libraries/solution`.

## Doelstelling

1. Begrijp hoe clientbibliotheken via een bewerkbare sjabloon op een pagina worden opgenomen.
1. Leer hoe te om UI.Frontend Module en een webpack ontwikkelingsserver voor specifieke front-end ontwikkeling te gebruiken.
1. Begrijp de werkstroom van begin tot eind van het leveren van gecompileerde CSS en JavaScript aan een implementatie van Plaatsen.

## Wat u gaat maken {#what-you-will-build}

In dit hoofdstuk zult u sommige basislijnstijlen voor de plaats WKND en het Malplaatje van de Pagina van het Artikel in een inspanning toevoegen om de implementatie dichter bij de [modellen](assets/pages-templates/wknd-article-design.xd)van het interfaceontwerp te brengen. U zult een geavanceerde front-end werkschema gebruiken om een webpack project in een AEM cliëntbibliotheek te integreren.

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## Achtergrond {#background}

Client-Side Libraries biedt een mechanisme voor het organiseren en beheren van CSS- en JavaScript-bestanden die nodig zijn voor een AEM Sites-implementatie. De basisdoelstellingen voor client-side bibliotheken of clientlibs zijn:

1. CSS/JS opslaan in kleine aparte bestanden voor eenvoudigere ontwikkeling en eenvoudig onderhoud
1. Afhankelijkheden van raamwerken van derden op georganiseerde wijze beheren
1. Minimaliseer het aantal cliënt-zijverzoeken door CSS/JS in één of twee verzoeken samen te voegen.

Hier vindt u meer informatie over het gebruik van bibliotheken aan de [clientzijde.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

Bibliotheken aan de clientzijde hebben enkele beperkingen. Het meest in het bijzonder is een beperkte ondersteuning voor populaire front-end talen zoals Sass, LESS en TypeScript. In de zelfstudie bekijken we hoe de module **ui.frontend** dit kan oplossen.

Implementeer de basis van de startcode naar een lokale AEM-instantie en navigeer naar [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Deze pagina is momenteel niet opgemaakt. Daarna implementeren we Client-side bibliotheken voor het WKND-merk om CSS en Javascript aan de pagina toe te voegen.

## Client-Side Libraries-organisatie {#organization}

Daarna zullen wij de organisatie van clientlibs onderzoeken die door het [AEM Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)van het Project worden geproduceerd.

![Clientbibliotheekorganisatie op hoog niveau](./assets/client-side-libraries/high-level-clientlib-organization.png)

*De organisatie van de Bibliotheek van de zijkant van het diagram van het hoog niveau Client-kant en paginaconclusie*

>[!NOTE]
>
> De volgende bibliotheekorganisatie aan de clientzijde wordt gegenereerd door AEM Project Archetype, maar vertegenwoordigt slechts een beginpunt. Hoe een project uiteindelijk CSS en Javascript aan een implementatie van Plaatsen beheert en levert kan dramatisch variëren gebaseerd op middelen, skillsets en vereisten.

1. Open de module **ui.apps** met de Eclipse of andere IDE.
1. Breid de weg uit `/apps/wknd/clientlibs` om de clientlibs te bekijken die door archetype worden geproduceerd.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Deze clientlibs worden hieronder gedetailleerder gecontroleerd.

1. Inspect de eigenschappen van `clientlibs/clientlib-base`.

   **clientlib-base** staat voor het basisniveau van CSS en JavaScript dat nodig is om de WKND-site te laten functioneren. Let op de eigenschap `categories` die is ingesteld op `wknd.base`. `categories` is een coderingsmechanisme voor clientlibs en is hoe ernaar kan worden verwezen.

   Let op de `embed` eigenschap en de `String[]` van waarden. De `embed` eigenschap sluit andere clientlibs in op basis van hun categorie. **clientlib-base** omvat alle benodigde AEM Core Component clientlibraries. Dit omvat artefacten zoals javascript voor de Carousel, Snelle onderzoekscomponenten om te functioneren. **clientlib-base** bevat geen eigen CSS en Javascript, maar sluit alleen andere clientbibliotheken in. **clientlib-base** bedt de **clientlib-grid** clientlib in met de categorie `wknd.grid`.

   Let op de `allowProxy` eigenschap ingesteld op `true`. Het is aan te raden altijd `allowProxy=true` op clientlibs in te stellen. Met de `allowProxy` eigenschap kunnen we de clientlibs met onze toepassingscode opslaan onder `/apps` maar **leveren we de clientlibs over een pad dat vooraf is ingesteld** `/etc.clientlibs` om te voorkomen dat toepassingscode aan eindgebruikers wordt getoond. Meer informatie over de eigenschap [allowProxy vindt u hier.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1. Inspect de eigenschappen van `clientlibs/clientlib-grid`.

   **clientlib-grid** is verantwoordelijk voor het opnemen/genereren van de CSS die nodig is voor de [Lay-outmodus](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html) om met de AEM Sites-editor te werken. **clientlib-grid** had een categorie ingesteld op `wknd.grid` en is ingesloten via **clientlib-base**.

   Het raster kan worden aangepast om verschillende hoeveelheden kolommen en onderbrekingspunten te gebruiken. Vervolgens worden de gegenereerde standaardonderbrekingspunten bijgewerkt.

1. Het bestand bijwerken `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`:

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   Hierdoor worden de breekpunten gewijzigd zodat deze overeenkomen met de sjabloononderbrekingspunten die in `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`Sjabloon zijn ingesteld.

   Dit bestand verwijst eigenlijk naar een `grid_base.less` bestand `/libs` waarin een aangepaste mix staat om het raster te genereren.

1. Inspect the properties for `clientlibs/clientlib-site`.

   **clientlib-site** bevat alle sitespecifieke stijlen voor het WKND-merk. Noteer de categorie van `wknd.site`. De CSS en Javascript die deze clientlib produceren zullen in feite in de `ui.frontend` module worden gehandhaafd. We zullen deze integratie daarna onderzoeken.

1. Inspect the properties for `clientlibs/clientlib-dependencies`.

   **clientlib-afhankelijkheden** zijn bedoeld om eventuele afhankelijkheden van derden in te sluiten. Het is een aparte clientlib, zodat deze indien nodig boven aan de HTML-pagina kan worden geladen. Noteer de categorie van `wknd.dependencies`. De CSS en Javascript die deze clientlib produceren zullen in feite in de `ui.frontend` module worden gehandhaafd. We zullen deze integratie later in de zelfstudie verkennen.

## De module ui.frontend gebruiken {#ui-frontend}

Daarna zullen wij het gebruik van de module **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** onderzoeken.

### Motivering

Bibliotheken aan de clientzijde hebben enkele beperkingen wat betreft de ondersteuning van talen zoals [Sass](https://sass-lang.com/) of [TypeScript](https://www.typescriptlang.org/). Er is ook een explosie geweest van opensource hulpmiddelen zoals [NPM](https://www.npmjs.com/) en [webpack](https://webpack.js.org/) die de ontwikkeling aan de voorkant versnellen en optimaliseren.

Het basisidee achter de module **ui.frontend** is fantastische hulpmiddelen zoals NPM en Webpack kunnen gebruiken om een meerderheid van front-end ontwikkeling te beheren. Een zeer belangrijk integratiestuk dat in de module **ui.frontend** wordt gebouwd, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) neemt gecompileerde CSS en JS artefacten van een webpack/npm project en zet hen in AEM cliënt-zijbibliotheken om. Dit geeft een ontwikkelaar aan de voorkant meer vrijheid om verschillende gereedschappen en technologieën te kiezen.

![ui.frontend architectuurintegratie](assets/client-side-libraries/ui-frontend-architecture.png)

### Gebruiken

Nu zullen wij sommige basisstijlen voor het merk WKND toevoegen door sommige dossiers van de Klasse (`.scss` uitbreiding) via de module **ui.frontend** toe te voegen.

1. Open de module **ui.frontend** en navigeer naar `src/main/webpack/base/sass`.

   ![ui.frontend-module](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. Maak een nieuw bestand met de naam `_variables.scss` onder de map `src/main/webpack/base/sass`.
1. Vul `_variables.scss` met het volgende:

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   Met Sass kunnen we variabelen maken, die vervolgens in verschillende bestanden kunnen worden gebruikt om consistentie te garanderen. Let op de lettertypefamilies. Verderop in de zelfstudie zullen we zien hoe we een aanroep naar Google-weblettertypen kunnen opnemen om deze lettertypen te kunnen gebruiken.

1. Maak een ander bestand met de naam `_elements.scss` eronder `src/main/webpack/base/sass` en vul het met het volgende:

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   Het `_elements.scss` bestand maakt gebruik van de variabelen in het `_variables.scss`bestand.

1. Werk `_shared.scss` hieronder bij `src/main/webpack/base/sass` om de `_elements.scss` bestanden en de `_variables.scss` bestanden op te nemen.

   ```css
   @import './variables';
   @import './elements';
   ```

1. Open een terminal op de opdrachtregel en installeer de module **ui.frontend** met de `npm install` opdracht:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` na een nieuwe kloon of een nieuwe generatie van het project hoeft slechts eenmaal te worden uitgevoerd .

1. In de zelfde terminal, bouw en stel de module **ui.frontend** door het `npm run dev` bevel te gebruiken op:

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   Het bevel `npm run dev` zou de broncode voor het project Webpack moeten bouwen en compileren en uiteindelijk de **cliëntlib-plaats** en **clientlib-gebiedsdelen** in de module **ui.apps** bevolken.

   >[!NOTE]
   >
   >Er is ook een `npm run prod` profiel dat de JS en CSS minieme waarden geeft. Dit is de standaardcompilatie wanneer de webpack-build wordt geactiveerd via Maven. Meer informatie over de module [ui.frontend vindt u hier](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect het bestand `site.css` eronder `ui.frontend/dist/clientlib-site/css/site.css`. De CSS bestaat meestal uit inhoud van het eerder gemaakte `_elements.scss` bestand, maar de variabelen zijn vervangen door werkelijke waarden.

   ![CSS voor gedistribueerde site](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect het bestand `ui.frontend/clientlib.config.js`. Dit is het configuratiedossier voor een npm stop, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator). **aem-clientlib-generator** is het hulpmiddel verantwoordelijk voor het omzetten van gecompileerde CSS/JavaScript en het kopiëren in de module **ui.apps** .

1. Inspect het bestand `site.css` in de module **ui.apps** op `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Dit moet een identieke kopie zijn van het `site.css` bestand uit de module **ui.frontend** . Nu de toepassing zich in de module **ui.apps** bevindt, kan deze worden geïmplementeerd op AEM.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Aangezien de **clientlib-plaats** eigenlijk tijdens bouwstijltijd wordt gecompileerd, gebruikend of **npm** of **gemaakt**, kan het eigenlijk van broncontrole in de module **ui.apps** worden genegeerd. Inspect het `.gitignore` bestand onder **ui.apps**.

>[!CAUTION]
>
> Het gebruik van de module **ui.frontend** is mogelijk niet voor alle projecten nodig. De module **ui.frontend** voegt extra complexiteit toe en als het niet nodig of gewenst is om een aantal van deze geavanceerde front-end tools (Sass, webpack, npm...) te gebruiken, kan het te zwaar zijn. Daarom wordt het beschouwd als een optioneel onderdeel van het AEM Project Archetype en wordt het gebruik van standaard client-side bibliotheken en vanilla CSS en JavaScript nog steeds volledig ondersteund.

## Pagina- en sjabloonopname {#page-inclusion}

Daarna zullen wij herzien hoe het project opstelling is om de clientlibs in AEM malplaatjes/pagina&#39;s te omvatten. Bij webontwikkeling is het vaak handig om CSS op te nemen in de HTML-koptekst `<head>` en JavaScript vlak voordat de `</body>` tag wordt gesloten.

1. Navigeer in de module **ui.apps** naar `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`.

   ![Structuurpaginacomponent](assets/client-side-libraries/customheaderlibs-html.png)

   Dit is de `page` component die wordt gebruikt om alle pagina&#39;s in de implementatie terug te geven WKND.

1. Open the file `customheaderlibs.html`. Let op de regels `${clientlib.css @ categories='wknd.base'}`. Dit geeft aan dat de CSS voor de client met een categorie van `wknd.base` via dit bestand wordt opgenomen, waarbij de clientlib-basis **** in de koptekst van al onze pagina&#39;s wordt opgenomen.

1. Update `customheaderlibs.html` om een verwijzing naar de lettertypestijlen van Google op te nemen die we eerder in de module **ui.frontend** hebben opgegeven. Wij zullen ook uit ContextHub voor nu commentaren...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. Inspect het bestand `customfooterlibs.html`. Dit dossier, zoals `customheaderlibs.html` wordt bedoeld om door projecten uit te voeren. Hier `${clientlib.js @ categories='wknd.base'}` betekent de regel dat het JavaScript van **clientlib-base** onder aan al onze pagina&#39;s wordt opgenomen.

1. Bouw en stel het project aan een lokale AEM instantie op gebruikend Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Blader naar de WKND-sjablonen op [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd).

1. Selecteer en open het Malplaatje **van de Pagina van het** Artikel in de Redacteur van het Malplaatje.

   ![Sjabloon artikelpagina selecteren](assets/client-side-libraries/open-article-page-template.png)

1. Klik op het pictogram **Pagina-informatie** en selecteer in het menu **Paginabeleid** om het dialoogvenster **Paginabeleid** te openen.

   ![Paginanummerbeleid voor artikelpaginasjabloon](assets/client-side-libraries/template-page-policy.png)

   *Paginagegevens > Paginabeleid*

1. De categorieën voor `wknd.dependencies` en `wknd.site` worden hier weergegeven. Standaard worden clientlibs die via het paginabeleid zijn geconfigureerd, gesplitst om de CSS op te nemen in de paginakop en de JavaScript op de tekstopmaak. Desgewenst kunt u expliciet vermelden dat de clientJavaScript in de paginakop moet worden geladen. Dat is het geval `wknd.dependencies`.

   ![Paginanummerbeleid voor artikelpaginasjabloon](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Het is ook mogelijk rechtstreeks naar de component `wknd.site` of `wknd.dependencies` van de paginacomponent te verwijzen, gebruikend het `customheaderlibs.html` of `customfooterlibs.html` manuscript, zoals wij eerder voor `wknd.base` clientlib zagen. Het gebruik van de sjabloon biedt enige flexibiliteit omdat u kunt kiezen welke clientlibs per sjabloon worden gebruikt. Stel dat u een zeer zware JavaScript-bibliotheek hebt die alleen wordt gebruikt voor een geselecteerde sjabloon.

1. Navigeer naar de pagina **LA Skateparks** die is gemaakt met de sjabloon **voor** artikelpagina: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Er wordt een verschil in lettertypen weergegeven en er worden enkele basisstijlen toegepast om aan te geven dat de CSS die in de module **ui.frontend** is gemaakt, werkt.

1. Klik op het pictogram **Pagina-informatie** en selecteer in het menu **Weergeven als gepubliceerd** om de artikelpagina buiten de AEM-editor te openen.

   ![Weergeven als gepubliceerd](assets/client-side-libraries/view-as-published-article-page.png)

1. Bekijk de bron van de Pagina van [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) en u zou de volgende cliëntlib verwijzingen in moeten kunnen zien `<head>`:

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   Bericht dat de clientlibs het volmachtseindpunt `/etc.clientlibs` gebruiken. De volgende clientlib-include-bestanden vindt u onder aan de pagina:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >Op de publicatiezijde is het van essentieel belang dat de clientbibliotheken **niet** worden aangeboden vanuit **/apps** , omdat dit pad om beveiligingsredenen moet worden beperkt met behulp van de filtersectie [](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)Dispatcher. De [eigenschap](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) allowProxy van de clientbibliotheek zorgt ervoor dat de CSS en JS worden aangeboden via **/etc.clientlibs**.

## Webpack DevServer {#webpack-dev-server}

In de vorige paar oefeningen konden wij verscheidene dossiers van Sass in de module **ui.frontend** en door een bouwstijlproces bijwerken, uiteindelijk deze veranderingen zien die in AEM worden weerspiegeld. Vervolgens bekijken we hoe we een [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) kunnen gebruiken om snel onze front-end stijlen te ontwikkelen.

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de video worden getoond:

1. Start de webpack-ontwikkelserver met de volgende opdracht vanuit de module **ui.frontend** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Hiermee opent u een nieuw browservenster op [http://localhost:8080/](http://localhost:8080/) met statische opmaakcodes.
1. Kopieer de paginabron van de artikelpagina van het LA skatepark op [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Plak de gekopieerde markering van AEM in de `index.html` module **ui.frontend** eronder `src/main/webpack/static`.
1. Bewerk de gekopieerde opmaak en verwijder verwijzingen naar **clientlib-site** en **clientlib-afhankelijkheden**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   We kunnen deze verwijzingen verwijderen omdat de webpack-ontwikkelserver deze artefacten automatisch genereert.

1. Bewerk de `.scss` bestanden en bekijk de wijzigingen die automatisch in de browser worden doorgevoerd.
1. Controleer het `/aem-guides-wknd.ui.frontend/webpack.dev.js` bestand. Dit bevat de webpack-configuratie die wordt gebruikt om de webpack-dev-server te starten. De paden `/content` en `/etc.clientlibs` een lokaal draaiende instantie van AEM worden met elkaar vergeleken. Op deze manier worden de afbeeldingen en andere clientlibs (niet beheerd door de **code ui.frontend** ) beschikbaar gemaakt.

   >[!CAUTION]
   >
   > De afbeeldingsbron van de statische markering verwijst naar een actieve afbeeldingscomponent op een lokale AEM. Afbeeldingen worden verbroken weergegeven als het pad naar de afbeelding verandert, als AEM niet is gestart of als de browser niet is aangemeld bij de lokale AEM.
1. U kunt de webpack-server **stoppen** vanaf de opdrachtregel door te typen `CTRL+C`.

## Samen plaatsen {#putting-it-together}

De focus van deze zelfstudie ligt op client-side bibliotheken en mogelijke front-end workflows om te integreren met AEM. Met dit in mening, zullen wij de implementatie versnellen door [cliënt-zij-bibliotheken-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip)te installeren, die sommige standaardstijlen voor de componenten verstrekt van de Kern die op het Malplaatje van de Pagina van het Artikel worden gebruikt:

* [Broodkruimel](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/breadcrumb.html)
* [Downloaden](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [Afbeelding](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)
* [Lijst](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/list.html)
* [Navigatie](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)
* [Snel zoeken](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/quick-search.html)
* [Scheidingsteken](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de video worden getoond:

1. Download [client-side-libraries-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) en decomprimeer de onderliggende inhoud `ui.frontend/src/main/webpack`. De inhoud van de ZIP moet de volgende mappen overschrijven:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. Geef een voorvertoning van de nieuwe stijlen weer met behulp van de webpack-ontwikkelserver:

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Stel de codebasis aan een lokale AEM instantie op om de nieuwe stijlen te zien die op het artikel van het de landspark van LA worden toegepast:

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, de artikelpagina heeft nu enkele consistente stijlen die overeenkomen met het WKND-merk en u bent vertrouwd geraakt met de module **ui.frontend** !

### Volgende stappen {#next-steps}

Leer hoe te om individuele stijlen uit te voeren en de Componenten van de Kern te hergebruiken gebruikend het Systeem van de Stijl van de Experience Manager. [Het ontwikkelen met het Systeem](style-system.md) van de Stijl behandelt het gebruiken van het Systeem van de Stijl om de Componenten van de Kern met merkspecifieke CSS en geavanceerde beleidsconfiguraties van de Redacteur van het Malplaatje uit te breiden.

Bekijk de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de schakelaar van de Git in `client-side-libraries/solution`.

1. Clone the [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repository.
1. Bekijk de `client-side-libraries/solution` vertakking.

## Aanvullende hulpmiddelen en bronnen {#additional-resources}

### geaëerd {#develop-aemfed}

[**aemfed**](https://aemfed.io/) is een open-bron, bevel-lijn hulpmiddel dat kan worden gebruikt om front-end ontwikkeling te versnellen. Deze wordt aangedreven door [aemsync](https://www.npmjs.com/package/aemsync), [BrowserSync](https://www.npmjs.com/package/browser-sync) en [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

Op een hoog niveau **is Geavanceerd** ontworpen om te luisteren naar bestandswijzigingen in de module **ui.apps** en deze automatisch te synchroniseren naar een actieve AEM. Gebaseerd op de veranderingen, zal lokale browser automatisch verfrissen, daardoor sneller front-end ontwikkeling. Het wordt ook gebouwd om met de Traceur van het Logboek van het Sling te werken om het even welke server-zijfouten direct in de terminal automatisch te tonen.

Als u veel werk verricht binnen de module **ui.apps** , die manuscripten van HTML wijzigt en douanecomponenten creeert, kan **gefiltreerd** een zeer krachtig hulpmiddel aan gebruik zijn. [Klik hier voor volledige documentatie.](https://github.com/abmaonline/aemfed).

### Fouten opsporen in clientbibliotheken {#debugging-clientlibs}

Bij verschillende methoden van **categorieën** en insluitingen **** voor het opnemen van meerdere clientbibliotheken kan het lastig zijn problemen op te lossen. AEM stelt verschillende hulpmiddelen beschikbaar om hierbij te helpen. Een van de belangrijkste tools is het **opnieuw samenstellen van clientbibliotheken** , waardoor AEM alle LESS-bestanden opnieuw moet compileren en de CSS moet genereren.

* [**Grijsstructuurbibliotheken**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Hiermee worden alle clientbibliotheken weergegeven die in de AEM zijn geregistreerd. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Uitvoer**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) testen - Hiermee kan een gebruiker de verwachte HTML-uitvoer van clientlib op basis van een categorie bekijken. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validatie**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) van afhankelijkheden voor bibliotheken - markeert eventuele afhankelijkheden of ingesloten categorieën die niet kunnen worden gevonden. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Client-bibliotheken**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) opnieuw samenstellen: hiermee kan een gebruiker AEM dwingen alle clientbibliotheken opnieuw samen te stellen of de cache van clientbibliotheken ongeldig maken. Dit hulpmiddel is vooral effectief wanneer het ontwikkelen met LESS aangezien dit AEM kan dwingen om geproduceerde CSS opnieuw te compileren. Over het algemeen is het effectiever om de Pagina&#39;s ongeldig te maken en dan een pagina uit te voeren verfrist zich tegenover het herbouwen van alle bibliotheken. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![clientbibliotheek opnieuw samenstellen](assets/client-side-libraries/rebuild-clientlibs.png)
