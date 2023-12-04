---
title: Clientbibliotheken en front-end workflow
description: Leer hoe u clientbibliotheken kunt gebruiken voor het implementeren en beheren van CSS en JavaScript voor een Adobe Experience Manager-implementatie (AEM) voor sites. Leer hoe de module ui.frontend, een webpack-project, kan worden geïntegreerd in het end-to-end buildproces.
version: 6.4, 6.5, Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 752
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2546'
ht-degree: 0%

---

# Clientbibliotheken en front-end workflow {#client-side-libraries}

Leer hoe Client-Side Libraries of clientlibs worden gebruikt om CSS en JavaScript voor een implementatie van Adobe Experience Manager (AEM) Plaatsen op te stellen en te beheren. In deze zelfstudie wordt ook uitgelegd hoe de [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) een ontkoppelde [webpack](https://webpack.js.org/) -project, kan worden geïntegreerd in het ontwikkelproces van begin tot eind.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment).

Het wordt ook aanbevolen de [Basisbeginselen van componenten](component-basics.md#client-side-libraries) zelfstudie om inzicht te krijgen in de grondbeginselen van bibliotheken en AEM aan de clientzijde.

### Starter-project

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide, kunt u het project opnieuw gebruiken en de stappen overslaan voor het uitchecken van het starterproject.

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Kijk uit de `tutorial/client-side-libraries-start` vertakking van [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > Indien u AEM 6.5 of 6.4 gebruikt, voegt u de `classic` aan om het even welke Gemaakt bevelen.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) of controleer de code plaatselijk door aan de tak over te schakelen `tutorial/client-side-libraries-solution`.

## Doelstellingen

1. Begrijp hoe clientbibliotheken via een bewerkbare sjabloon op een pagina worden opgenomen.
1. Leer hoe u de `ui.frontend` en een webpack-ontwikkelingsserver voor speciale front-end ontwikkeling.
1. Begrijp de werkstroom van begin tot eind van het leveren van gecompileerde CSS en JavaScript aan een implementatie van Plaatsen.

## Wat u gaat bouwen {#what-build}

In dit hoofdstuk voegt u enkele basislijnstijlen toe voor de WKND-site en de artikelpaginasjabloon om de implementatie dichter bij de [UI-ontwerpmodellen](assets/pages-templates/wknd-article-design.xd). U gebruikt een geavanceerde front-end werkschema om een webpack project in een AEM cliëntbibliotheek te integreren.

![Voltooide stijlen](assets/client-side-libraries/finished-styles.png)

*Artikelpagina met toegepaste basislijnstijlen*

## Achtergrond {#background}

Client-Side Libraries bieden een mechanisme voor het organiseren en beheren van CSS- en JavaScript-bestanden die nodig zijn voor een AEM Sites-implementatie. De basisdoelstellingen voor client-side bibliotheken of clientlibs zijn:

1. CSS/JS opslaan in kleine aparte bestanden voor eenvoudigere ontwikkeling en eenvoudig onderhoud
1. Afhankelijkheden van externe frameworks op georganiseerde wijze beheren
1. Minimaliseer het aantal cliënt-zijverzoeken door CSS/JS in één of twee verzoeken samen te voegen.

Meer informatie over het gebruik [Hier vindt u bibliotheken aan de clientzijde.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

Bibliotheken aan de clientzijde hebben enkele beperkingen. Het meest in het bijzonder is een beperkte ondersteuning voor populaire front-end talen zoals Sass, LESS en TypeScript. Laten we in de zelfstudie bekijken hoe de **ui.frontend** kan dit helpen oplossen.

Startcode naar lokale AEM-instantie implementeren en naar [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Deze pagina is niet opgemaakt. Laten we Client-side bibliotheken voor het WKND-merk implementeren om CSS en JavaScript aan de pagina toe te voegen.

## Client-Side Libraries-organisatie {#organization}

Verken nu de organisatie van clientlibs die worden gegenereerd door de [Projectarchetype AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![Clientbibliotheekorganisatie op hoog niveau](./assets/client-side-libraries/high-level-clientlib-organization.png)

*Het diagram van de cliënt-zijorganisatie van de Bibliotheek en paginaopneming op hoog niveau*

>[!NOTE]
>
> De volgende bibliotheekorganisatie aan de clientzijde wordt gegenereerd door AEM Project Archetype, maar vertegenwoordigt slechts een beginpunt. Hoe een project uiteindelijk CSS en JavaScript aan een implementatie van Plaatsen beheert en levert kan dramatisch variëren gebaseerd op middelen, vaardigheidsreeksen en vereisten.

1. Met VSCode of andere IDE opent u het dialoogvenster **ui.apps** -module.
1. Het pad uitbreiden `/apps/wknd/clientlibs` om de clientlibs te bekijken die door archetype worden geproduceerd.

   ![Clientlibs in ui.apps](assets/client-side-libraries/four-clientlib-folders.png)

   In de onderstaande sectie worden deze clientlibs nader bekeken.

1. De volgende tabel geeft een overzicht van de clientbibliotheken. Meer informatie over [inclusief clientbibliotheken vindt u hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | Naam | Beschrijving | Notities |
   |-------------------| ------------| ------|
   | `clientlib-base` | Basisniveau van CSS en JavaScript nodig voor WKND-site om te kunnen functioneren | Sluit de clientbibliotheken van de Core Component in |
   | `clientlib-grid` | Genereert de CSS die nodig is voor [Lay-outmodus](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) om te werken. | Mobiele/tabletonderbrekingspunten kunnen hier worden geconfigureerd |
   | `clientlib-site` | Bevat site-specifiek thema voor de WKND-site | Door de `ui.frontend` module |
   | `clientlib-dependencies` | Sluit om het even welke derdegebiedsdelen in | Door de `ui.frontend` module |

1. Waarnemen dat `clientlib-site` en `clientlib-dependencies` worden genegeerd van broncontrole. Dit is door ontwerp, aangezien deze bij bouwstijltijd door `ui.frontend` -module.

## Basisstijlen bijwerken {#base-styles}

Werk vervolgens de basisstijlen bij die in het dialoogvenster **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** -module. De bestanden in het dialoogvenster `ui.frontend` module genereren `clientlib-site` en `clientlib-dependecies` bibliotheken die het thema Site en eventuele afhankelijkheden van derden bevatten.

Clientbibliotheken bieden geen ondersteuning voor meer geavanceerde talen zoals [Soort](https://sass-lang.com/) of [TypeScript](https://www.typescriptlang.org/). Er zijn verschillende opensource-gereedschappen, zoals [NPM](https://www.npmjs.com/) en [webpack](https://webpack.js.org/) die de ontwikkeling aan de voorzijde versnellen en optimaliseren. Het doel van het **ui.frontend** moet deze hulpmiddelen kunnen gebruiken om de meeste front-end brondossiers te beheren.

1. Open de **ui.frontend** en navigeer naar `src/main/webpack/site`.
1. Het bestand openen `main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` is het ingangspunt voor de Sass-bestanden in het dialoogvenster `ui.frontend` -module. Het omvat de `_variables.scss` bestand, dat een reeks merkvariabelen bevat die in de verschillende bestanden voor de klasse in het project moeten worden gebruikt. De `_base.scss` Het bestand is ook opgenomen en definieert enkele basisstijlen voor HTML-elementen. Een reguliere expressie bevat de stijlen voor afzonderlijke componentstijlen onder `src/main/webpack/components`. Een andere reguliere expressie omvat de bestanden onder `src/main/webpack/site/styles`.

1. Het bestand Inspect `main.ts`. Hieronder vallen `main.scss` en een reguliere expressie om `.js` of `.ts` bestanden in het project. Dit ingangspunt wordt gebruikt door [webpack-configuratiebestanden](https://webpack.js.org/configuration/) als het ingangspunt voor de gehele `ui.frontend` -module.

1. De onderliggende bestanden Inspect `src/main/webpack/site/styles`:

   ![Stijlbestanden](assets/client-side-libraries/style-files.png)

   Deze bestanden zijn stijlen voor algemene elementen in de sjabloon, zoals Koptekst, Voettekst en container met hoofdinhoud. De CSS-regels in deze bestanden zijn gericht op verschillende HTML-elementen `header`, `main`, en  `footer`. Deze HTML-elementen zijn gedefinieerd door het beleid in het vorige hoofdstuk [Pagina&#39;s en sjablonen](./pages-templates.md).

1. Breid uit `components` map onder `src/main/webpack` en inspecteer de bestanden.

   ![Bestanden van componentklasse](assets/client-side-libraries/component-sass-files.png)

   Elk bestand wordt toegewezen aan een Core-component zoals de [Accordeoncomponent](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). Elke Core-component is gebouwd met [Element-wijziging blokkeren](https://getbem.com/) of BEM-notatie, zodat het eenvoudiger wordt om specifieke CSS-klassen met stijlregels als doel in te stellen. De onderliggende bestanden `/components` zijn door het AEM Project Archetype met de verschillende regels van BEM voor elke component geschrapt.

1. De WKND-basisstijlen downloaden **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** en **unzip** het bestand.

   ![WKND-basisstijlen](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Om de zelfstudie te versnellen, worden verschillende Sass-bestanden weergegeven die het WKND-merk implementeren op basis van Core Components en de structuur van het sjabloon voor artikelpagina.

1. De inhoud van `ui.frontend/src` met bestanden uit de vorige stap. De inhoud van de ZIP moet de volgende mappen overschrijven:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![Gewijzigde bestanden](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect de gewijzigde bestanden voor meer informatie over de WKND-stijlimplementatie.

## Inspect de integratie van ui.frontend {#ui-frontend-integration}

Een belangrijk integratieonderdeel dat in de **ui.frontend** module, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) neemt de gecompileerde CSS en JS artefacten van een webpack/npm project en zet hen in AEM cliënt-zijbibliotheken om.

![ui.frontend architectuurintegratie](assets/client-side-libraries/ui-frontend-architecture.png)

Deze integratie wordt automatisch ingesteld door het AEM Project Archetype. Ga vervolgens na hoe het werkt.


1. Open een opdrachtregelterminal en installeer de **ui.frontend** met de `npm install` opdracht:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` uitvoering is slechts eenmaal nodig , net als na een nieuwe kloon of generatie van het project .

1. Start de webpack-ontwikkelserver in **horloge** door de volgende opdracht uit te voeren:

   ```shell
   $ npm run watch
   ```

1. Hiermee worden de bronbestanden van de `ui.frontend` en synchroniseert de wijzigingen met AEM op [http://localhost:4502](http://localhost:4502)

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

1. De opdracht `npm run watch` vult uiteindelijk de **clientlib-site** en **clientlib-afhankelijkheden** in de **ui.apps** die dan automatisch met AEM wordt gesynchroniseerd.

   >[!NOTE]
   >
   >Er is ook een `npm run prod` profiel dat de JS en CSS minieme. Dit is de standaardcompilatie wanneer de webpack-build wordt geactiveerd via Maven. Meer informatie over de [ui.frontend module is hier te vinden](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Het bestand Inspect `site.css` beneide `ui.frontend/dist/clientlib-site/site.css`. Dit is gecompileerde CSS die op de brondossiers van de Klasse wordt gebaseerd.

   ![CSS voor gedistribueerde site](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Het bestand Inspect `ui.frontend/clientlib.config.js`. Dit is het configuratiebestand voor een npm-insteekmodule. [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) dat de inhoud van `/dist` naar een clientbibliotheek en naar de `ui.apps` -module.

1. Het bestand Inspect `site.css` in de **ui.apps** module bij `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. Dit moet een identieke kopie zijn van het `site.css` bestand van de **ui.frontend** -module. Nu is het **ui.apps** kan het aan AEM worden opgesteld.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Sinds **clientlib-site** wordt gecompileerd tijdens bouwstijltijd, gebruikend of **npm**, of **bemand**, kan het veilig door broncontrole in worden genegeerd **ui.apps** -module. Inspect the `.gitignore` bestand onder **ui.apps**.

1. Open het artikel van LA Skatepark in AEM op: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Bijgewerkte basisstijlen voor het artikel](assets/client-side-libraries/updated-base-styles.png)

   De bijgewerkte stijlen voor het artikel worden nu weergegeven. Mogelijk moet u op de een of andere manier vernieuwen om CSS-bestanden te wissen die in cache zijn geplaatst door de browser.

   Het begint veel dichter bij de mockups te komen!

   >[!NOTE]
   >
   > De stappen hierboven worden uitgevoerd om de code te bouwen en op te stellen ui.frontend aan AEM wordt uitgevoerd automatisch wanneer een Geleiende bouwstijl van de wortel van het project wordt teweeggebracht `mvn clean install -PautoInstallSinglePackage`.

## Een stijl wijzigen

Breng vervolgens een kleine wijziging aan in de `ui.frontend` om de `npm run watch` Implementeer de stijlen automatisch in de lokale AEM.

1. Van, `ui.frontend` het bestand openen: `ui.frontend/src/main/webpack/site/_variables.scss`.
1. Werk de `$brand-primary` kleurvariabele:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Sla de wijzigingen op.

1. Ga terug naar de browser en vernieuw de AEM pagina om de updates te zien:

   ![Client Side Libraries](assets/client-side-libraries/style-update-brand-primary.png)

1. De wijziging in de `$brand-primary` kleur en stop de webpack-build met de opdracht `CTRL+C`.

>[!CAUTION]
>
> Het gebruik van **ui.frontend** is niet voor alle projecten noodzakelijk. De **ui.frontend** voegt extra complexiteit toe en als het niet nodig of gewenst is om een aantal van deze geavanceerde front-end tools (Sass, webpack, npm...) te gebruiken, is dit wellicht niet nodig.

## Pagina- en sjabloonopname {#page-inclusion}

Daarna, herzien hoe de clientlibs in de Pagina van de AEM van verwijzingen worden voorzien. De beste manier bij webontwikkeling is om CSS op te nemen in de koptekst van de HTML `<head>` en JavaScript vlak voor het sluiten `</body>` -tag.

1. Blader naar de sjabloon Artikelpagina op [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Klik op de knop **Pagina-informatie** en in het menu dat u selecteert **Paginabeleid** om de **Paginabeleid** in.

   ![Paginanummerbeleid voor artikelpaginasjabloon](assets/client-side-libraries/template-page-policy.png)

   *Paginagegevens > Paginabeleid*

1. U ziet dat de categorieën voor `wknd.dependencies` en `wknd.site` worden hier weergegeven. Standaard worden clientlibs die via het paginabeleid zijn geconfigureerd, gesplitst om de CSS op te nemen in de paginakop en de JavaScript op de tekstopmaak. U kunt expliciet vermelden welke clientlib-JavaScript in de paginakop moet worden geladen. Dit is het geval voor `wknd.dependencies`.

   ![Paginanummerbeleid voor artikelpaginasjabloon](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Het is ook mogelijk te verwijzen naar de `wknd.site` of `wknd.dependencies` rechtstreeks vanuit de paginacomponent, met de `customheaderlibs.html` of `customfooterlibs.html` script. Het gebruik van de sjabloon biedt flexibiliteit omdat u kunt kiezen welke clientlibs per sjabloon worden gebruikt. Als u bijvoorbeeld een zware JavaScript-bibliotheek hebt die alleen wordt gebruikt voor een geselecteerde sjabloon.

1. Ga naar de **LA Skateparks** pagina gemaakt met de **Artikelpaginasjabloon**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Klik op de knop **Pagina-informatie** en in het menu dat u selecteert **Weergeven als gepubliceerd** om de artikelpagina buiten de AEM-editor te openen.

   ![Weergeven als gepubliceerd](assets/client-side-libraries/view-as-published-article-page.png)

1. De paginabron weergeven van [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) en u zou de volgende clientlib verwijzingen in moeten kunnen zien `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   De clientlibs gebruiken de proxy `/etc.clientlibs` eindpunt. U zou ook moeten zien dat volgende clientlib bij de bodem van de pagina omvat:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Voor AEM 6.5/6.4 worden de bibliotheken aan de clientzijde niet automatisch geminiateerd. Zie de documentatie op de [HTML Library Manager om miniaturen in te schakelen (aanbevolen)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >Het is essentieel aan de publicatiezijde dat de cliëntbibliotheken zijn **niet** aangedaan door **/apps** aangezien dit pad om veiligheidsredenen beperkt moet worden met behulp van de [Sectie voor het filter Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). De [allowProxy, eigenschap](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) van de clientbibliotheek zorgt ervoor dat de CSS en JS worden aangeboden vanaf **/etc.clientlibs**.

### Volgende stappen {#next-steps}

Leer hoe te om individuele stijlen uit te voeren en de Componenten van de Kern te hergebruiken gebruikend het Systeem van de Stijl van de Experience Manager. [Ontwikkelen met het Stijlsysteem](style-system.md) behandelt het gebruiken van het Systeem van de Stijl om de Componenten van de Kern met merkspecifieke CSS en geavanceerde beleidsconfiguraties van de Redacteur van het Malplaatje uit te breiden.

De voltooide code weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van de it op `tutorial/client-side-libraries-solution`.

1. Klonen met [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) opslagplaats.
1. Kijk uit de `tutorial/client-side-libraries-solution` vertakking.

## Aanvullende hulpmiddelen en bronnen {#additional-resources}

### Webpack DevServer - Statische opmaakcodes {#webpack-dev-static}

In de voorgaande paar oefeningen zijn verschillende bestanden met een score opgenomen in de **ui.frontend** werden bijgewerkt en door een bouwstijlproces, uiteindelijk zie dat deze veranderingen in AEM weerspiegeld. Kijk nu naar een techniek die een [webpack-server](https://webpack.js.org/configuration/dev-server/) snel de front-end stijlen te ontwikkelen tegen **static** HTML.

Deze techniek is handig als de meeste stijlen en front-end code worden uitgevoerd door een toegewijde front-end ontwikkelaar die mogelijk geen eenvoudige toegang tot een AEM omgeving heeft. Met deze techniek kan de FED ook direct wijzigingen aanbrengen aan de HTML, die vervolgens aan een AEM ontwikkelaar kan worden overgedragen om als onderdelen te implementeren.

1. Kopieer de paginabron van de artikelpagina van het LA skatepark op [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Open uw IDE opnieuw. Plak de gekopieerde markering van AEM in de `index.html` in de **ui.frontend** module onder `src/main/webpack/static`.
1. De gekopieerde markering bewerken en verwijzingen verwijderen naar **clientlib-site** en **clientlib-afhankelijkheden**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Verwijder deze referenties omdat de webpack-ontwikkelserver deze artefacten automatisch genereert.

1. Start de webpack dev-server vanaf een nieuwe terminal met de volgende opdracht vanuit de **ui.frontend** module:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Hiermee opent u een nieuw browservenster op [http://localhost:8080/](http://localhost:8080/) met statische opmaakcodes.

1. Het bestand bewerken `src/main/webpack/site/_variables.scss` bestand. Vervang de `$text-color` regel met het volgende:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Sla de wijzigingen op.

1. De wijzigingen worden automatisch in de browser weergegeven [http://localhost:8080](http://localhost:8080).

   ![Wijzigingen in lokale webpack-server](assets/client-side-libraries/local-webpack-dev-server.png)

1. Controleer de `/aem-guides-wknd.ui.frontend/webpack.dev.js` bestand. Dit bevat de webpack-configuratie die wordt gebruikt om de webpack-dev-server te starten. De paden worden gevuld `/content` en `/etc.clientlibs` van een lokaal actieve instantie van AEM. Zo worden de afbeeldingen en andere clientlibs (niet beheerd door de **ui.frontend** code) beschikbaar worden gesteld.

   >[!CAUTION]
   >
   > De afbeeldingsbron van de statische markering verwijst naar een actieve afbeeldingscomponent op een lokale AEM. Afbeeldingen worden verbroken weergegeven als het pad naar de afbeelding verandert, als AEM niet is gestart of als de browser zich niet heeft aangemeld bij de lokale AEM. Als u de afbeelding naar een externe bron afgeeft, is het ook mogelijk de afbeeldingen te vervangen door statische verwijzingen.

1. U kunt **stoppen** de webpack-server vanaf de opdrachtregel door te typen `CTRL+C`.

### geaëerd {#develop-aemfed}

**[geaëerd](https://aemfed.io/)** is een open-bron, bevel-lijn hulpmiddel dat kan worden gebruikt om front-end ontwikkeling te versnellen. Het wordt aangedreven door [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://browsersync.io/), en [Sling Log Tracker](https://sling.apache.org/documentation/bundles/log-tracers.html).

Op hoog niveau `aemfed`is ontworpen om te luisteren naar bestandswijzigingen binnen het **ui.apps** en deze automatisch rechtstreeks te synchroniseren met een actieve AEM. Op basis van de wijzigingen wordt een lokale browser automatisch vernieuwd, waardoor de ontwikkeling aan de voorkant sneller gaat. Het wordt ook gebouwd om met de Traceur van het Logboek van het Sling te werken om het even welke server-zijfouten direct in de terminal automatisch te tonen.

Als u binnen de **ui.apps** modules, HTML-scripts wijzigen en aangepaste componenten maken, **geaëerd** kan een krachtig hulpmiddel zijn om te gebruiken. [Hier vindt u volledige documentatie](https://github.com/abmaonline/aemfed).

### Fouten opsporen in clientbibliotheken {#debugging-clientlibs}

Verschillende methoden gebruiken **categorieën** en **insluiten** om veelvoudige cliëntbibliotheken te omvatten kan het lastig zijn om problemen op te lossen. AEM stelt verschillende hulpmiddelen beschikbaar om hierbij te helpen. Een van de belangrijkste instrumenten is **Client-bibliotheken opnieuw samenstellen** waarbij AEM alle LESS-bestanden opnieuw compileren en de CSS genereren.

* [**Reliëfstructuren**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Hier worden de clientbibliotheken weergegeven die in de AEM zijn geregistreerd. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Uitvoer testen**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - stelt een gebruiker in staat de verwachte HTML-uitvoer van clientlib te zien op basis van categorie. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**Validatie van bibliotheekafhankelijke onderdelen**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - Hiermee worden afhankelijkheden of ingesloten categorieën gemarkeerd die niet zijn gevonden. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**Client-bibliotheken opnieuw samenstellen**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - kan een gebruiker de AEM dwingen de clientbibliotheken opnieuw op te bouwen of de cache van clientbibliotheken ongeldig te maken. Dit gereedschap is effectief bij het ontwikkelen met LESS, omdat dit AEM ertoe kan dwingen de gegenereerde CSS opnieuw te compileren. Over het algemeen is het effectiever om de cache ongeldig te maken en vervolgens een pagina te vernieuwen in plaats van de bibliotheken opnieuw samen te stellen. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![clientbibliotheek opnieuw samenstellen](assets/client-side-libraries/rebuild-clientlibs.png)
