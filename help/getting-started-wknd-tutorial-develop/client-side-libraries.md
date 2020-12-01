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


# Client-Side Libraries en Front-end Workflow {#client-side-libraries}

Leer hoe Client-Side Bibliotheken of clientlibs worden gebruikt om CSS en JavaScript voor een implementatie van Adobe Experience Manager (AEM) Plaatsen op te stellen en te beheren. In deze zelfstudie wordt ook uitgelegd hoe de [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)-module, een losgekoppeld [webpack](https://webpack.js.org/)-project, kan worden geïntegreerd in het end-to-end buildproces.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

Het wordt ook aanbevolen de zelfstudie [Componentbasisbeginselen](component-basics.md#client-side-libraries) te lezen om inzicht te krijgen in de basisbeginselen van bibliotheken en AEM aan de clientzijde.

### Starter-project

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Clone the [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repository.
1. De `client-side-libraries/start`-vertakking uitchecken

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

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) altijd bekijken of de code plaatselijk controleren door aan de tak `client-side-libraries/solution` te schakelen.

## Doelstelling

1. Begrijp hoe clientbibliotheken via een bewerkbare sjabloon op een pagina worden opgenomen.
1. Leer hoe te om UI.Frontend Module en een webpack ontwikkelingsserver voor specifieke front-end ontwikkeling te gebruiken.
1. Begrijp de werkstroom van begin tot eind van het leveren van gecompileerde CSS en JavaScript aan een implementatie van Plaatsen.

## Wat u {#what-you-will-build} wilt maken

In dit hoofdstuk zult u sommige basislijnstijlen voor de plaats WKND en het Malplaatje van de Pagina van het Artikel in een inspanning toevoegen om de implementatie dichter aan [UI ontwerpmodellen te brengen](assets/pages-templates/wknd-article-design.xd). U zult een geavanceerde front-end werkschema gebruiken om een webpack project in een AEM cliëntbibliotheek te integreren.

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## Achtergrond {#background}

Client-Side Libraries biedt een mechanisme voor het organiseren en beheren van CSS- en JavaScript-bestanden die nodig zijn voor een AEM Sites-implementatie. De basisdoelstellingen voor client-side bibliotheken of clientlibs zijn:

1. CSS/JS opslaan in kleine aparte bestanden voor eenvoudigere ontwikkeling en eenvoudig onderhoud
1. Afhankelijkheden van raamwerken van derden op georganiseerde wijze beheren
1. Minimaliseer het aantal cliënt-zijverzoeken door CSS/JS in één of twee verzoeken samen te voegen.

Meer informatie over het gebruik van [Client-Side Libraries vindt u hier.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

Bibliotheken aan de clientzijde hebben enkele beperkingen. Het meest in het bijzonder is een beperkte ondersteuning voor populaire front-end talen zoals Sass, LESS en TypeScript. In het leerprogramma zullen wij bekijken hoe **ui.frontend** module dit kan helpen oplossen.

Implementeer de basis van de startcode naar een lokale AEM-instantie en navigeer naar [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Deze pagina is momenteel niet opgemaakt. Daarna implementeren we Client-side bibliotheken voor het WKND-merk om CSS en Javascript aan de pagina toe te voegen.

## Client-Side Libraries-organisatie {#organization}

Daarna zullen wij de organisatie van clientlibs onderzoeken die door [AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) worden geproduceerd.

![Clientbibliotheekorganisatie op hoog niveau](./assets/client-side-libraries/high-level-clientlib-organization.png)

*De organisatie van de Bibliotheek van de zijkant van het diagram van het hoog niveau Client-kant en paginaconclusie*

>[!NOTE]
>
> De volgende bibliotheekorganisatie aan de clientzijde wordt gegenereerd door AEM Project Archetype, maar vertegenwoordigt slechts een beginpunt. Hoe een project uiteindelijk CSS en Javascript aan een implementatie van Plaatsen beheert en levert kan dramatisch variëren gebaseerd op middelen, skillsets en vereisten.

1. Open de module **ui.apps** met de Eclipse of andere IDE.
1. Vouw het pad `/apps/wknd/clientlibs` uit om de clientlibs weer te geven die door het archetype worden gegenereerd.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Deze clientlibs worden hieronder gedetailleerder gecontroleerd.

1. Inspect de eigenschappen van `clientlibs/clientlib-base`.

   **clientLb-** baserVertegenwoordigt het basisniveau van CSS en JavaScript nodig voor de plaats WKND om te functioneren. Let op de eigenschap `categories` die is ingesteld op `wknd.base`. `categories` is een coderingsmechanisme voor clientlibs en is hoe ernaar kan worden verwezen.

   Let op de eigenschap `embed` en de `String[]` van waarden. Met de eigenschap `embed` worden andere clientlibs ingesloten op basis van hun categorie. **clientlib-** basewill include all of Core Component Client libraries that are required. Dit omvat artefacten zoals javascript voor de Carousel, Snelle onderzoekscomponenten om te functioneren. **Clientlib-** base bevat geen eigen CSS en JavaScript, maar sluit alleen andere clientbibliotheken in. **clientlib-** baseembedt  **clientlib-** gridclientlib met de categorie van  `wknd.grid`.

   Let op de eigenschap `allowProxy` ingesteld op `true`. Het is raadzaam `allowProxy=true` altijd in te stellen op clientlibs. Met de eigenschap `allowProxy` kunnen we de clientlibs met onze toepassingscode opslaan onder `/apps` **maar** en levert u vervolgens de clientlibs boven een pad dat vooraf is vastgesteld met `/etc.clientlibs` om te voorkomen dat toepassingscode aan eindgebruikers wordt getoond. Meer informatie over het [allowProxy bezit kan hier worden gevonden.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet).

1. Inspect de eigenschappen van `clientlibs/clientlib-grid`.

   **clientlib-** gridis is verantwoordelijk voor het opnemen/genereren van de CSS die nodig is voor de  [Layout-](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html) modus om met de AEM Sites-editor te werken. **clientlib-** gridhad een categorie ingesteld op  `wknd.grid` en is ingesloten via  **clientlib-base**.

   Het raster kan worden aangepast om verschillende hoeveelheden kolommen en onderbrekingspunten te gebruiken. Vervolgens worden de gegenereerde standaardonderbrekingspunten bijgewerkt.

1. Bestand `/apps/wknd/clientlibs/clientlib-grid/less/grid.less` bijwerken:

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

   Hierdoor worden de onderbrekingspunten zodanig gewijzigd dat deze overeenkomen met de sjabloononderbrekingspunten die zijn ingesteld in `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`.

   Dit bestand verwijst in feite naar een `grid_base.less`-bestand onder `/libs` dat een aangepaste mix bevat om het raster te genereren.

1. Inspect de eigenschappen voor `clientlibs/clientlib-site`.

   **Clientlib-** site bevat alle sitespecifieke stijlen voor het WKND-merk. Noteer de categorie van `wknd.site`. De CSS en Javascript die deze clientlib produceert zullen in feite in `ui.frontend` module worden gehandhaafd. We zullen deze integratie daarna onderzoeken.

1. Inspect de eigenschappen voor `clientlibs/clientlib-dependencies`.

   **clientlib-** gebiedsdelen zijn bedoeld om het even welke derdegebiedsdelen in te bedden. Het is een aparte clientlib, zodat deze indien nodig boven aan de HTML-pagina kan worden geladen. Noteer de categorie van `wknd.dependencies`. De CSS en Javascript die deze clientlib produceert zullen in feite in `ui.frontend` module worden gehandhaafd. We zullen deze integratie later in de zelfstudie verkennen.

## De module ui.frontend {#ui-frontend} gebruiken

Daarna zullen wij het gebruik van **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** module onderzoeken.

### Motivering

Clientbibliotheken hebben enkele beperkingen wat betreft de ondersteuning van talen zoals [Sass](https://sass-lang.com/) of [TypeScript](https://www.typescriptlang.org/). Er is ook een explosie van opensource hulpmiddelen zoals [NPM](https://www.npmjs.com/) en [webpack](https://webpack.js.org/) geweest die front-end ontwikkeling versnellen en optimaliseren.

Het basisidee achter de **ui.frontend** module is fantastische hulpmiddelen zoals NPM en Webpack te kunnen gebruiken om een meerderheid van front-end ontwikkeling te beheren. Een zeer belangrijk integratiestuk dat in **ui.frontend** module wordt gebouwd, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) neemt gecompileerde CSS en JS artefacten van een webpack/npm project en zet hen in AEM cliënt-zijbibliotheken om. Dit geeft een ontwikkelaar aan de voorkant meer vrijheid om verschillende gereedschappen en technologieën te kiezen.

![ui.frontend architectuurintegratie](assets/client-side-libraries/ui-frontend-architecture.png)

### Gebruiken

Nu zullen wij sommige basisstijlen voor het merk WKND toevoegen door sommige dossiers van de Klasse (`.scss` uitbreiding) via **ui.frontend** module toe te voegen.

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

1. Maak een ander bestand met de naam `_elements.scss` onder `src/main/webpack/base/sass` en voeg het volgende in:

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

   In het bestand `_elements.scss` worden de variabelen in `_variables.scss` gebruikt.

1. Werk `_shared.scss` onder `src/main/webpack/base/sass` bij om de `_elements.scss` en `_variables.scss` dossiers op te nemen.

   ```css
   @import './variables';
   @import './elements';
   ```

1. Open een opdrachtregelterminal en installeer de module **ui.frontend** met de opdracht `npm install`:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` na een nieuwe kloon of een nieuwe generatie van het project hoeft slechts eenmaal te worden uitgevoerd .

1. In de zelfde terminal, bouw en stel **ui.frontend** module door `npm run dev` bevel te gebruiken op:

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   De opdracht `npm run dev` moet de broncode voor het Webpack-project maken en compileren en uiteindelijk de **clientlib-site** en **clientlib-afhankelijkheden** in de module **ui.apps** vullen.

   >[!NOTE]
   >
   >Er is ook een `npm run prod` profiel dat JS en CSS minieme. Dit is de standaardcompilatie wanneer de webpack-build wordt geactiveerd via Maven. Meer details over [ui.frontend module kunnen hier worden gevonden](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect het bestand `site.css` onder `ui.frontend/dist/clientlib-site/css/site.css`. De CSS bestaat voornamelijk uit inhoud van het `_elements.scss`-bestand dat eerder is gemaakt, maar de variabelen zijn vervangen door werkelijke waarden.

   ![CSS voor gedistribueerde site](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect het bestand `ui.frontend/clientlib.config.js`. Dit is het configuratiedossier voor een npm stop, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator). **aem-clientlib-** generatoris het hulpmiddel verantwoordelijk voor het omzetten van gecompileerde CSS/JavaScript en het kopiëren in  **ui.** appsmodule.

1. Inspect het bestand `site.css` in de **ui.apps** module op `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Dit moet een identieke kopie zijn van het `site.css`-bestand uit de module **ui.frontend**. Nu het in **ui.apps** module is kan het aan AEM worden opgesteld.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Aangezien **clientlib-site** daadwerkelijk tijdens de ontwikkeltijd wordt gecompileerd, met gebruik van **npm** of **maven**, kan deze feitelijk worden genegeerd vanuit het bronbesturingselement in de module **ui.apps**. Inspect het `.gitignore`-bestand onder **ui.apps**.

>[!CAUTION]
>
> Het gebruik van de **ui.frontend** module kan niet noodzakelijk voor alle projecten zijn. De **ui.frontend** module voegt extra ingewikkeldheid toe en als er geen behoefte/wens is om sommige van deze geavanceerde voorste hulpmiddelen (Sass, webpack, npm...) te gebruiken kan het overkill zijn. Daarom wordt het beschouwd als een optioneel onderdeel van het AEM Project Archetype en wordt het gebruik van standaard client-side bibliotheken en vanilla CSS en JavaScript nog steeds volledig ondersteund.

## Pagina- en sjabloonopname {#page-inclusion}

Daarna zullen wij herzien hoe het project opstelling is om de clientlibs in AEM malplaatjes/pagina&#39;s te omvatten. Bij webontwikkeling wordt vaak aanbevolen om CSS op te nemen in de HTML-koptekst `<head>` en JavaScript vlak voordat de tag `</body>` wordt gesloten.

1. Navigeer in de module **ui.apps** naar `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`.

   ![Structuurpaginacomponent](assets/client-side-libraries/customheaderlibs-html.png)

   Dit is de `page` component die wordt gebruikt om alle pagina&#39;s in de implementatie WKND terug te geven.

1. Open het bestand `customheaderlibs.html`. Let op de regels `${clientlib.css @ categories='wknd.base'}`. Dit geeft aan dat de CSS voor de client met een categorie van `wknd.base` via dit bestand wordt opgenomen, waarbij **client-lib-base** in de koptekst van al onze pagina&#39;s wordt opgenomen.

1. Update `customheaderlibs.html` om een verwijzing naar de doopvontstijlen van Google op te nemen die wij eerder in **ui.frontend** module specificeerden. Wij zullen ook uit ContextHub voor nu commentaren...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. Inspect het bestand `customfooterlibs.html`. Dit dossier, als `customheaderlibs.html` wordt bedoeld om door projecten te worden beschreven uit te voeren. Hier betekent de regel `${clientlib.js @ categories='wknd.base'}` dat het JavaScript van **clientlib-base** onder aan al onze pagina&#39;s wordt opgenomen.

1. Bouw en stel het project aan een lokale AEM instantie op gebruikend Maven:

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Blader naar de WKND-sjablonen op [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd).

1. Selecteer en open **Article Page Template** in de Redacteur van het Malplaatje.

   ![Sjabloon artikelpagina selecteren](assets/client-side-libraries/open-article-page-template.png)

1. Klik op het pictogram **Pagina-informatie** en selecteer **Paginabeleid** in het menu om het dialoogvenster **Paginabeleid** te openen.

   ![Paginanummerbeleid voor artikelpaginasjabloon](assets/client-side-libraries/template-page-policy.png)

   *Paginagegevens > Paginabeleid*

1. De categorieën `wknd.dependencies` en `wknd.site` worden hier weergegeven. Standaard worden clientlibs die via het paginabeleid zijn geconfigureerd, gesplitst om de CSS op te nemen in de paginakop en de JavaScript op de tekstopmaak. Desgewenst kunt u expliciet vermelden dat de clientJavaScript in de paginakop moet worden geladen. Dit is het geval voor `wknd.dependencies`.

   ![Paginanummerbeleid voor artikelpaginasjabloon](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Het is ook mogelijk om direct naar `wknd.site` of `wknd.dependencies` van de paginacomponent te verwijzen, gebruikend `customheaderlibs.html` of `customfooterlibs.html` manuscript, zoals wij eerder voor `wknd.base` cliëntlib zagen. Het gebruik van de sjabloon biedt enige flexibiliteit omdat u kunt kiezen welke clientlibs per sjabloon worden gebruikt. Stel dat u een zeer zware JavaScript-bibliotheek hebt die alleen wordt gebruikt voor een geselecteerde sjabloon.

1. Navigeer naar de pagina **LA Skateparks** die is gemaakt met de **Article Page Template**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Er moet een verschil in lettertypen worden weergegeven en er moeten enkele basisstijlen worden toegepast om aan te geven dat de CSS die is gemaakt in de module **ui.frontend** werkt.

1. Klik op het pictogram **Pagina-informatie** en selecteer **Weergeven als gepubliceerd** om de artikelpagina buiten de AEM-editor te openen.

   ![Weergeven als gepubliceerd](assets/client-side-libraries/view-as-published-article-page.png)

1. Bekijk de paginabron van [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) en u zou de volgende cliëntlib verwijzingen in `<head>` moeten kunnen zien:

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

   Bericht dat de clientlibs de volmacht `/etc.clientlibs` eindpunt gebruiken. De volgende clientlib-include-bestanden vindt u onder aan de pagina:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >Op de publicatiezijde is het van essentieel belang dat de clientbibliotheken **niet** worden aangeboden bij **/apps**, omdat dit pad om beveiligingsredenen moet worden beperkt met behulp van de [Dispatcher-filtersectie](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). De [allowProxy-eigenschap](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) van de clientbibliotheek zorgt ervoor dat de CSS en JS worden aangeboden via **/etc.clientlibs**.

## Webpack DevServer {#webpack-dev-server}

In de vorige paar oefeningen konden wij verscheidene dossiers van de Klasse in **ui.frontend** module en door een bouwstijlproces bijwerken, uiteindelijk deze veranderingen zien die in AEM worden weerspiegeld. Hierna bekijken we hoe we een [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) kunnen gebruiken om snel onze front-end stijlen te ontwikkelen.

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de video worden getoond:

1. Start de webpack Dev-server met de volgende opdracht vanuit de module **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dit zou een nieuw browser venster bij [http://localhost:8080/](http://localhost:8080/) met statische prijsverhoging moeten openen.
1. Kopieer de paginabron van de LA skatepark artikelpagina op [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Plak de gekopieerde markering van AEM in `index.html` in **ui.frontend** module onder `src/main/webpack/static`.
1. Bewerk de gekopieerde opmaak en verwijder alle verwijzingen naar **clientlib-site** en **client-lib-afhankelijkheden**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   We kunnen deze verwijzingen verwijderen omdat de webpack-ontwikkelserver deze artefacten automatisch genereert.

1. Bewerk de `.scss`-bestanden en bekijk de wijzigingen die automatisch in de browser worden doorgevoerd.
1. Controleer het `/aem-guides-wknd.ui.frontend/webpack.dev.js`-bestand. Dit bevat de webpack-configuratie die wordt gebruikt om de webpack-dev-server te starten. Merk op dat het de wegen `/content` en `/etc.clientlibs` van een plaatselijk lopende instantie van AEM voorziet. Op deze manier worden de afbeeldingen en andere clientlibs (niet beheerd door de **ui.frontend**-code) beschikbaar gemaakt.

   >[!CAUTION]
   >
   > De afbeeldingsbron van de statische markering verwijst naar een actieve afbeeldingscomponent op een lokale AEM. Afbeeldingen worden verbroken weergegeven als het pad naar de afbeelding verandert, als AEM niet is gestart of als de browser niet is aangemeld bij de lokale AEM.
1. U kunt **stop** de webpack server van de bevellijn door `CTRL+C` te typen.

## {#putting-it-together} samenvoegen

De focus van deze zelfstudie ligt op client-side bibliotheken en mogelijke front-end workflows om te integreren met AEM. Met dit in mening zullen wij de implementatie versnellen door [cliënt-zijbibliotheken-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) te installeren, die sommige standaardstijlen voor de componenten verstrekt van de Kern die op het Malplaatje van de Pagina van het Artikel worden gebruikt:

* [Broodkruimel](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/breadcrumb.html)
* [Downloaden](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [Afbeelding](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/image.html)
* [Lijst](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/list.html)
* [Navigatie](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)
* [Snel zoeken](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/quick-search.html)
* [Scheidingsteken](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de video worden getoond:

1. Download [client-side-libraries-final-styles.zip](assets/client-side-libraries/client-side-libraries-final-styles.zip) en decomprimeer de inhoud onder `ui.frontend/src/main/webpack`. De inhoud van de ZIP moet de volgende mappen overschrijven:

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

De artikelpagina heeft nu enkele consistente stijlen die overeenkomen met het WKND-merk en u bent vertrouwd geraakt met de module **ui.frontend**!

### Volgende stappen {#next-steps}

Leer hoe te om individuele stijlen uit te voeren en de Componenten van de Kern te hergebruiken gebruikend het Systeem van de Stijl van de Experience Manager. [Het ontwikkelen met het Systeem van de Stijl ](style-system.md) behandelt het gebruiken van het Systeem van de Stijl om de Componenten van de Kern met merkspecifieke CSS en geavanceerde beleidsconfiguraties van de Redacteur van het Malplaatje uit te breiden.

Bekijk de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de schakelaar van de Git `client-side-libraries/solution` op.

1. Clone the [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repository.
1. Bekijk de `client-side-libraries/solution` vertakking.

## Extra hulpmiddelen en Middelen {#additional-resources}

### gewijzigd {#develop-aemfed}

[****](https://aemfed.io/) bevat een opensource opdrachtregelprogramma waarmee u de ontwikkeling aan de voorkant kunt versnellen. Het wordt aangedreven door [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) en [Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html).

Op een hoog niveau **aemfed** is ontworpen om naar dossierveranderingen binnen de **ui.apps** module te luisteren en hen automatisch te synchroniseren direct aan een lopende AEM instantie. Gebaseerd op de veranderingen, zal lokale browser automatisch verfrissen, daardoor sneller front-end ontwikkeling. Het wordt ook gebouwd om met de Traceur van het Logboek van het Sling te werken om het even welke server-zijfouten direct in de terminal automatisch te tonen.

Als u veel werk doet binnen de **ui.apps** module, die manuscripten van HTML wijzigt en douanecomponenten creeert, **aemfed** kan een zeer krachtig hulpmiddel aan gebruik zijn. [Klik hier voor volledige documentatie.](https://github.com/abmaonline/aemfed).

### Fouten opsporen in bibliotheken aan de clientzijde {#debugging-clientlibs}

Bij verschillende methoden van **categorieën** en **embedds** om veelvoudige cliëntbibliotheken te omvatten kan het lastig zijn om problemen op te lossen. AEM stelt verschillende hulpmiddelen beschikbaar om hierbij te helpen. Een van de belangrijkste gereedschappen is **Client-bibliotheken opnieuw samenstellen**. Hierdoor worden AEM gedwongen om alle LESS-bestanden opnieuw te compileren en de CSS te genereren.

* [**Grijsstructuurbibliotheken**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : geeft alle clientbibliotheken weer die in de AEM zijn geregistreerd.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**Uitvoer**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  testen: hiermee kan een gebruiker de verwachte HTML-uitvoer van clientlib op basis van een categorie bekijken.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validatie**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  van afhankelijkheden voor bibliotheken - markeert eventuele afhankelijkheden of ingesloten categorieën die niet kunnen worden gevonden.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Client-bibliotheken**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  opnieuw samenstellen: hiermee kan een gebruiker AEM dwingen alle clientbibliotheken opnieuw samen te stellen of de cache van clientbibliotheken ongeldig maken. Dit hulpmiddel is vooral effectief wanneer het ontwikkelen met LESS aangezien dit AEM kan dwingen om geproduceerde CSS opnieuw te compileren. Over het algemeen is het effectiever om de Pagina&#39;s ongeldig te maken en dan een pagina uit te voeren verfrist zich tegenover het herbouwen van alle bibliotheken. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![clientbibliotheek opnieuw samenstellen](assets/client-side-libraries/rebuild-clientlibs.png)
