---
title: Aan de slag met AEM Sites - Pagina's en sjablonen
seo-title: Aan de slag met AEM Sites - Pagina's en sjablonen
description: Leer over de verhouding tussen een component van de basispagina en editable malplaatjes. Begrijp hoe de Componenten van de Kern in het project proxied zijn en geavanceerde beleidsconfiguraties van editable malplaatjes leren om een goed-gestructureerd malplaatje van de Pagina van het Artikel te bouwen dat op een model van Adobe XD wordt gebaseerd.
sub-product: sites
feature: template-editor, core-components
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
translation-type: tm+mt
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '3072'
ht-degree: 0%

---


# Pagina&#39;s en sjablonen {#pages-and-template}

In dit hoofdstuk onderzoeken we de relatie tussen een component van de basispagina en bewerkbare sjablonen. We maken een niet-opgemaakte artikelsjabloon op basis van een aantal modellen van [AdobeXD](https://www.adobe.com/products/xd.html). In het proces om het malplaatje uit te bouwen, zijn de Componenten van de Kern en de geavanceerde beleidsconfiguraties van de Bewerkbare Malplaatjes behandeld.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

### Starter-project

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide kunt u het project hergebruiken en de stappen overslaan voor het uitchecken van het starterproject.

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Ontdek de `tutorial/pages-templates-start`-vertakking van [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > Als u AEM 6.5 of 6.4 gebruikt, voegt u het `classic`-profiel toe aan alle Maven-opdrachten.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution) altijd bekijken of de code plaatselijk controleren door aan de tak `tutorial/pages-templates-solution` te schakelen.

## Doelstelling

1. Inspect a page design created in Adobe XD and map it to Core Components.
1. Begrijp de details van Bewerkbare Malplaatjes en hoe het beleid kan worden gebruikt om korrelige controle van paginainhoud af te dwingen.
1. Leer hoe sjablonen en pagina&#39;s zijn gekoppeld

## Wat u {#what-you-will-build} wilt maken

In dit gedeelte van de zelfstudie maakt u een nieuwe artikelpaginasjabloon die u kunt gebruiken om nieuwe artikelpagina&#39;s te maken en deze uit te lijnen met een gemeenschappelijke structuur. Het sjabloon voor artikelpagina wordt gebaseerd op ontwerpen en een UI-kit die in AdobeXD wordt gemaakt. Dit hoofdstuk is alleen gericht op het opbouwen van de structuur of het skelet van de sjabloon. Er worden geen stijlen geïmplementeerd, maar de sjabloon en pagina&#39;s zijn functioneel.

![Artikelpaginaontwerp en niet-opgemaakte versie](assets/pages-templates/what-you-will-build.png)

## UI-planning met Adobe XD {#adobexd}

In de meeste gevallen begint de planning voor een nieuwe website met modellen en statische ontwerpen. [Adobe ](https://www.adobe.com/products/xd.html) XD is een ontwerpgereedschap waarmee gebruikers een ervaring kunnen opbouwen. Vervolgens inspecteren we een UI-kit en -modellen om de structuur van het sjabloon voor artikelpagina te helpen plannen.

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**Download het  [WKND-artikelontwerpbestand](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

## De sjabloon voor artikelpagina maken

Wanneer u een pagina maakt, moet u een sjabloon selecteren die wordt gebruikt als basis voor het maken van de nieuwe pagina. De sjabloon definieert de structuur van de resulterende pagina, de initiële inhoud en de toegestane componenten.

Er zijn drie hoofdgebieden van [Bewerkbare sjablonen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html):

1. **Structuur**  - definieert componenten die deel uitmaken van de sjabloon. Deze kunnen niet worden bewerkt door auteurs van inhoud.
1. **Initiële inhoud**  - definieert componenten waarmee de sjabloon begint. Deze kunnen worden bewerkt en/of verwijderd door makers van inhoud
1. **Het beleid**  - bepaalt configuraties op hoe de componenten zich zullen gedragen en welke optiesauteurs beschikbaar zullen hebben.

Maak vervolgens een nieuwe sjabloon in AEM die overeenkomt met de structuur van de modellen. Dit zal in een lokale instantie van AEM voorkomen. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

Stappen op hoog niveau voor de onderstaande video:

### Structuurconfiguraties

1. Maak een nieuwe sjabloon met de naam **Paginasjabloontype**, **Artikelpagina**.
1. Schakel over naar de modus **Structuur**.
1. Voeg een **Ervaar Fragment** component toe om als **Kopbal** bij de bovenkant van het malplaatje te dienst te doen.
   * Vorm de component om aan `/content/experience-fragments/wknd/us/en/site/header/master` te richten.
   * Stel het beleid in op **Paginakop** en zorg ervoor dat **Standaardelement** is ingesteld op `header`. Het `header`element zal met CSS in het volgende hoofdstuk worden gericht.
1. Voeg een **Ervaar Fragment** component toe om als **Voettekst** bij de bodem van het malplaatje te dienst te doen.
   * Vorm de component om aan `/content/experience-fragments/wknd/us/en/site/footer/master` te richten.
   * Stel het beleid in op **Paginavoettekst** en zorg ervoor dat **Standaardelement** is ingesteld op `footer`. Het `footer` element zal met CSS in het volgende hoofdstuk worden gericht.
1. Vergrendel de **main** container die was opgenomen toen de sjabloon voor het eerst werd gemaakt.
   * Stel het beleid in op **Page Main** en zorg ervoor dat **Default Element** is ingesteld op `main`. Het `main` element zal met CSS in het volgende hoofdstuk worden gericht.
1. Voeg een **Image** component aan **main** container toe.
   * Ontgrendel de **component Image**.
1. Voeg een **Breadcrumb**-component onder de **Image**-component in de hoofdcontainer toe.
   * Maak een nieuw beleid voor de **Breadcrumb**-component met de naam **Article Page - Breadcrumb**. Stel **Navigatiebeginniveau** in op **4**.
1. Voeg een **Container** component onder de **Breadcrumb** component en binnen de **main** container toe. Dit zal als **Inhoudscontainer** voor het malplaatje dienst doen.
   * Ontgrendel de container **Content**.
   * Stel het beleid in op **Pagina-inhoud**.
1. Voeg een andere **Container** component onder **Inhoudscontainer** toe. Dit zal als **Zijspoor** container voor het malplaatje dienst doen.
   * Ontgrendel de **Zijspoor** container.
   * Maak een nieuw beleid met de naam **Article Page - Side Rail**.
   * Configureer **Toegestane componenten** onder **WKND-siteproject - Inhoud** om op te nemen: **Button**, **Download**, **Image**, **List**, **Scheidingsteken**, **Sociale media delen**, &lt;a1 6/>Tekst **en** Titel **.**
1. Werk het beleid van de container van de Wortel van de Pagina bij. Dit is de buitenste container op de sjabloon. Stel het beleid in op **Basispagina**.
   * Stel onder **Containerinstellingen** de **Layout** in op **Responsief raster**.
1. Modus Lay-out inschakelen voor de **Inhoudscontainer**. Sleep de greep van rechts naar links en maak de container kleiner tot 8 kolommen breed.
1. Modus Lay-out inschakelen voor de **container voor zijspoor**. Sleep de greep van rechts naar links en krimpt de container in tot 4 kolommen breed. Vervolgens sleept u de linkergreep van links naar rechts 1 kolom om de container 3 kolommen breed te maken en een gat van 1 kolom tussen de **Inhoudscontainer** te laten.
1. Open de mobiele emulator en schakel over naar een mobiel onderbrekingspunt. Schakel de lay-outmodus opnieuw in en maak de **Content container** en de **Side Rail container** de volledige breedte van de pagina. Hierdoor worden de containers verticaal in het mobiele breekpunt gestapeld.
1. Werk het beleid van **Text** component in **Inhoudscontainer** bij.
   * Stel het beleid in op **Inhoudstekst**.
   * Selecteer **Insteekmodules** > **Alineastijlen**, schakel **Alineastijlen inschakelen** in en controleer of **Offerteblok** is ingeschakeld.

### Aanvankelijke inhoudsconfiguraties

1. Schakel over naar de modus **Eerste inhoud**.
1. Voeg een **Title** component aan **Content container** toe. Dit is de titel van artikel. Als de pagina leeg blijft, wordt automatisch de titel van de huidige pagina weergegeven.
1. Voeg een tweede **Title** component onder de eerste component van de Titel toe.
   * Configureer de component met de tekst: &quot;Door auteur&quot;. Dit wordt een tijdelijke aanduiding voor tekst.
   * Stel het type in op `H4`.
1. Voeg een **Text** component onder **Door Auteur** component van de Titel toe.
1. Voeg een **Title**-component toe aan de **Side Rail Container**.
   * Configureer de component met de tekst: &quot;Dit artikel delen&quot;.
   * Stel het type in op `H5`.
1. Voeg een **Delen van sociale media**-component onder de **Deel dit artikel** Titelcomponent.
1. Voeg een **Scheidingsteken** component onder **Sociale media die** component delen toe.
1. Voeg een **Download** component onder **Scheidingsteken** component toe.
1. Voeg een **component List** onder de **component Download** toe.
1. Werk **Initiële pagina-eigenschappen** voor de sjabloon bij.
   * Onder **Sociale media** > **Sociale media delen**, controleer **Facebook** en **Pinterest**

### De sjabloon inschakelen en een miniatuur toevoegen

1. Bekijk het malplaatje in de console van het Malplaatje door aan [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd) te navigeren
1. **De sjabloon voor de artikelpagina** inschakelen.
1. Bewerk de eigenschappen van de sjabloon Artikelpagina en upload de volgende miniatuur om snel pagina&#39;s te identificeren die zijn gemaakt met de sjabloon Artikelpagina:

   ![Miniatuur van artikelpaginasjabloon](assets/pages-templates/article-page-template-thumbnail.png)

## Koptekst en voettekst bijwerken met ervaringsfragmenten {#experience-fragments}

Bij het maken van algemene inhoud, zoals een kop- of voettekst, wordt vaak gebruikgemaakt van een [Experience Fragment](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). Met de functie Fragmenten van ervaring kunnen gebruikers meerdere componenten combineren om één component te maken die geschikt is voor referentie. De Fragmenten van de ervaring hebben het voordeel om multi-site beheer en [localization](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure) te steunen.

Het AEM Projectarchetype produceerde een Kopbal en Voettekst. Werk vervolgens de Experience Fragments bij zodat deze overeenkomen met de modellen. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

Stappen op hoog niveau voor de onderstaande video:

1. Download het pakket met voorbeeldinhoud **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**.
1. Upload en installeer het inhoudspakket met Package Manager op [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Werk het malplaatje van de Variatie van het Web bij, dat het malplaatje voor de Fragmenten van de Ervaring in [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html) wordt gebruikt
   * Werk het beleid de **Container** component op het malplaatje bij.
   * Stel het beleid in op **XF Root**.
   * Selecteer onder **Toegestane componenten** de componentengroep **WKND-siteproject - Structuur** om **Taalnavigatie**, **Navigatie** en **Snel zoeken**-componenten op te nemen.

### Fragment voor koptekstervaring bijwerken

1. Open het fragment van de Ervaring dat de Kopbal op [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html) teruggeeft
1. Configureer de hoofdmap **Container** van het fragment. Dit is de buitenste meest **Container**.
   * **Layout** instellen op **Responsief raster**
1. Voeg het **Donker WKND-logo** toe als een afbeelding boven aan de **Container**. Het logo is opgenomen in het pakket dat in een vorige stap is geïnstalleerd.
   * Wijzig de lay-out van **Donker Logo WKND** om **2** kolommen breed te zijn. Sleep de handgrepen van rechts naar links.
   * Configureer het logo met **Alternatieve tekst** van &quot;WKND-logo&quot;.
   * Vorm het embleem aan **Verbinding** aan `/content/wknd/us/en` de Homepage.
1. Configureer de **Navigatie**-component die al op de pagina is geplaatst.
   * Stel **Hoofdniveaus uitsluiten** in op **1**.
   * Stel de **Navigatiestructuurdiepte** in op **1**.
   * Wijzig de lay-out van de **Navigation** component om **8** kolommen breed te zijn. Sleep de handgrepen van rechts naar links.
1. Verwijder de **Taalnavigatie** component.
1. Wijzig de lay-out van de **Search** component om **2** kolommen breed te zijn. Sleep de handgrepen van rechts naar links. Alle componenten moeten nu horizontaal op één rij worden uitgelijnd.

### Fragment Voettekstervaring bijwerken

1. Open het fragment Experience dat de voettekst weergeeft op [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. Configureer de hoofdmap **Container** van het fragment. Dit is de buitenste meest **Container**.
   * **Layout** instellen op **Responsief raster**
1. Voeg het **WKND-lichtlogo** toe als een afbeelding boven aan de **Container**. Het logo is opgenomen in het pakket dat in een vorige stap is geïnstalleerd.
   * Wijzig de lay-out van het **WKND Licht Logo** om **2** kolommen breed te zijn. Sleep de handgrepen van rechts naar links.
   * Configureer het logo met **Alternatieve tekst** van &quot;WKND-logolicht&quot;.
   * Vorm het embleem aan **Verbinding** aan `/content/wknd/us/en` de Homepage.
1. Voeg een **Navigatie** component onder het embleem toe. Configureer de **Navigation** component:
   * Stel **Hoofdniveaus uitsluiten** in op **1**.
   * Schakel **Alle onderliggende pagina&#39;s verzamelen** uit.
   * Stel de **Navigatiestructuurdiepte** in op **1**.
   * Wijzig de lay-out van de **Navigation** component om **8** kolommen breed te zijn. Sleep de handgrepen van rechts naar links.

## Een artikelpagina maken

Maak vervolgens een nieuwe pagina met de sjabloon Artikelpagina. Maak de inhoud van de pagina zodat deze overeenkomt met de sitemakken. Voer de stappen in de onderstaande video uit:

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

Stappen op hoog niveau voor de onderstaande video:

1. Navigeer naar de Sites-console op [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. Maak een nieuwe pagina onder **WKND** > **US** > **EN** > **Magazine**.
   * Kies de sjabloon **Artikel pagina**.
   * Onder **Eigenschappen** stelt **Titel** in op &quot;Ultimate Guide to LA Skateparks&quot;
   * **Naam** instellen op &quot;guide-la-skateparks&quot;
1. Vervang **Door auteur** Titel door de tekst &quot;Door Stacey Roswells&quot;.
1. Werk de **component Text** bij om een alinea op te nemen om het artikel te vullen. U kunt het volgende tekstbestand als kopie gebruiken: [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. Voeg een andere **component Text** toe.
   * Werk de component bij om het citaat te omvatten: &quot;Er is geen betere plek om te delen dan Los Angeles.&quot;
   * Bewerk de Rich Text Editor in de modus Volledig scherm en wijzig het bovenstaande citaat om het element **Citaatblok** te gebruiken.
1. Blijf de tekst van het artikel vullen om deze aan te passen aan de modellen.
1. Configureer de component **Download** om een PDF-versie van het artikel te gebruiken.
   * Klik onder **Download** > **Eigenschappen** op het selectievakje om de titel op te halen uit het DAM-element **.**
   * Stel de **Beschrijving** in op: &quot;Get the full Story&quot;.
   * Stel de **Action Text** in op: &quot;PDF downloaden&quot;.
1. Configureer de component **List**.
   * Onder **Lijstinstellingen** > **Lijst samenstellen met** selecteert u **Onderliggende pagina&#39;s**.
   * Stel de **Bovenliggende pagina** in op `/content/wknd/us/en/magazine`.
   * Onder **Iteminstellingen** schakelt u **Items koppelen** in en schakelt u **Datum tonen** in.

## Inspect the node structure {#node-structure}

Op dit punt is de artikelpagina duidelijk niet-opgemaakt. De basisstructuur is echter aanwezig. Controleer vervolgens de knooppuntstructuur van de artikelpagina om meer inzicht te krijgen in de rol van de sjabloon, pagina en componenten.

Gebruik het hulpmiddel CRXDE-Lite op een lokale AEM instantie om de onderliggende knoopstructuur te bekijken.

1. Open [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) en gebruik de boomnavigatie om aan `/content/wknd/us/en/magazine/guide-la-skateparks` te navigeren.

1. Klik op het knooppunt `jcr:content` onder de pagina `la-skateparks` en bekijk de eigenschappen:

   ![Eigenschappen van JCR-inhoud](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   Let op de waarde voor `cq:template`, die verwijst naar `/conf/wknd/settings/wcm/templates/article-page`, de sjabloon voor artikelpagina die we eerder hebben gemaakt.

   Let ook op de waarde van `sling:resourceType`, die naar `wknd/components/page` wijst. Dit is de paginacomponent die door het AEM projectarchetype wordt gecreeerd en is verantwoordelijk voor het teruggeven van pagina die op het malplaatje wordt gebaseerd.

1. Breid `jcr:content` knoop onder `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` uit en bekijk de knoophiërarchie:

   ![JCR Content LA Skateparks](assets/pages-templates/page-jcr-structure.png)

   U zou elk van de knopen aan componenten moeten kunnen losjes in kaart brengen die werden authored. Controleer of u de verschillende containers voor lay-out kunt identificeren die worden gebruikt door de knooppunten te inspecteren die vooraf met `container` zijn ingesteld.

1. Controleer vervolgens de paginacomponent op `/apps/wknd/components/page`. De componenteigenschappen weergeven in CRXDE Lite:

   ![Eigenschappen van pagina-component](assets/pages-templates/page-component-properties.png)

   Er staan slechts 2 HTML-scripts, `customfooterlibs.html` en `customheaderlibs.html` onder de paginacomponent. *Hoe geeft deze component de pagina weer?*

   De eigenschap `sling:resourceSuperType` verwijst naar `core/wcm/components/page/v2/page`. This property allows the WKND&#39;s page component to inherit **all** of the functionaliteit of the Core Component page component. Dit is het eerste voorbeeld van iets genoemd het [Patroon van de Component van de Volmacht](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). Meer informatie vindt u [hier.](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect een andere component binnen de componenten WKND, de `Breadcrumb` component die bij wordt gevestigd: `/apps/wknd/components/breadcrumb`. Merk op dat het zelfde `sling:resourceSuperType` bezit kan worden gevonden, maar dit keer richt het aan `core/wcm/components/breadcrumb/v2/breadcrumb`. Dit is een ander voorbeeld van het gebruiken van het de componentenpatroon van de Volmacht om een Component van de Kern te omvatten. In feite, zijn alle componenten in de WKND codebasis volmachten van AEM Componenten van de Kern (behalve onze beroemde component HelloWorld). Het is aan te raden te proberen zoveel mogelijk van de functionaliteit van Core Components te hergebruiken *voordat* aangepaste code schrijft.

1. Controleer vervolgens de pagina Core Component op `/libs/core/wcm/components/page/v2/page` met behulp van CRXDE Lite:

   >[!NOTE]
   >
   > In AEM 6.5/6.4 bevinden de Core Components zich onder `/apps/core/wcm/components`. In AEM als Cloud Service, worden de Componenten van de Kern gevestigd onder `/libs` en automatisch bijgewerkt.

   ![Basiscomponentpagina](assets/pages-templates/core-page-component-properties.png)

   U ziet dat er nog veel meer scripts onder deze pagina staan. De pagina Core Component bevat veel functionaliteit. Deze functionaliteit is opgedeeld in meerdere scripts voor eenvoudiger onderhoud en leesbaarheid. U kunt de opname van de HTML-scripts traceren door `page.html` te openen en `data-sly-include` te zoeken:

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

   De andere reden om HTML in veelvoudige manuscripten uit te breken is de volmachtscomponenten toe te staan om individuele manuscripten met voeten te treden om douanebedrijfslogica uit te voeren. De manuscripten van HTML, `customfooterlibs.html` en `customheaderlibs.html`, worden gecreeerd voor het expliciete doel dat moet worden met voeten getreden door projecten uit te voeren.

   U kunt meer over leren hoe het Bewerkbare Malplaatje in het teruggeven van de [inhoudspagina door dit artikel te lezen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html) beïnvloedt.

1. Inspect de andere Core Component, zoals de Breadcrumb op `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. Bekijk het `breadcrumb.html` manuscript om te begrijpen hoe de prijsverhoging voor de component Breadcrumb uiteindelijk wordt geproduceerd.

## Configuraties opslaan naar bronbesturing {#configuration-persistence}

In veel gevallen, vooral aan het begin van een AEM project is het waardevol om configuraties, zoals malplaatjes en verwant inhoudsbeleid, aan broncontrole voort te zetten. Dit zorgt ervoor dat alle ontwikkelaars tegen de zelfde reeks inhoud en configuraties werken en extra consistentie tussen milieu&#39;s kunnen verzekeren. Wanneer een project een bepaald ontwikkelingsniveau heeft bereikt, kan het beheren van sjablonen worden overgedragen aan een speciale groep van energiegebruikers.

Momenteel behandelen wij de malplaatjes als andere stukken van code en synchroniseren **Sjabloon van de Pagina van het Artikel** neer als deel van het project. Tot nu toe hebben wij **geduwde** code van ons AEM project aan een lokale instantie van AEM. **Artikelpaginasjabloon** is rechtstreeks gemaakt op een lokaal AEM. Daarom moeten we de sjabloon **importeren** in ons AEM project plaatsen. De **ui.content** module is inbegrepen in het AEM project voor dit specifieke doel.

De volgende paar stappen zullen plaatsvinden gebruikend winde VSCode gebruikend [VSCode AEM de stop van Synchronisatie](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) maar zouden kunnen doen gebruikend om het even welke winde die u aan **import** hebt gevormd of inhoud van een lokale instantie van AEM invoeren.

1. In VSCode open het `aem-guides-wknd` project.

1. Breid **ui.content** module in de ontdekkingsreiziger van het Project uit. Vouw de map `src` uit en navigeer naar `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL Right+Click] de  `templates` map en selecteer  **Importeren van AEM Server**:

   ![VSCode-importsjabloon](assets/pages-templates/vscode-import-templates.png)

   De `article-page` moet worden geïmporteerd en de `page-content`-, `xf-web-variation`-sjablonen moeten ook worden bijgewerkt.

   ![Bijgewerkte sjablonen](assets/pages-templates/updated-templates.png)

1. Herhaal de stappen voor het importeren van inhoud, maar selecteer de map **policies** op `/conf/wknd/settings/wcm/policies`.

   ![Beleid voor het importeren van VSCode](assets/pages-templates/policies-article-page-template.png)

1. Inspect het `filter.xml`-bestand op `ui.content/src/main/content/META-INF/vault/filter.xml`.

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

   Het `filter.xml`-bestand identificeert de paden van knooppunten die samen met het pakket worden geïnstalleerd. Let op `mode="merge"` op elk van de filters die aangeeft dat bestaande inhoud niet wordt gewijzigd, alleen nieuwe inhoud wordt toegevoegd. Aangezien de inhoudsauteurs deze wegen kunnen bijwerken, is het belangrijk dat een codeplaatsing inhoud **niet** beschrijft. Raadpleeg de [FileVault-documentatie](https://jackrabbit.apache.org/filevault/filter.html) voor meer informatie over het werken met filterelementen.

   Vergelijk `ui.content/src/main/content/META-INF/vault/filter.xml` en `ui.apps/src/main/content/META-INF/vault/filter.xml` om de verschillende knopen te begrijpen die door elke module worden beheerd.

   >[!WARNING]
   >
   > Om consistente plaatsingen voor de plaats van de Verwijzing te verzekeren WKND zijn sommige takken van het project opstelling dusdanig dat `ui.content` om het even welke veranderingen in JCR zal beschrijven. Dit is door ontwerp, d.w.z. voor de Tak van de Oplossing, aangezien de code/de stijlen voor specifiek beleid zullen worden geschreven.

## Gefeliciteerd! {#congratulations}

Je hebt zojuist een nieuwe sjabloon en pagina met Adobe Experience Manager Sites gemaakt.

### Volgende stappen {#next-steps}

Op dit punt is de artikelpagina duidelijk niet-opgemaakt. Volg de zelfstudie [Client-Side Libraries en Front-end Workflow](client-side-libraries.md) om de beste werkwijzen te leren voor het opnemen van CSS en Javascript om globale stijlen op de site toe te passen en een toegewijde front-end build te integreren.

Bekijk de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de schakelaar van de Git `tutorial/pages-templates-solution` op.

1. Clone the [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repository.
1. Bekijk de `tutorial/pages-templates-solution` vertakking.
