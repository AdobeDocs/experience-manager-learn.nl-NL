---
title: Aan de slag met AEM Sites - Pagina's en sjablonen
description: Leer over de verhouding tussen een component van de basispagina en editable malplaatjes. Begrijp hoe de Componenten van de Kern in het project proxied zijn. Leer geavanceerde beleidsconfiguraties van editable malplaatjes om een goed-gestructureerd malplaatje van de artikelpagina te bouwen dat op een model van Adobe XD wordt gebaseerd.
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4082
thumbnail: 30214.jpg
doc-type: Tutorial
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
duration: 2049
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2853'
ht-degree: 0%

---

# Pagina&#39;s en sjablonen {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

In dit hoofdstuk, onderzoeken wij de verhouding tussen een component van de basispagina en editable malplaatjes. Leer een naamloze artikelsjabloon op basis van een aantal modellen op basis van [Adobe XD](https://helpx.adobe.com/support/xd.html). In het proces om het malplaatje uit te bouwen, zijn de Componenten van de Kern en de geavanceerde beleidsconfiguraties van de Bewerkbare Malplaatjes behandeld.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment).

### Starter-project

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide, kunt u het project opnieuw gebruiken en de stappen overslaan voor het uitchecken van het starterproject.

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Kijk uit de `tutorial/pages-templates-start` vertakking van [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) of controleer de code plaatselijk door aan de tak over te schakelen `tutorial/pages-templates-solution`.

## Doelstelling

1. Inspect a page design created in Adobe XD and map it to Core Components.
1. Begrijp de details van Bewerkbare Malplaatjes en hoe het beleid kan worden gebruikt om korrelige controle van paginainhoud af te dwingen.
1. Leer hoe sjablonen en pagina&#39;s zijn gekoppeld

## Wat u gaat bouwen {#what-build}

In dit gedeelte van de zelfstudie maakt u een nieuwe artikelpaginasjabloon die u kunt gebruiken om artikelpagina&#39;s te maken en deze uit te lijnen met een gemeenschappelijke structuur. Het sjabloon voor artikelpagina is gebaseerd op ontwerpen en een UI-kit die in Adobe XD is gemaakt. Dit hoofdstuk is alleen gericht op het opbouwen van de structuur of het skelet van de sjabloon. Er worden geen stijlen geïmplementeerd, maar de sjabloon en pagina&#39;s zijn functioneel.

![Artikelpaginaontwerp en niet-opgemaakte versie](assets/pages-templates/what-you-will-build.png)

## UI-planning met Adobe XD {#adobexd}

Gewoonlijk begint het plannen voor een nieuwe website met modellen en statische ontwerpen. [Adobe XD](https://helpx.adobe.com/support/xd.html) is een ontwerphulpmiddel dat gebruikerservaring bouwt. Vervolgens inspecteren we een UI-kit en -modellen om de structuur van het sjabloon voor artikelpagina te plannen.

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**Download de [WKND-artikelontwerpbestand](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> Een generiek [AEM Core Components UI Kit is ook beschikbaar](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) als uitgangspunt voor aangepaste projecten.

## De sjabloon voor artikelpagina maken

Wanneer u een pagina maakt, moet u een sjabloon selecteren die wordt gebruikt als basis voor het maken van de pagina. De sjabloon definieert de structuur van de resulterende pagina, de initiële inhoud en de toegestane componenten.

Er zijn drie hoofdgebieden [Bewerkbare sjablonen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Structuur** - definieert componenten die deel uitmaken van de sjabloon. Deze kunnen niet worden bewerkt door de auteurs van de inhoud.
1. **Oorspronkelijke inhoud** - definieert componenten waarmee de sjabloon begint. Deze kunnen door makers van inhoud worden bewerkt en/of verwijderd.
1. **Beleid** - definieert configuraties voor het gedrag van componenten en de opties die auteurs hebben.

Maak vervolgens een sjabloon in AEM die overeenkomt met de structuur van de modellen. Dit gebeurt in een lokale instantie van AEM. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

Stappen op hoog niveau voor de bovenstaande video:

### Structuurconfiguraties

1. Een sjabloon maken met de opdracht **Type paginasjabloon**, benoemd **Artikelpagina**.
1. Overschakelen op **Structuur** -modus.
1. Een **Ervaar fragment** als de **Koptekst** boven aan de sjabloon.
   * Vorm de component om te richten `/content/experience-fragments/wknd/us/en/site/header/master`.
   * Beleid instellen op **Paginakoptekst** en ervoor zorgen dat de **Standaardelement** is ingesteld op `header`. De `header`element is gericht met CSS in het volgende hoofdstuk.
1. Een **Ervaar fragment** als de **Voettekst** onder aan de sjabloon.
   * Vorm de component om te richten `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * Beleid instellen op **Paginavoettekst** en ervoor zorgen dat de **Standaardelement** is ingesteld op `footer`. De `footer` element is gericht met CSS in het volgende hoofdstuk.
1. De **hoofd** container die was inbegrepen toen het malplaatje aanvankelijk werd gecreeerd.
   * Beleid instellen op **Hoofdpagina van pagina** en ervoor zorgen dat de **Standaardelement** is ingesteld op `main`. De `main` element is gericht met CSS in het volgende hoofdstuk.
1. Een **Afbeelding** aan de component **hoofd** container.
   * Ontgrendel de **Afbeelding** component.
1. Voeg een **Broodkruimel** component onder de component **Afbeelding** in de hoofdcontainer.
   * Een beleid maken voor de **Broodkruimel** component benoemd **Artikel pagina - Broodkruimel**. Stel de **Beginniveau navigatie** tot **4**.
1. Voeg een **Container** component onder de component **Broodkruimel** en binnen de **hoofd** container. Dit fungeert als **Inhoud container** voor de sjabloon.
   * Ontgrendel de **Inhoud** container.
   * Beleid instellen op **Pagina-inhoud**.
1. Nog een toevoegen **Container** component onder de component **Inhoud container**. Dit fungeert als **Zijspoor** container voor de sjabloon.
   * Ontgrendel de **Zijspoor** container.
   * Een beleid maken met de naam **Artikel Pagina - zijspoor**.
   * Vorm **Toegestane componenten** krachtens **WKND-siteproject - Inhoud** op te nemen: **Knop**, **Downloaden**, **Afbeelding**, **Lijst**, **Scheidingsteken**, **Delen van sociale media**, **Tekst**, en **Titel**.
1. Werk het beleid van de container van de Wortel van de Pagina bij. Dit is de buitenste container op de sjabloon. Beleid instellen op **Basispagina**.
   * Onder **Containerinstellingen**, stelt u de **Layout** tot **Responsief raster**.
1. Modus Lay-out inschakelen voor de **Inhoud container**. Sleep de handgreep van rechts naar links en krimpt de container tot acht kolommen breed.
1. Modus Lay-out inschakelen voor de **Zijspoor, container**. Sleep de greep van rechts naar links en krimpt de container in tot vier kolommen breed. Sleep vervolgens de linkergreep van links naar rechts om de container 3 kolommen breed te maken en een tussenruimte van 1 kolom tussen de kolommen te laten staan **Inhoud container**.
1. Open de mobiele emulator en schakel over naar een mobiel onderbrekingspunt. Schakel de lay-outmodus opnieuw in en maak de **Inhoud container** en de **Zijspoor, container** de volledige breedte van de pagina. Hierdoor worden de containers verticaal in het mobiele onderbrekingspunt gestapeld.
1. Het beleid van de **Tekst** in de **Inhoud container**.
   * Beleid instellen op **Inhoudstekst**.
   * Onder **Plug-ins** > **Alineastijlen**, controle **Alineastijlen inschakelen** en ervoor zorgen dat de **Offerteblok** is ingeschakeld.

### Aanvankelijke inhoudsconfiguraties

1. Overschakelen op **Oorspronkelijke inhoud** -modus.
1. Voeg een **Titel** aan de component **Inhoud container**. Dit is de titel van artikel. Als de pagina leeg blijft, wordt automatisch de titel van de huidige pagina weergegeven.
1. Een seconde toevoegen **Titel** component onder de eerste component Title.
   * Configureer de component met de tekst &quot;Door auteur&quot;. Dit is een tijdelijke aanduiding voor tekst.
   * Te gebruiken tekst instellen `H4`.
1. Voeg een **Tekst** component onder de component **Op auteur** Component Titel.
1. Voeg een **Titel** aan de component **Zijspoorcontainer**.
   * Configureer de component met de tekst &quot;Dit artikel delen&quot;.
   * Te gebruiken tekst instellen `H5`.
1. Voeg een **Delen van sociale media** component onder de component **Dit artikel delen** Component Titel.
1. Voeg een **Scheidingsteken** component onder de component **Delen van sociale media** component.
1. Voeg een **Downloaden** component onder de component **Scheidingsteken** component.
1. Voeg een **Lijst** component onder de component **Downloaden** component.
1. Werk de **Oorspronkelijke pagina-eigenschappen** voor de sjabloon.
   * Onder **Sociale media** > **Delen van sociale media**, controle **Facebook** en **Pinterest**

### De sjabloon inschakelen en een miniatuur toevoegen

1. Bekijk het malplaatje in de console van het Malplaatje door te navigeren aan [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **Inschakelen** de sjabloon voor artikelpagina.
1. Bewerk de eigenschappen van de sjabloon Artikelpagina en upload de volgende miniatuur om snel pagina&#39;s te identificeren die zijn gemaakt met de sjabloon Artikelpagina:

   ![Miniatuur van artikelpaginasjabloon](assets/pages-templates/article-page-template-thumbnail.png)

## Koptekst en voettekst bijwerken met ervaringsfragmenten {#experience-fragments}

Bij het maken van algemene inhoud, zoals een kop- of voettekst, is het gebruikelijk om een [Ervaar fragment](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Met de functie Fragmenten van ervaring kunnen gebruikers meerdere componenten combineren om één component te maken die geschikt is voor referentie. De Fragmenten van de ervaring hebben het voordeel om multi-site beheer te steunen en [lokalisatie](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en).

Het AEM Projectarchetype produceerde een Kopbal en Voettekst. Werk vervolgens de Experience Fragments bij zodat deze overeenkomen met de modellen. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

Stappen op hoog niveau voor de bovenstaande video:

1. Download het pakket met voorbeeldinhoud **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. Upload en installeer het inhoudspakket met Package Manager op [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Werk het malplaatje van de Variatie van het Web bij, dat het malplaatje voor de Fragmenten van de Ervaring bij wordt gebruikt [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * Werk het beleid bij **Container** op de sjabloon.
   * Beleid instellen op **XF-basiskleur**.
   * Onder, **Toegestane componenten** Selecteer de componentgroep **WKND-siteproject - structuur** op te nemen **Taalnavigatie**, **Navigatie**, en **Snel zoeken** componenten.

### Fragment voor koptekstervaring bijwerken

1. Open het fragment van de Ervaring dat de Kopbal bij teruggeeft [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. De hoofdmap configureren **Container** van het fragment. Dit is de meest buitenste **Container**.
   * Stel de **Layout** tot **Responsief raster**
1. Voeg de **WKND Donker logo** als een afbeelding boven aan het dialoogvenster **Container**. Het logo is opgenomen in het pakket dat in een vorige stap is geïnstalleerd.
   * De lay-out van de **WKND Donker logo** te worden **twee** kolommen breed. Sleep de handgrepen van rechts naar links.
   * Het logo configureren met **Alternatieve tekst** van &quot;WKND-logo&quot;.
   * Het logo configureren om **Koppeling** tot `/content/wknd/us/en` de startpagina.
1. Vorm **Navigatie** die al op de pagina is geplaatst.
   * Stel de **Basisniveaus uitsluiten** tot **1**.
   * Stel de **Navigatiestructuurdiepte** tot **1**.
   * De lay-out van de **Navigatie** te gebruiken component **acht** kolommen breed. Sleep de handgrepen van rechts naar links.
1. Verwijder de **Taalnavigatie** component.
1. De lay-out van de **Zoeken** te gebruiken component **twee** kolommen breed. Sleep de handgrepen van rechts naar links. Alle componenten moeten nu horizontaal op één rij worden uitgelijnd.

### Fragment Voettekstervaring bijwerken

1. Open het fragment van de Ervaring dat de Voettekst bij teruggeeft [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. De hoofdmap configureren **Container** van het fragment. Dit is de meest buitenste **Container**.
   * Stel de **Layout** tot **Responsief raster**
1. Voeg de **WKND-lichtlogo** als een afbeelding boven aan het dialoogvenster **Container**. Het logo is opgenomen in het pakket dat in een vorige stap is geïnstalleerd.
   * De lay-out van de **WKND-lichtlogo** te worden **twee** kolommen breed. Sleep de handgrepen van rechts naar links.
   * Het logo configureren met **Alternatieve tekst** van &quot;WKND Logo Light&quot;.
   * Het logo configureren om **Koppeling** tot `/content/wknd/us/en` de startpagina.
1. Voeg een **Navigatie** onder het logo. Vorm **Navigatie** component:
   * Stel de **Basisniveaus uitsluiten** tot **1**.
   * Uitschakelen **Alle onderliggende pagina&#39;s verzamelen**.
   * Stel de **Navigatiestructuurdiepte** tot **1**.
   * De lay-out van de **Navigatie** te gebruiken component **acht** kolommen breed. Sleep de handgrepen van rechts naar links.

## Een artikelpagina maken

Maak vervolgens een pagina met de sjabloon Artikelpagina. Maak de inhoud van de pagina zodat deze overeenkomt met de sitemakken. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

Stappen op hoog niveau voor de bovenstaande video:

1. Ga naar de Sites-console op [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Een onderliggende pagina maken **WKND** > **VS** > **NL** > **Tijdschrift**.
   * Kies de optie **Artikelpagina** sjabloon.
   * Onder **Eigenschappen** instellen **Titel** naar &quot;Ultimate Guide to LA Skateparks&quot;
   * Stel de **Naam** tot &quot;guide-la-skateparks&quot;
1. Vervangen **Op auteur** Titel met de tekst &quot;Door Stacey Roswells&quot;.
1. Werk de **Tekst** om een alinea op te nemen om het artikel te vullen. U kunt het volgende tekstbestand als kopie gebruiken: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Nog een toevoegen **Tekst** component.
   * Werk de component bij om het aanhalingsteken op te nemen: &quot;Er is geen betere plaats om te delen dan Los Angeles.&quot;.
   * Bewerk de Rich Text Editor in de modus Volledig scherm en wijzig het bovenstaande citaat om de **Offerteblok** element.
1. Blijf de tekst van het artikel vullen om deze aan te passen aan de modellen.
1. Vorm **Downloaden** om een PDF-versie van het artikel te gebruiken.
   * Onder **Downloaden** > **Eigenschappen**, klik checkbox aan **De titel ophalen van de DAM-middelen**.
   * Stel de **Beschrijving** naar: &quot;Get the Full Story&quot;.
   * Stel de **Tekst van handeling** naar: &quot;Download PDF&quot;.
1. Vorm **Lijst** component.
   * Onder **Lijstinstellingen** > **Lijst samenstellen met**, selecteert u **Onderliggende pagina&#39;s**.
   * Stel de **Bovenliggende pagina** tot `/content/wknd/us/en/magazine`.
   * Onder, **Iteminstellingen** controleren **Items koppelen** en controle **Datum tonen**.

## Inspect de nodestructuur {#node-structure}

Op dit punt is de artikelpagina duidelijk ongeopend. De basisstructuur is echter aanwezig. Controleer vervolgens de knooppuntstructuur van de artikelpagina om meer inzicht te krijgen in de rol van de sjabloon, pagina en componenten.

Gebruik het hulpmiddel CRXDE-Lite op een lokale AEM instantie om de onderliggende knoopstructuur te bekijken.

1. Openen [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) en gebruik de boomnavigatie om te navigeren naar `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. Klik op de knop `jcr:content` knooppunt onder de `la-skateparks` pagina en bekijk de eigenschappen:

   ![Eigenschappen van JCR-inhoud](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Let op de waarde voor `cq:template`, die `/conf/wknd/settings/wcm/templates/article-page`, de sjabloon voor artikelpagina die u eerder hebt gemaakt.

   Let ook op de waarde van `sling:resourceType`, die `wknd/components/page`. Dit is de paginacomponent die door het AEM projectarchetype wordt gecreeerd en is verantwoordelijk voor het teruggeven van pagina die op het malplaatje wordt gebaseerd.

1. Breid uit `jcr:content` knooppunt onder `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` en bekijk de knooppunthiërarchie:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   U zou elk van de knopen aan componenten moeten kunnen losjes in kaart brengen die werden authored. Controleren of u de verschillende containers voor lay-out kunt identificeren die worden gebruikt door de knooppunten te inspecteren die vooraf zijn ingesteld `container`.

1. Controleer de pagina-component op `/apps/wknd/components/page`. De componenteigenschappen weergeven in CRXDE Lite:

   ![Eigenschappen van pagina-component](assets/pages-templates/page-component-properties.png)

   Er zijn slechts twee HTML-scripts. `customfooterlibs.html` en `customheaderlibs.html` onder de pagina-component. *Hoe geeft deze component de pagina weer?*

   De `sling:resourceSuperType` eigenschap verwijst naar `core/wcm/components/page/v2/page`. Met deze eigenschap kan de paginacomponent van de WKND overerven **alles** de functionaliteit van de pagina-component Core Component. Dit is het eerste voorbeeld van iets dat de [Proxycomponentpatroon](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Meer informatie is beschikbaar op [hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect een andere component binnen de WKND-componenten, de `Breadcrumb` component van: `/apps/wknd/components/breadcrumb`. Hetzelfde geldt `sling:resourceSuperType` eigenschap kan worden gevonden, maar deze keer verwijst deze naar `core/wcm/components/breadcrumb/v2/breadcrumb`. Dit is een ander voorbeeld van het gebruiken van het de componentenpatroon van de Volmacht om een Component van de Kern te omvatten. In feite, zijn alle componenten in de WKND codebasis volmachten van AEMComponenten van de Kern (behalve de component van douane demo HelloWorld). Het is aan te raden om zoveel mogelijk van de functionaliteit van kerncomponenten te hergebruiken *voor* aangepaste code schrijven.

1. Controleer de pagina Core Component op `/libs/core/wcm/components/page/v2/page` CRXDE Lite gebruiken:

   >[!NOTE]
   >
   > In AEM 6.5/6.4 bevinden de Core Components zich onder `/apps/core/wcm/components`. In, AEM as a Cloud Service, worden de Componenten van de Kern gevestigd onder `/libs` en worden automatisch bijgewerkt.

   ![Basiscomponentpagina](assets/pages-templates/core-page-component-properties.png)

   U ziet dat er veel scriptbestanden onder deze pagina staan. De pagina Core Component bevat een groot aantal functies. Deze functionaliteit is opgedeeld in meerdere scripts voor eenvoudiger onderhoud en leesbaarheid. U kunt de opname van de HTML-scripts traceren door het dialoogvenster `page.html` en op zoek naar `data-sly-include`:

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   De andere reden om HTML in veelvoudige manuscripten uit te breken is de volmachtscomponenten toe te staan om individuele manuscripten met voeten te treden om douanebedrijfslogica uit te voeren. De HTML-scripts `customfooterlibs.html`, en `customheaderlibs.html`, worden gecreëerd om expliciet te worden vervangen door de uitvoering van projecten.

   U kunt meer weten over de manier waarop de bewerkbare sjabloon invloed heeft op de rendering van de [inhoudspagina door dit artikel te lezen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect nog een Core Component, zoals de Breadcrumb op `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. De weergave `breadcrumb.html` om te begrijpen hoe de prijsverhoging voor de component Breadcrumb uiteindelijk wordt geproduceerd.

## Configuraties opslaan naar bronbeheer {#configuration-persistence}

Vaak, vooral aan het begin van een AEM project is het waardevol om configuraties, zoals malplaatjes en verwant inhoudsbeleid, aan broncontrole voort te zetten. Dit zorgt ervoor dat alle ontwikkelaars tegen de zelfde reeks inhoud en configuraties werken en extra consistentie tussen milieu&#39;s kunnen verzekeren. Wanneer een project een bepaald ontwikkelingsniveau heeft bereikt, kan het beheren van sjablonen worden overgedragen aan een speciale groep van energiegebruikers.


Sjablonen worden momenteel op dezelfde manier verwerkt als andere codefragmenten en de **Artikelpaginasjabloon** als onderdeel van het project.
Tot nu toe wordt de code geduwd van het AEM project aan een lokaal geval van AEM. De **Artikelpaginasjabloon** is rechtstreeks gemaakt op een lokale AEM, dus moet u **import** het malplaatje in het AEM project. De **ui.content** is opgenomen in het AEM project voor dit specifieke doel.

De volgende paar stappen worden gedaan in winde VSCode gebruikend [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) insteekmodule. Maar zij konden doen gebruikend om het even welke winde die u aan hebt gevormd **import** of importeer inhoud uit een lokale instantie van AEM.

1. In, opent VSCode `aem-guides-wknd` project.

1. Breid uit **ui.content** in de explorator van het Project. Breid uit `src` en navigeer naar `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Right+Click] de `templates` map en selecteer **Importeren vanaf AEM server**:

   ![VSCode-importsjabloon](assets/pages-templates/vscode-import-templates.png)

   De `article-page` moeten worden ingevoerd en `page-content`, `xf-web-variation` sjablonen moeten ook worden bijgewerkt.

   ![Bijgewerkte sjablonen](assets/pages-templates/updated-templates.png)

1. Herhaal de stappen om inhoud te importeren, maar selecteer de **beleid** map van `/conf/wknd/settings/wcm/policies`.

   ![Beleid voor het importeren van VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect the `filter.xml` bestand van `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   De `filter.xml` is verantwoordelijk voor het identificeren van de paden van knooppunten die samen met het pakket zijn geïnstalleerd. Let op: `mode="merge"` op elk van de filters die aangeeft dat bestaande inhoud niet moet worden gewijzigd, wordt alleen nieuwe inhoud toegevoegd. Aangezien de inhoudsauteurs deze wegen kunnen bijwerken, is het belangrijk dat een codeplaatsing doet **niet** overschrijven, inhoud. Zie de [FileVault-documentatie](https://jackrabbit.apache.org/filevault/filter.html) voor meer informatie over het werken met filterelementen.

   Ververgelijken `ui.content/src/main/content/META-INF/vault/filter.xml` en `ui.apps/src/main/content/META-INF/vault/filter.xml` om de verschillende knopen te begrijpen die door elke module worden beheerd.

   >[!WARNING]
   >
   > Om verenigbare plaatsingen voor de plaats van de Verwijzing te verzekeren WKND worden sommige takken van het project opgezet dusdanig dat `ui.content` Hiermee overschrijft u eventuele wijzigingen in het JCR. Dit is door ontwerp, d.w.z. voor de Tak van de Oplossing, aangezien de code/de stijlen voor specifiek beleid worden geschreven.

## Gefeliciteerd! {#congratulations}

U hebt een sjabloon en pagina met Adobe Experience Manager Sites gemaakt.

### Volgende stappen {#next-steps}

Op dit punt is de artikelpagina duidelijk ongeopend. Volg de [Client-Side Libraries en front-end workflow](client-side-libraries.md) zelfstudie voor het leren van de beste werkwijzen voor het opnemen van CSS en JavaScript om algemene stijlen toe te passen op de site en een speciale front-end build te integreren.

De voltooide code weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van de it op `tutorial/pages-templates-solution`.

1. Klonen met [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) opslagplaats.
1. Kijk uit de `tutorial/pages-templates-solution` vertakking.
