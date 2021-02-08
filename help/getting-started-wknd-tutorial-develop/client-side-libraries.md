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
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '3291'
ht-degree: 0%

---


# Client-Side Libraries en Front-end Workflow {#client-side-libraries}

Leer hoe Client-Side Bibliotheken of clientlibs worden gebruikt om CSS en JavaScript voor een implementatie van Adobe Experience Manager (AEM) Plaatsen op te stellen en te beheren. In deze zelfstudie wordt ook uitgelegd hoe de [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)-module, een losgekoppeld [webpack](https://webpack.js.org/)-project, kan worden geïntegreerd in het end-to-end buildproces.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

Het wordt ook aanbevolen de zelfstudie [Componentbasisbeginselen](component-basics.md#client-side-libraries) te lezen om inzicht te krijgen in de basisbeginselen van bibliotheken en AEM aan de clientzijde.

### Starter-project

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide kunt u het project hergebruiken en de stappen overslaan voor het uitchecken van het starterproject.

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Ontdek de `tutorial/client-side-libraries-start`-vertakking van [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Stel codebasis aan een lokale AEM instantie op gebruikend uw Maven vaardigheden:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Als u AEM 6.5 of 6.4 gebruikt, voegt u het `classic`-profiel toe aan alle Maven-opdrachten.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) altijd bekijken of de code plaatselijk controleren door aan de tak `tutorial/client-side-libraries-solution` te schakelen.

## Doelstelling

1. Begrijp hoe clientbibliotheken via een bewerkbare sjabloon op een pagina worden opgenomen.
1. Leer hoe te om UI.Frontend Module en een webpack ontwikkelingsserver voor specifieke front-end ontwikkeling te gebruiken.
1. Begrijp de werkstroom van begin tot eind van het leveren van gecompileerde CSS en JavaScript aan een implementatie van Plaatsen.

## Wat u {#what-you-will-build} wilt maken

In dit hoofdstuk zult u sommige basislijnstijlen voor de plaats WKND en het Malplaatje van de Pagina van het Artikel in een inspanning toevoegen om de implementatie dichter aan [UI ontwerpmodellen te brengen](assets/pages-templates/wknd-article-design.xd). U zult een geavanceerde front-end werkschema gebruiken om een webpack project in een AEM cliëntbibliotheek te integreren.

![Voltooide stijlen](assets/client-side-libraries/finished-styles.png)

*Artikelpagina met toegepaste basislijnstijlen*

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

1. Als u VSCode of een andere IDE gebruikt, opent u de module **ui.apps**.
1. Vouw het pad `/apps/wknd/clientlibs` uit om de clientlibs weer te geven die door het archetype worden gegenereerd.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   Deze clientlibs worden hieronder gedetailleerder gecontroleerd.

1. De volgende tabel geeft een overzicht van de clientbibliotheken. Meer informatie over [inclusief clientbibliotheken vindt u hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Naam | Beschrijving | Opmerkingen |
   |-------------------| ------------| ------|
   | `clientlib-base` | Basisniveau van CSS en JavaScript nodig voor WKND-site om te kunnen functioneren | Sluit de clientbibliotheken van de Core Component in |
   | `clientlib-grid` | Hiermee genereert u de CSS die nodig is om [Lay-outmodus](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) te laten werken. | Mobiele/tabletonderbrekingspunten kunnen hier worden geconfigureerd |
   | `clientlib-site` | Bevat site-specifiek thema voor de WKND-site | Gegenereerd door de module `ui.frontend` |
   | `clientlib-dependencies` | Hiermee sluit u eventuele afhankelijkheden van derden in | Gegenereerd door de module `ui.frontend` |

1. Merk op dat `clientlib-site` en `clientlib-dependencies` van broncontrole worden genegeerd. Dit is door ontwerp, aangezien deze bij bouwstijltijd door de `ui.frontend` module zullen worden geproduceerd.

## Basisstijlen {#base-styles} bijwerken

Werk vervolgens de basisstijlen bij die zijn gedefinieerd in de module **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**. De dossiers in `ui.frontend` module zullen `clientlib-site` en `clientlib-dependecies` bibliotheken produceren die het thema van de Plaats en om het even welke derdegebiedsdelen bevatten.

Clientbibliotheken hebben enkele beperkingen wat betreft de ondersteuning van talen zoals [Sass](https://sass-lang.com/) of [TypeScript](https://www.typescriptlang.org/). Er zijn een aantal open-bronhulpmiddelen zoals [NPM](https://www.npmjs.com/) en [webpack](https://webpack.js.org/) die front-end ontwikkeling versnellen en optimaliseren. Het doel van de **ui.frontend** module is deze hulpmiddelen te kunnen gebruiken om een meerderheid van front-end brondossiers te beheren.

1. Open de module **ui.frontend** en navigeer naar `src/main/webpack/site`.
1. Het bestand `main.scss` openen

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` is het ingangspunt aan alle dossiers van de Klasse in de  `ui.frontend` module. Het zal het `_variables.scss` dossier omvatten, dat een reeks merkvariabelen bevat die door verschillende dossiers van de Klassen in het project moeten worden gebruikt. Het `_base.scss`-bestand is ook opgenomen en definieert enkele basisstijlen voor HTML-elementen. Een reguliere expressie bevat alle stijlen voor afzonderlijke componentstijlen onder `src/main/webpack/components`. Een andere reguliere expressie bevat alle bestanden onder `src/main/webpack/site/styles`.

1. Inspect het bestand `main.ts`. `main.ts` neemt een reguliere expressie op  `main.scss` en neemt deze op om alle  `.js` of  `.ts` bestanden in het project te verzamelen. Dit ingangspunt zal door [webpack configuratiedossiers](https://webpack.js.org/configuration/) als ingangspunt voor de volledige `ui.frontend` module worden gebruikt.

1. Inspect de bestanden onder `src/main/webpack/site/styles`:

   ![Stijlbestanden](assets/client-side-libraries/style-files.png)

   Deze bestanden zijn stijlen voor algemene elementen in de sjabloon, zoals Koptekst, Voettekst en container met hoofdinhoud. De CSS-regels in deze bestanden zijn gericht op verschillende HTML-elementen `header`, `main` en `footer`. Deze HTML-elementen zijn gedefinieerd door beleid in het vorige hoofdstuk [Pagina&#39;s en sjablonen](./pages-templates.md).

1. Vouw de map `components` onder `src/main/webpack` uit en inspecteer de bestanden.

   ![Bestanden van componentklasse](assets/client-side-libraries/component-sass-files.png)

   Elk bestand wordt toegewezen aan een kerncomponent, zoals de [Accordeoncomponent](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). Elke Core-component is gebouwd met de notatie [Element modifier](https://getbem.com/) of BEM blokkeren om het eenvoudiger te maken om specifieke CSS-klassen met stijlregels als doel in te stellen. De bestanden onder `/components` zijn door het AEM Projectarchetype met de verschillende BEM-regels voor elke component uitgestald.

1. Download de WKND-basisstijlen **[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** en **unzip** het bestand.

   ![WKND-basisstijlen](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Om de zelfstudie te versnellen, hebben wij de verscheidene dossiers verstrekt van de Klasse die het merk WKND op de Componenten van de Kern en de structuur van het Malplaatje van de Pagina van het Artikel uitvoeren.

1. Overschrijf de inhoud van `ui.frontend/src` met bestanden uit de vorige stap. De inhoud van de ZIP moet de volgende mappen overschrijven:

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![Gewijzigde bestanden](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect de gewijzigde bestanden voor meer informatie over de WKND-stijlimplementatie.

## Inspect the ui.frontend integration {#ui-frontend-integration}

Een zeer belangrijk integratiestuk dat in **ui.frontend** module wordt gebouwd, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) neemt gecompileerde CSS en JS artefacten van een webpack/npm project en zet hen in AEM cliënt-zijbibliotheken om.

![ui.frontend architectuurintegratie](assets/client-side-libraries/ui-frontend-architecture.png)

Deze integratie wordt automatisch ingesteld door het AEM Project Archetype. Ga vervolgens na hoe het werkt.


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
   ```

   >[!CAUTION]
   >
   > Je ontvangt mogelijk een fout als &quot;FOUT in./src/main/webpack/site/main.scss&quot;.
   > Dit gebeurt gewoonlijk omdat uw omgeving is gewijzigd sinds `npm install` wordt uitgevoerd.
   > Voer `npm rebuild node-sass` uit om het probleem op te lossen. Dit gebeurt als de versie van `npm` die op uw lokale dev-computer is geïnstalleerd, afwijkt van de versie die door de Maven `frontend-maven-plugin` in het `aem-guides-wknd/pom.xml`-bestand wordt gebruikt. U kunt dit permanent verhelpen door de versie in het pombestand aan te passen aan uw lokale versie of andersom.

1. De opdracht `npm run dev` moet de broncode voor het Webpack-project maken en compileren en uiteindelijk de **clientlib-site** en **clientlib-afhankelijkheden** in de module **ui.apps** vullen.

   >[!NOTE]
   >
   >Er is ook een `npm run prod` profiel dat JS en CSS minieme. Dit is de standaardcompilatie wanneer de webpack-build wordt geactiveerd via Maven. Meer details over [ui.frontend module kunnen hier worden gevonden](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect het bestand `site.css` onder `ui.frontend/dist/clientlib-site/css/site.css`. Dit is gecompileerde CSS die op de brondossiers van de Klasse wordt gebaseerd.

   ![CSS voor gedistribueerde site](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect het bestand `ui.frontend/clientlib.config.js`. Dit is het configuratiedossier voor een npm stop, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) die de inhoud van `/dist` in een cliëntbibliotheek omzet en het naar `ui.apps` module verplaatst.

1. Inspect het bestand `site.css` in de **ui.apps** module op `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Dit moet een identieke kopie zijn van het `site.css`-bestand uit de module **ui.frontend**. Nu het in **ui.apps** module is kan het aan AEM worden opgesteld.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Aangezien **clientlib-site** tijdens de ontwikkeltijd wordt gecompileerd met **npm** of **maven**, kan deze veilig worden genegeerd door bronbesturing in de module **ui.apps**. Inspect het `.gitignore`-bestand onder **ui.apps**.

1. Synchroniseer de `clientlib-site` bibliotheek met een lokale instantie van AEM gebruikend de ontwikkelaarshulpmiddelen of Maven vaardigheden.

   ![Clientlib-site synchroniseren](assets/client-side-libraries/sync-clientlib-site.png)

1. Open het artikel van LA Skatepark in AEM op: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Bijgewerkte basisstijlen voor het artikel](assets/client-side-libraries/updated-base-styles.png)

   De bijgewerkte stijlen voor het artikel worden nu weergegeven. Mogelijk moet u op de een of andere manier vernieuwen om CSS-bestanden te wissen die in cache zijn geplaatst door de browser.

   Het begint veel dichter bij de mockups te komen!

   >[!NOTE]
   >
   > De hierboven uitgevoerde stappen om de code ui.frontend aan AEM te bouwen en op te stellen zullen automatisch worden uitgevoerd wanneer een Maven bouwt van de wortel van het project `mvn clean install -PautoInstallSinglePackage` wordt teweeggebracht.

>[!CAUTION]
>
> Het gebruik van de **ui.frontend** module kan niet noodzakelijk voor alle projecten zijn. De **ui.frontend** module voegt extra complexiteit toe en als er geen behoefte/wens is om enkele van deze geavanceerde front-end hulpmiddelen (Sass, webpack, npm...) te gebruiken, is het mogelijk niet nodig.

## Pagina- en sjabloonopname {#page-inclusion}

Daarna, herzien hoe de clientlibs in de Pagina van de AEM van verwijzingen worden voorzien. Bij webontwikkeling wordt vaak aanbevolen om CSS op te nemen in de HTML-koptekst `<head>` en JavaScript vlak voordat de tag `</body>` wordt gesloten.

1. Navigeer in de module **ui.apps** naar `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`.

   ![Structuurpaginacomponent](assets/client-side-libraries/customheaderlibs-html.png)

   Dit is de `page` component die wordt gebruikt om alle pagina&#39;s in de implementatie WKND terug te geven.

1. Open het bestand `customheaderlibs.html`. Let op de regels `${clientlib.css @ categories='wknd.base'}`. Dit geeft aan dat de CSS voor de client met een categorie van `wknd.base` via dit bestand wordt opgenomen, waarbij **client-lib-base** in de koptekst van al onze pagina&#39;s wordt opgenomen.

1. Update `customheaderlibs.html` om een verwijzing naar de doopvontstijlen van Google op te nemen die wij eerder in **ui.frontend** module specificeerden.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspect het bestand `customfooterlibs.html`. Dit dossier, als `customheaderlibs.html` wordt bedoeld om door projecten te worden beschreven uit te voeren. Hier betekent de regel `${clientlib.js @ categories='wknd.base'}` dat het JavaScript van **clientlib-base** onder aan al onze pagina&#39;s wordt opgenomen.

1. Exporteer de `page`-component naar de AEM server met behulp van de ontwikkelaarsgereedschappen of met behulp van uw Maven-vaardigheden.

1. Blader naar de sjabloon voor artikelpagina op [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Klik op het pictogram **Pagina-informatie** en selecteer **Paginabeleid** in het menu om het dialoogvenster **Paginabeleid** te openen.

   ![Paginanummerbeleid voor artikelpaginasjabloon](assets/client-side-libraries/template-page-policy.png)

   *Paginagegevens > Paginabeleid*

1. De categorieën `wknd.dependencies` en `wknd.site` worden hier weergegeven. Standaard worden clientlibs die via het paginabeleid zijn geconfigureerd, gesplitst om de CSS op te nemen in de paginakop en de JavaScript op de tekstopmaak. Desgewenst kunt u expliciet vermelden dat de clientJavaScript in de paginakop moet worden geladen. Dit is het geval voor `wknd.dependencies`.

   ![Paginanummerbeleid voor artikelpaginasjabloon](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Het is ook mogelijk om direct naar `wknd.site` of `wknd.dependencies` van de paginacomponent te verwijzen, gebruikend `customheaderlibs.html` of `customfooterlibs.html` manuscript, zoals wij eerder voor `wknd.base` cliëntlib zagen. Het gebruik van de sjabloon biedt enige flexibiliteit omdat u kunt kiezen welke clientlibs per sjabloon worden gebruikt. Stel dat u een zeer zware JavaScript-bibliotheek hebt die alleen wordt gebruikt voor een geselecteerde sjabloon.

1. Navigeer naar de pagina **LA Skateparks** die is gemaakt met de **Article Page Template**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Er is een verschil in lettertypen.

1. Klik op het pictogram **Pagina-informatie** en selecteer **Weergeven als gepubliceerd** om de artikelpagina buiten de AEM-editor te openen.

   ![Weergeven als gepubliceerd](assets/client-side-libraries/view-as-published-article-page.png)

1. Bekijk de paginabron van [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) en u zou de volgende cliëntlib verwijzingen in `<head>` moeten kunnen zien:

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   Bericht dat de clientlibs de volmacht `/etc.clientlibs` eindpunt gebruiken. De volgende clientlib-include-bestanden vindt u onder aan de pagina:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Als deze op 6.5/6.4 volgen, worden de bibliotheken aan de clientzijde niet automatisch geminificeerd. Raadpleeg de documentatie bij [HTML-bibliotheekbeheer om mijnopruiming in te schakelen (aanbevolen)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Op de publicatiezijde is het van essentieel belang dat de clientbibliotheken **niet** worden aangeboden bij **/apps**, omdat dit pad om beveiligingsredenen moet worden beperkt met behulp van de [Dispatcher-filtersectie](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). De [allowProxy-eigenschap](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) van de clientbibliotheek zorgt ervoor dat de CSS en JS worden aangeboden via **/etc.clientlibs**.

## Webpack DevServer - Statische opmaakcodes {#webpack-dev-static}

In de vorige paar oefeningen konden wij verscheidene dossiers van de Klasse in **ui.frontend** module en door een bouwstijlproces bijwerken, uiteindelijk deze veranderingen zien die in AEM worden weerspiegeld. Vervolgens bekijken we technieken die een [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) gebruiken om snel onze front-end stijlen te ontwikkelen tegen **static** HTML.

Deze techniek is handig als de meeste stijlen en front-end code worden uitgevoerd door een speciale Front End-ontwikkelaar die mogelijk geen eenvoudige toegang heeft tot een AEM. Met deze techniek kan de FED ook direct wijzigingen aanbrengen in de HTML, die vervolgens kan worden overgedragen aan een AEM ontwikkelaar om te implementeren als componenten.

1. Kopieer de paginabron van de LA skatepark artikelpagina op [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Open uw IDE opnieuw. Plak de gekopieerde markering van AEM in `index.html` in **ui.frontend** module onder `src/main/webpack/static`.
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

1. Start de webpack-ontwikkelserver vanaf een nieuwe terminal met de volgende opdracht vanuit de module **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dit zou een nieuw browser venster bij [http://localhost:8080/](http://localhost:8080/) met statische prijsverhoging moeten openen.

1. Bewerk het bestand `src/main/webpack/site/_variables.scss`. Vervang de `$text-color` regel door het volgende:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Sla de wijzigingen op.

1. De wijzigingen worden automatisch weergegeven in de browser op [http://localhost:8080](http://localhost:8080).

   ![Wijzigingen in lokale webpack-server](assets/client-side-libraries/local-webpack-dev-server.png)

1. Controleer het `/aem-guides-wknd.ui.frontend/webpack.dev.js`-bestand. Dit bevat de webpack-configuratie die wordt gebruikt om de webpack-dev-server te starten. Merk op dat het de wegen `/content` en `/etc.clientlibs` van een plaatselijk lopende instantie van AEM voorziet. Op deze manier worden de afbeeldingen en andere clientlibs (niet beheerd door de **ui.frontend**-code) beschikbaar gemaakt.

   >[!CAUTION]
   >
   > De afbeeldingsbron van de statische markering verwijst naar een actieve afbeeldingscomponent op een lokale AEM. Afbeeldingen worden verbroken weergegeven als het pad naar de afbeelding verandert, als AEM niet is gestart of als de browser zich niet heeft aangemeld bij de lokale AEM. Als het afleveren aan een externe bron is het ook mogelijk om de beelden met statische verwijzingen te vervangen.

1. U kunt **stop** de webpack server van de bevellijn door `CTRL+C` te typen.

## Webpack DevServer - Controle en synchronisatie {#webpack-dev-watch}

Een andere techniek is om Node.js- controle voor om het even welke dossierveranderingen in de src- dossiers in de `ui.frontend` module te hebben. Wanneer een bestand wordt gewijzigd, wordt de clientbibliotheek snel gecompileerd en wordt de Npm-module [aemsync](https://www.npmjs.com/package/aemsync) gebruikt om de wijzigingen te synchroniseren met een actieve AEM.

1. Start de webpack dev-server in **watch**-modus vanuit een nieuwe terminal met de volgende opdracht vanuit de module **ui.frontend**:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. Hiermee worden de `src`-bestanden gecompileerd en worden de wijzigingen gesynchroniseerd met AEM op [http://localhost:4502](http://localhost:4502)

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. Navigeer naar AEM en het artikel LA Skateparks: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![Wijzigingen die zijn geïmplementeerd op AEM](assets/client-side-libraries/changes-deployed-aem-watch.png)

   De wijzigingen moeten worden toegepast op AEM. Er is een kleine vertraging en u moet de browser handmatig vernieuwen om de updates te kunnen zien. Het is echter nuttig om de wijzigingen rechtstreeks in AEM weer te geven als u met nieuwe componenten en het ontwerpen van dialoogvensters werkt.

1. Keer de verandering in `_variables.scss` terug en sparen de veranderingen. De wijzigingen moeten na een kleine vertraging weer worden gesynchroniseerd met de lokale AEM.

1. Stop de webpack dev-server en voer een volledige Maven-build uit vanuit de hoofdmap van het project:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Ook hier wordt de `ui.frontend` module gecompileerd, getransformeerd naar clientbibliotheken en geïmplementeerd op AEM via de `ui.apps` module. Maar deze keer doet Maven alles voor ons.

## Gefeliciteerd! {#congratulations}

De artikelpagina heeft nu enkele consistente stijlen die overeenkomen met het WKND-merk en u bent vertrouwd geraakt met de module **ui.frontend**!

### Volgende stappen {#next-steps}

Leer hoe te om individuele stijlen uit te voeren en de Componenten van de Kern te hergebruiken gebruikend het Systeem van de Stijl van de Experience Manager. [Het ontwikkelen met het Systeem van de Stijl ](style-system.md) behandelt het gebruiken van het Systeem van de Stijl om de Componenten van de Kern met merkspecifieke CSS en geavanceerde beleidsconfiguraties van de Redacteur van het Malplaatje uit te breiden.

Bekijk de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de schakelaar van de Git `tutorial/client-side-libraries-solution` op.

1. Clone the [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repository.
1. Bekijk de `tutorial/client-side-libraries-solution` vertakking.

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
