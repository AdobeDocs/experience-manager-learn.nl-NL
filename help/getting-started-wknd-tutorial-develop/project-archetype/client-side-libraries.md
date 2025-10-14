---
title: Clientbibliotheken en front-end workflow
description: Leer hoe u clientbibliotheken kunt gebruiken voor de implementatie van CSS en JavaScript voor een Adobe Experience Manager (AEM)-site. Leer hoe de module ui.frontend, een webpack-project, kan worden geïntegreerd in het end-to-end buildproces.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 557
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '2432'
ht-degree: 0%

---

# Clientbibliotheken en front-end workflow {#client-side-libraries}

Leer hoe Client-Side Libraries of clientlibs worden gebruikt om CSS en JavaScript voor een implementatie van Adobe Experience Manager (AEM) Plaatsen op te stellen en te beheren. Dit leerprogramma behandelt ook hoe de {[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=nl-NL) module 0} ui.frontend, een ontkoppelde [&#x200B; webpack &#x200B;](https://webpack.js.org/) project, in het bouwstijlproces van begin tot eind kan worden geïntegreerd.

## Vereisten {#prerequisites}

Herzie het vereiste tooling en de instructies voor vestiging a [&#x200B; lokale ontwikkelomgeving &#x200B;](overview.md#local-dev-environment).

Het wordt ook geadviseerd om de [&#x200B; Basisbeginselen van de Component &#x200B;](component-basics.md#client-side-libraries) leerprogramma te herzien om de grondbeginselen van cliënt-zijbibliotheken en AEM te begrijpen.

### Starter-project

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide, kunt u het project opnieuw gebruiken en de stappen overslaan voor het uitchecken van het starterproject.

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Controle uit de `tutorial/client-side-libraries-start` tak van [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Stel codebasis aan een lokale instantie van AEM op gebruikend uw Maven vaardigheden:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Als u AEM 6.5 of 6.4 gebruikt, voegt u het `classic` -profiel toe aan Maven-opdrachten.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) altijd bekijken of de code plaatselijk controleren door aan de tak `tutorial/client-side-libraries-solution` te schakelen.

## Doelstellingen

1. Begrijp hoe clientbibliotheken via een bewerkbare sjabloon op een pagina worden opgenomen.
1. Leer hoe u de module `ui.frontend` en een webpack-ontwikkelingsserver gebruikt voor speciale front-end ontwikkeling.
1. Begrijp de werkschema van begin tot eind van het leveren van gecompileerde CSS en JavaScript aan een implementatie van Plaatsen.

## Wat u gaat bouwen {#what-build}

In dit hoofdstuk, voegt u sommige basislijnstijlen voor de plaats WKND en het Malplaatje van de Pagina van het Artikel toe om de implementatie dichter aan de [&#x200B; UI ontwerpmodellen &#x200B;](assets/pages-templates/wknd-article-design.xd) te brengen. U gebruikt een geavanceerde front-end workflow om een webpack-project te integreren in een AEM-clientbibliotheek.

![&#x200B; Voltooide Stijlen &#x200B;](assets/client-side-libraries/finished-styles.png)

*de Pagina van het Artikel met toegepaste basislijnstijlen*

## Achtergrond {#background}

Client-Side Libraries bieden een mechanisme voor het organiseren en beheren van CSS- en JavaScript-bestanden die nodig zijn voor een AEM Sites-implementatie. De basisdoelstellingen voor client-side bibliotheken of clientlibs zijn:

1. CSS/JS opslaan in kleine aparte bestanden voor eenvoudigere ontwikkeling en eenvoudig onderhoud
1. Afhankelijkheden van externe frameworks op georganiseerde wijze beheren
1. Minimaliseer het aantal cliënt-zijverzoeken door CSS/JS in één of twee verzoeken samen te voegen.

Meer informatie over het gebruiken van [&#x200B; cliënt-Kant Bibliotheken kan hier worden gevonden.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=nl-NL)

Bibliotheken aan de clientzijde hebben enkele beperkingen. Het meest in het bijzonder is een beperkte ondersteuning voor populaire front-end talen zoals Sass, LESS en TypeScript. In het leerprogramma, kijken hoe de {**module 0} ui.frontend dit kan helpen oplossen.**

Stel de basis van de startcode aan een lokale instantie van AEM op en navigeer aan [&#x200B; http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html &#x200B;](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). Deze pagina is niet opgemaakt. Laten we Client-side bibliotheken voor het WKND-merk implementeren om CSS en JavaScript aan de pagina toe te voegen.

## Client-Side Libraries-organisatie {#organization}

Daarna onderzoeken wij de organisatie van clientlibs die door het [&#x200B; Archetype van het Project van AEM &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=nl-NL) worden geproduceerd.

![&#x200B; Clientlibrary van hoog niveau organisatie &#x200B;](./assets/client-side-libraries/high-level-clientlib-organization.png)

*de organisatie van de cliënt-zijBibliotheek van het diagram van het hoog-niveau en paginaconclusie*

>[!NOTE]
>
> De volgende bibliotheekorganisatie aan de clientzijde wordt gegenereerd door AEM Project Archetype, maar vormt slechts een beginpunt. Hoe een project uiteindelijk CSS en JavaScript aan een implementatie van Plaatsen beheert en levert kan dramatisch variëren gebaseerd op middelen, vaardigheidsreeksen en vereisten.

1. Het gebruiken van VSCode of andere winde opent omhoog de {**module 0} ui.apps.**
1. Breid de weg `/apps/wknd/clientlibs` uit om de clientlibs te bekijken die door archetype worden geproduceerd.

   ![&#x200B; Clientlibs in ui.apps &#x200B;](assets/client-side-libraries/four-clientlib-folders.png)

   In de onderstaande sectie worden deze clientlibs nader bekeken.

1. De volgende tabel geeft een overzicht van de clientbibliotheken. Meer details over [&#x200B; met inbegrip van de Bibliotheken van de Cliënt kunnen hier &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=nl-NL#developing) worden gevonden.

   | Naam | Beschrijving | Notities |
   |-------------------| ------------| ------|
   | `clientlib-base` | Basisniveau van CSS en JavaScript vereist voor WKND-site om te kunnen functioneren | Sluit de clientbibliotheken van de Core Component in |
   | `clientlib-grid` | Produceert CSS noodzakelijk voor [&#x200B; Wijze van de Lay-out &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=nl-NL) om te werken. | Mobiele/tabletonderbrekingspunten kunnen hier worden geconfigureerd |
   | `clientlib-site` | Bevat site-specifiek thema voor de WKND-site | Gegenereerd door de module `ui.frontend` |
   | `clientlib-dependencies` | Sluit om het even welke derdegebiedsdelen in | Gegenereerd door de module `ui.frontend` |

1. Let op: `clientlib-site` en `clientlib-dependencies` worden genegeerd vanuit bronbesturing. Dit is door ontwerp, aangezien deze bij bouwstijltijd door de `ui.frontend` module worden geproduceerd.

## Basisstijlen bijwerken {#base-styles}

Daarna, werk de basisstijlen bij die in **[worden bepaald ui.frontend &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=nl-NL)** module. De bestanden in de module `ui.frontend` genereren de bibliotheken `clientlib-site` en `clientlib-dependecies` met het thema Site en eventuele afhankelijkheden van derden.

De cliënt-zijbibliotheken steunen geen geavanceerdere talen zoals [&#x200B; Sass &#x200B;](https://sass-lang.com/) of [&#x200B; TypeScript &#x200B;](https://www.typescriptlang.org/). Er zijn verscheidene open-bronhulpmiddelen zoals [&#x200B; NPM &#x200B;](https://www.npmjs.com/) en [&#x200B; webpack &#x200B;](https://webpack.js.org/) die vooruitstrevende ontwikkeling versnellen en optimaliseren. Het doel van de {**module 0} ui.frontend moet deze hulpmiddelen kunnen gebruiken om de meeste front-end brondossiers te beheren.**

1. Open omhoog de {**module 0} ui.frontend en navigeer aan `src/main/webpack/site`.**
1. Het bestand openen `main.scss`

   ![&#x200B; main.scss - ingangspunt &#x200B;](assets/client-side-libraries/main-scss.png)

   `main.scss` is het ingangspunt voor de dossiers van de Klasse in de `ui.frontend` module. Het bevat het bestand `_variables.scss` , dat een reeks merkvariabelen bevat die in de verschillende geluidsbestanden van het project moeten worden gebruikt. Het bestand `_base.scss` is ook opgenomen en definieert enkele basisstijlen voor HTML-elementen. Een reguliere expressie bevat de stijlen voor afzonderlijke componentstijlen onder `src/main/webpack/components` . Een andere reguliere expressie bevat de bestanden onder `src/main/webpack/site/styles` .

1. Controleer het bestand `main.ts` . Het bevat `main.scss` en een reguliere expressie voor het verzamelen van `.js` - of `.ts` -bestanden in het project. Dit ingangspunt wordt gebruikt door de [&#x200B; dossiers van de webpakconfiguratie &#x200B;](https://webpack.js.org/configuration/) als ingangspunt voor de volledige `ui.frontend` module.

1. Controleer de bestanden onder `src/main/webpack/site/styles` :

   ![&#x200B; de Dossiers van de Stijl &#x200B;](assets/client-side-libraries/style-files.png)

   Deze bestanden zijn stijlen voor algemene elementen in de sjabloon, zoals Koptekst, Voettekst en container met hoofdinhoud. De CSS-regels in deze bestanden zijn gericht op verschillende HTML-elementen `header` , `main` en `footer` . Deze elementen van HTML werden bepaald door beleid in het vorige hoofdstuk [&#x200B; Pagina&#39;s en Malplaatjes &#x200B;](./pages-templates.md).

1. Vouw de map `components` onder `src/main/webpack` uit en inspecteer de bestanden.

   ![&#x200B; de dossiers van de Klasse van de Component &#x200B;](assets/client-side-libraries/component-sass-files.png)

   Elk dossier brengt aan een Component van de Kern zoals de [&#x200B; Component van de Accordeon &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=nl-NL) in kaart. Elke Component van de Kern wordt gebouwd met [&#x200B; Modifier van het Element van het Blok &#x200B;](https://getbem.com/) of BEM aantekening om het gemakkelijker te maken om specifieke CSS klassen met stijlregels te richten. De bestanden onder `/components` zijn door het AEM Project Archetype uitgesneden met de verschillende BEM-regels voor elke component.

1. Download de Stijlen van de Basis WKND **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** en **unzip** het dossier.

   ![&#x200B; WKND de Stijlen van de Basis &#x200B;](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   Om de zelfstudie te versnellen, worden verschillende Sass-bestanden weergegeven die het WKND-merk implementeren op basis van Core Components en de structuur van het sjabloon voor artikelpagina.

1. Overschrijf de inhoud van `ui.frontend/src` met bestanden uit de vorige stap. De inhoud van de ZIP moet de volgende mappen overschrijven:

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![&#x200B; Gewijzigde dossiers &#x200B;](assets/client-side-libraries/changed-files-uifrontend.png)

   Controleer de gewijzigde bestanden om details van de WKND-stijlimplementatie te bekijken.

## De integratie ui.frontend controleren {#ui-frontend-integration}

Een zeer belangrijk integratiestuk dat in **wordt gebouwd ui.frontend** module, [&#x200B; aem-clientlib-generator &#x200B;](https://github.com/wcm-io-frontend/aem-clientlib-generator) neemt gecompileerde CSS en JS artefacten van een webpack/npm project en zet hen in cliënt-zijbibliotheken van AEM om.

![&#x200B; ui.frontend architectuurintegratie &#x200B;](assets/client-side-libraries/ui-frontend-architecture.png)

Deze integratie wordt automatisch ingesteld door het AEM Project Archetype. Ga vervolgens na hoe het werkt.


1. Open een bevel-lijn terminal en installeer **ui.frontend** module gebruikend het `npm install` bevel:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` hoeft slechts eenmaal te worden uitgevoerd, net als na een nieuwe kloon of generatie van het project.

1. Open `ui.frontend/package.json` en in het **manuscripten** **begin** bevel voegt `--env writeToDisk=true` toe.

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. Begin de webpack dev server op **horloge** wijze door het volgende bevel in werking te stellen:

   ```shell
   $ npm run watch
   ```

1. Dit compileert de brondossiers van de `ui.frontend` module en synchroniseert de veranderingen met AEM in [&#x200B; http://localhost:4502 &#x200B;](http://localhost:4502)

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

1. Het bevel `npm run watch` bevolkt uiteindelijk de **clientlib-plaats** en **client-afhankelijkheden** in de **ui.apps** module die dan automatisch met AEM wordt gesynchroniseerd.

   >[!NOTE]
   >
   >Er is ook een `npm run prod` -profiel dat de JS en CSS minieme waarden geeft. Dit is de standaardcompilatie wanneer de webpack-build wordt geactiveerd via Maven. Meer details over de {[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=nl-NL) kunnen worden gevonden module 0} ui.frontend.

1. Controleer het bestand `site.css` onder `ui.frontend/dist/clientlib-site/site.css` . Dit is gecompileerde CSS die op de brondossiers van de Klasse wordt gebaseerd.

   ![&#x200B; Verdeelde Plaats CSS &#x200B;](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Controleer het bestand `ui.frontend/clientlib.config.js` . Dit is het configuratiedossier voor een npm stop, [&#x200B; aem-clientlib-generator &#x200B;](https://github.com/wcm-io-frontend/aem-clientlib-generator) die de inhoud van `/dist` in een cliëntbibliotheek omzet en het naar de `ui.apps` module verplaatst.

1. Inspecteer het dossier `site.css` in de {**module 1} ui.apps bij `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`.** Dit zou een identiek exemplaar van het `site.css` dossier van de {**module 1} ui.frontend moeten zijn.** Nu het in **ui.apps** module is kan het aan AEM worden opgesteld.

   ![&#x200B; ui.apps clientlib-plaats &#x200B;](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > Aangezien **client-plaats** tijdens bouwstijltijd wordt gecompileerd, gebruikend of **npm**, of **gemandateerd**, kan het veilig van broncontrole in de **ui.apps** module worden genegeerd. Inspecteer het `.gitignore` dossier onder **ui.apps**.

1. Open het artikel van LA Skatepark in AEM in: [&#x200B; http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html &#x200B;](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![&#x200B; Bijgewerkte Stijlen van de Basis voor het Artikel &#x200B;](assets/client-side-libraries/updated-base-styles.png)

   De bijgewerkte stijlen voor het artikel worden nu weergegeven. Mogelijk moet u op de een of andere manier vernieuwen om CSS-bestanden te wissen die in cache zijn geplaatst door de browser.

   Het begint veel dichter bij de mockups te komen!

   >[!NOTE]
   >
   > De stappen hierboven worden uitgevoerd om de code ui.frontend aan AEM te bouwen en op te stellen worden automatisch uitgevoerd wanneer een Maven bouwstijl van de wortel van het project `mvn clean install -PautoInstallSinglePackage` wordt teweeggebracht.

## Een stijl wijzigen

Breng vervolgens een kleine wijziging aan in de module `ui.frontend` om te zien hoe `npm run watch` de stijlen automatisch implementeert in de lokale AEM-instantie.

1. Open vanuit de module `ui.frontend` het bestand: `ui.frontend/src/main/webpack/site/_variables.scss` .
1. Werk de kleurvariabele `$brand-primary` bij:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   Sla de wijzigingen op.

1. Ga terug naar de browser en vernieuw de AEM-pagina om de updates te zien:

   ![&#x200B; de Zijbibliotheken van de Cliënt &#x200B;](assets/client-side-libraries/style-update-brand-primary.png)

1. Vorige versie de wijziging in de `$brand-primary` -kleur en stop de webpack-bewerking met de opdracht `CTRL+C` .

>[!CAUTION]
>
> Het gebruik van de {**module 0} ui.frontend kan niet noodzakelijk voor alle projecten zijn.** De {**module 0} ui.frontend voegt extra ingewikkeldheid toe en als er geen behoefte/verlangen is om sommige van deze geavanceerde voorste hulpmiddelen (Sass, webpack, npm..) te gebruiken het kan niet nodig zijn.**

## Pagina- en sjabloonopname {#page-inclusion}

Laten we nu bekijken hoe naar de clientlibs wordt verwezen in de AEM-pagina. Bij webontwikkeling kunt u het beste CSS direct in de HTML Header `<head>` en JavaScript opnemen voordat u de tag `</body>` sluit.

1. Blader naar het malplaatje van de Pagina van het Artikel in [&#x200B; http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. Klik het **pictogram van de Informatie van de Pagina** en in het menu uitgezocht **Beleid van de Pagina** om de **dialoog van het Beleid van de Pagina te openen**.

   ![&#x200B; het Beleid van de Pagina van het Malplaatje van het Artikel van het Menu van de Pagina &#x200B;](assets/client-side-libraries/template-page-policy.png)

   *de Informatie van de Pagina > Beleid van de Pagina*

1. De categorieën `wknd.dependencies` en `wknd.site` worden hier weergegeven. Standaard worden clientlibs die via het paginabeleid zijn geconfigureerd, gesplitst om de CSS op te nemen in de paginakop en de JavaScript op het hoofdeinde. U kunt de clientlib-JavaScript die in de paginakop moet worden geladen expliciet weergeven. Dit geldt voor `wknd.dependencies` .

   ![&#x200B; het Beleid van de Pagina van het Malplaatje van het Artikel van het Menu van de Pagina &#x200B;](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > Het is ook mogelijk rechtstreeks naar de `wknd.site` - of `wknd.dependencies` -pagina te verwijzen vanuit de paginacomponent met behulp van het `customheaderlibs.html` - of `customfooterlibs.html` -script. Het gebruik van de sjabloon biedt flexibiliteit omdat u kunt kiezen welke clientlibs per sjabloon worden gebruikt. Bijvoorbeeld, als u een zware bibliotheek van JavaScript hebt die slechts op een uitgezochte malplaatje zal worden gebruikt.

1. Navigeer aan **LA Skateparks** pagina die gebruikend het **Malplaatje van de Pagina van het Artikel** wordt gecreeerd: [&#x200B; http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html &#x200B;](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Klik het **pictogram van de Informatie van de Pagina** en in het menu uitgezocht **Mening zoals Gepubliceerd** om de artikelpagina buiten de Redacteur van AEM te openen.

   ![&#x200B; Mening zoals Gepubliceerd &#x200B;](assets/client-side-libraries/view-as-published-article-page.png)

1. Bekijk de bron van de Pagina van [&#x200B; http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled &#x200B;](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) en u zou de volgende clientlib verwijzingen in `<head>` moeten kunnen zien:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   De clientlibs gebruiken het proxy `/etc.clientlibs` eindpunt. U zou ook moeten zien dat volgende clientlib bij de bodem van de pagina omvat:

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > Voor AEM 6.5/6.4 worden de bibliotheken aan de clientzijde niet automatisch geminiateerd. Zie de documentatie op de [&#x200B; Manager van de Bibliotheek van HTML om (geadviseerde) minificatie toe te laten &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=nl-NL#using-preprocessors).

   >[!WARNING]
   >
   >Het is kritiek op publiceer kant dat de cliëntbibliotheken **niet** van **worden gediend/apps** aangezien deze weg om veiligheidsredenen zou moeten worden beperkt gebruikend de [&#x200B; de filtersectie van Dispatcher &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#example-filter-section). Het [&#x200B; allowProxy bezit &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=nl-NL#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) van de cliëntbibliotheek verzekert CSS en JS van **/etc.clientlibs** worden gediend.

### Volgende stappen {#next-steps}

Leer hoe u afzonderlijke stijlen implementeert en Core Components opnieuw gebruikt met Experience Manager Style System. [&#x200B; het ontwikkelen met het Systeem van de Stijl &#x200B;](style-system.md) behandelt het gebruiken van het Systeem van de Stijl om de Componenten van de Kern met merkspecifieke CSS en geavanceerde beleidsconfiguraties van de Redacteur van het Malplaatje uit te breiden.

Bekijk de gebeëindigde code op [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van het Git `tutorial/client-side-libraries-solution` op.

1. Kloon de [&#x200B; github.com/adobe/aem-wknd-guides &#x200B;](https://github.com/adobe/aem-guides-wknd) bewaarplaats.
1. Bekijk de `tutorial/client-side-libraries-solution` -vertakking.

## Aanvullende hulpmiddelen en bronnen {#additional-resources}

### Webpack DevServer - Statische opmaakcodes {#webpack-dev-static}

In de vorige paar oefeningen werden verscheidene dossiers van de Klasse in de {**module 0} ui.frontend bijgewerkt en door een bouwstijlproces, uiteindelijk zien dat deze veranderingen in AEM weerspiegeld.** Daarna kijken naar een techniek die a [&#x200B; webpack-dev-server &#x200B;](https://webpack.js.org/configuration/dev-server/) gebruikt om de front-end stijlen tegen **statische** HTML snel te ontwikkelen.

Deze techniek is handig als de meeste stijlen en front-end code worden uitgevoerd door een toegewijde front-end ontwikkelaar die mogelijk geen eenvoudige toegang heeft tot een AEM-omgeving. Met deze techniek kan de FED ook direct wijzigingen aanbrengen aan de HTML, die vervolgens kan worden overgedragen aan een AEM-ontwikkelaar om te implementeren als onderdelen.

1. Kopieer de paginabron van de LA het artikelpagina van het skatepark in [&#x200B; http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled &#x200B;](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. Open uw IDE opnieuw. Plak de gekopieerde prijsverhoging van AEM in `index.html` in de {**module 1} ui.frontend onder `src/main/webpack/static`.**
1. Bewerk de gekopieerde prijsverhoging en verwijder om het even welke verwijzingen naar **clientlib-plaats** en **clientlib-gebiedsdelen**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Verwijder deze referenties omdat de webpack-ontwikkelserver deze artefacten automatisch genereert.

1. Begin de webpack dev server van een nieuwe terminal door het volgende bevel van binnen de {**module 0} in werking te stellen ui.frontend:**

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. Dit zou een nieuw browser venster in [&#x200B; http://localhost:8080/ &#x200B;](http://localhost:8080/) met statische prijsverhoging moeten openen.

1. Bewerk het bestand `src/main/webpack/site/_variables.scss` . Vervang de `$text-color` -regel door:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   Sla de wijzigingen op.

1. U zou automatisch de veranderingen moeten zien die automatisch in browser op [&#x200B; http://localhost:8080 &#x200B;](http://localhost:8080) worden weerspiegeld.

   ![&#x200B; Lokale webpack dev serververanderingen &#x200B;](assets/client-side-libraries/local-webpack-dev-server.png)

1. Controleer het `/aem-guides-wknd.ui.frontend/webpack.dev.js` -bestand. Dit bevat de webpack-configuratie die wordt gebruikt om de webpack-dev-server te starten. De paden `/content` en `/etc.clientlibs` worden doorgegeven vanuit een lokaal actieve instantie van AEM. Dit is hoe de beelden, en andere clientlibs (die niet door **worden beheerd ui.frontend** code) ter beschikking worden gesteld.

   >[!CAUTION]
   >
   > De afbeeldingsbron van de statische markering verwijst naar een live-afbeeldingscomponent op een lokale AEM-instantie. Afbeeldingen worden verbroken weergegeven als het pad naar de afbeelding verandert, als AEM niet wordt gestart of als de browser zich niet heeft aangemeld bij de lokale AEM-instantie. Als u de afbeelding naar een externe bron afgeeft, is het ook mogelijk de afbeeldingen te vervangen door statische verwijzingen.

1. U kunt **tegenhouden** de webpack server van de bevellijn door `CTRL+C` te typen.

### Fouten opsporen in clientbibliotheken {#debugging-clientlibs}

Het gebruiken van verschillende methodes van **categorieën** en **sluit** in om veelvoudige cliëntbibliotheken te omvatten het kan lastig zijn om problemen op te lossen. AEM stelt verschillende hulpmiddelen beschikbaar om hierbij te helpen. Één van de belangrijkste hulpmiddelen is **opnieuw bouwt de Bibliotheken van de Cliënt** die AEM dwingt om om het even welke LESS dossiers opnieuw te compileren en CSS te produceren.

* [**de Libben van de Reliëf** &#x200B;](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - maakt een lijst van de cliëntbibliotheken die in de instantie van AEM worden geregistreerd. `<host>/libs/granite/ui/content/dumplibs.html`

* [**Uitvoer van de Test** &#x200B;](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - staat een gebruiker toe om de verwachte output van HTML van clientlib te zien omvat gebaseerd op categorie. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**de bevestiging van de Afhankelijkheden van Bibliotheken** &#x200B;](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - benadrukt om het even welke gebiedsdelen of ingebedde categorieën die niet kunnen worden gevonden. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**herbouwt de Bibliotheken van de Cliënt** &#x200B;](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - staat een gebruiker toe om AEM te dwingen om de cliëntbibliotheken te herbouwen of het geheime voorgeheugen van cliëntbibliotheken ongeldig te maken. Dit gereedschap is effectief bij het ontwikkelen met LESS, omdat dit AEM kan dwingen de gegenereerde CSS opnieuw te compileren. Over het algemeen is het effectiever om de cache ongeldig te maken en vervolgens een pagina te vernieuwen in plaats van de bibliotheken opnieuw samen te stellen. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![&#x200B; herbouwt cliëntbibliotheek &#x200B;](assets/client-side-libraries/rebuild-clientlibs.png)
