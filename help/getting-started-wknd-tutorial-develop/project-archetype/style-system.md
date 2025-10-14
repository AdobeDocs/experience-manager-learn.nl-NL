---
title: Ontwikkelen met het Stijlsysteem
description: Leer hoe u afzonderlijke stijlen implementeert en Core Components opnieuw gebruikt met Experience Manager Style System. Deze zelfstudie behandelt het ontwikkelen voor het Systeem van de Stijl om de Componenten van de Kern met merkspecifieke CSS en geavanceerde beleidsconfiguraties van de Redacteur van het Malplaatje uit te breiden.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4128
mini-toc-levels: 1
thumbnail: 30386.jpg
doc-type: Tutorial
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
duration: 358
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1555'
ht-degree: 0%

---

# Ontwikkelen met het Stijlsysteem {#developing-with-the-style-system}

Leer hoe u afzonderlijke stijlen implementeert en Core Components opnieuw gebruikt met Experience Manager Style System. Deze zelfstudie behandelt het ontwikkelen voor het Systeem van de Stijl om de Componenten van de Kern met merkspecifieke CSS en geavanceerde beleidsconfiguraties van de Redacteur van het Malplaatje uit te breiden.

## Vereisten {#prerequisites}

Herzie het vereiste tooling en de instructies voor vestiging a [&#x200B; lokale ontwikkelomgeving &#x200B;](overview.md#local-dev-environment).

Het wordt ook geadviseerd om de [&#x200B; cliënt-zijBibliotheken en het 1&rbrace; leerprogramma van het Voorste-Eind van het Werkschema te herzien &lbrace;om de grondbeginselen van cliënt-zijbibliotheken en de diverse front-end hulpmiddelen te begrijpen die in het project van AEM worden gebouwd.](client-side-libraries.md)

### Starter-project

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide, kunt u het project opnieuw gebruiken en de stappen overslaan voor het uitchecken van het starterproject.

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Controle uit de `tutorial/style-system-start` tak van [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

U kunt de gebeëindigde code op [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) altijd bekijken of de code plaatselijk controleren door aan de tak `tutorial/style-system-solution` te schakelen.

## Doelstelling

1. Begrijp hoe u het Stijlsysteem kunt gebruiken om merkspecifieke CSS toe te passen op AEM Core Components.
1. Leer meer over BEM-notatie en hoe u deze kunt gebruiken om stijlen zorgvuldig in bereik te brengen.
1. Geavanceerde beleidsconfiguraties toepassen met bewerkbare sjablonen.

## Wat u gaat bouwen {#what-build}

Dit hoofdstuk gebruikt de [&#x200B; eigenschap van het Systeem van de Stijl &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=nl-NL) om variaties van de **Titel** tot stand te brengen en **Tekst** componenten die op de pagina van het Artikel worden gebruikt.

![&#x200B; Stijlen beschikbaar voor Titel &#x200B;](assets/style-system/styles-added-title.png)

*onderstreepte stijl beschikbaar aan gebruik voor de Component van de Titel*

## Achtergrond {#background}

Het [&#x200B; Systeem van de Stijl &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=nl-NL) staat ontwikkelaars en malplaatjeredacteurs toe om veelvoudige visuele variaties van een component tot stand te brengen. Auteurs kunnen vervolgens bepalen welke stijl moet worden gebruikt bij het samenstellen van een pagina. Het Stijlsysteem wordt gebruikt door de rest van de zelfstudie om verscheidene unieke stijlen te bereiken terwijl het gebruiken van de Componenten van de Kern in een lage codebenadering.

Het algemene idee met het Stijlsysteem is dat ontwerpers verschillende stijlen kunnen kiezen van hoe een component eruit moet zien. De &quot;stijlen&quot; worden ondersteund door extra CSS-klassen die in de buitenste div van een component worden geïnjecteerd. In de clientbibliotheken worden CSS-regels toegevoegd op basis van deze stijlklassen, zodat de vormgeving van de component verandert.

U kunt [&#x200B; gedetailleerde documentatie voor het Systeem van de Stijl hier &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=nl-NL) vinden. Er is ook een grote [&#x200B; technische video voor het begrip van het Systeem van de Stijl &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html?lang=nl-NL).

## Onderstrepingsstijl - Titel {#underline-style}

De [&#x200B; Component van de Titel &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html?lang=nl-NL) is proxied in het project onder `/apps/wknd/components/title` als deel van de **ui.apps** module. De standaardstijlen van de elementen van de Kop (`H1`, `H2`, `H3`..) zijn reeds uitgevoerd in **ui.frontend** module.

De [&#x200B; ontwerpen van het Artikel van WKND &#x200B;](assets/pages-templates/wknd-article-design.xd) bevatten een unieke stijl voor de component van de Titel met onderstrepen. In plaats van twee componenten te maken of het dialoogvenster van de component te wijzigen, kunt u het Stijlsysteem gebruiken om auteurs de optie toe te staan een onderstrepingsstijl toe te voegen.

![&#x200B; onderstreepte Stijl - de Component van de Titel &#x200B;](assets/style-system/title-underline-style.png)

### Titelbeleid toevoegen

Voeg een beleid voor de componenten van de Titel toe om inhoudsauteurs toe te staan om de onderstreepte stijl te kiezen om op specifieke componenten toe te passen. Dit doet u met de Sjablooneditor in AEM.

1. Navigeer aan het **malplaatje van de Pagina van het Artikel** van: [&#x200B; http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html &#x200B;](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. In **wijze van de Structuur 0&rbrace;, op de belangrijkste** Container van de Lay-out **, selecteer het** pictogram van het Beleid **naast de** Titel **component die onder *wordt vermeld Toegelaten Componenten*:**

   ![&#x200B; het Beleid van de Titel vormt &#x200B;](assets/style-system/article-template-title-policy-icon.png)

1. Maak een beleid voor de component Title met de volgende waarden:

   *Titel van het Beleid&#42;*: **Titel WKND**

   *Eigenschappen* > *het Lusje van Stijlen* > *voegt een nieuwe stijl* toe

   **onderstreping** : `cmp-title--underline`

   ![&#x200B; het beleidsconfiguratie van de Stijl voor titel &#x200B;](assets/style-system/title-style-policy.png)

   Klik **Gedaan** om de veranderingen in het beleid van de Titel te bewaren.

   >[!NOTE]
   >
   > Met de waarde `cmp-title--underline` wordt de CSS-klasse gevuld op de buitenste div van de HTML-opmaak van de component.

### De stijl Onderstrepen toepassen

Laten we als auteur de onderstrepingsstijl toepassen op bepaalde titelcomponenten.

1. Navigeer aan **La Skateparks** artikel in de redacteur van AEM Sites bij: [&#x200B; http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html &#x200B;](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. Op **geef** wijze uit, kies een component van de Titel. Klik het **pictogram van het schildpenseel** en selecteer **onderstrepen** stijl:

   ![&#x200B; pas de onderstreepte Stijl &#x200B;](assets/style-system/apply-underline-style-title.png) toe

   >[!NOTE]
   >
   > Op dit punt vindt geen zichtbare wijziging plaats omdat de `underline` -stijl niet is geïmplementeerd. In de volgende oefening, wordt deze stijl uitgevoerd.

1. Klik het **pictogram van de Informatie van de Pagina** > **Mening zoals Gepubliceerd** om de pagina buiten de redacteur van AEM te inspecteren.
1. Gebruik de browsergereedschappen om te controleren of de CSS-klasse `cmp-title--underline` is toegepast op de buitenste div van de markering rondom de component Title.

   ![&#x200B; Div met onderstreepte toegepaste klasse &#x200B;](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### Implementeer de onderstrepingsstijl - ui.frontend

Daarna, voer de onderstreepte stijl uit gebruikend de {**module 0} ui.frontend van het project van AEM.** De webpack ontwikkelingsserver die met de {**module 0} ui.frontend &lbrace;wordt gebundeld om de stijlen *voor* het opstellen aan een lokale instantie van AEM te voorproef wordt gebruikt.**

1. Begin het `watch` proces van binnen de **ui.frontend** module:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   Hiermee wordt een proces gestart dat de wijzigingen in de module `ui.frontend` controleert en de wijzigingen in de instantie AEM synchroniseert.


1. Retourneer uw IDE en open het bestand `_title.scss` van: `ui.frontend/src/main/webpack/components/_title.scss` .
1. Introduceer een nieuwe regel voor de `cmp-title--underline` -klasse:

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >Het wordt beschouwd als beste praktijken om werkingsgebiedstijlen aan de doelcomponent altijd strak te maken. Dit zorgt ervoor dat extra stijlen andere gebieden van de pagina niet beïnvloeden.
   >
   >Alle Componenten van de Kern houden zich aan **[BEM aantekening &#x200B;](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)** aan. Het wordt aanbevolen de buitenste CSS-klasse als doel in te stellen wanneer u een standaardstijl voor een component maakt. Een andere beste manier is om klassennamen te richten die door de notatie van de Component BEM van de Kern eerder dan de elementen van HTML worden gespecificeerd.

1. Ga terug naar de browser en de AEM pagina. De stijl Onderstrepen wordt toegevoegd:

   ![&#x200B; onderstreepte stijl zichtbaar in webpack dev server &#x200B;](assets/style-system/underline-implemented-webpack.png)

1. In de Redacteur van AEM, zou u nu in en van **moeten kunnen van de Onderstreping** stijl van een knevel voorzien en zien dat de veranderingen visueel weerspiegeld.

## Stijl prijsblok - Tekst {#text-component}

Daarna, herhaal gelijkaardige stappen om een unieke stijl op de [&#x200B; Component van de Tekst &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html?lang=nl-NL) toe te passen. De component van de Tekst is proxied in het project onder `/apps/wknd/components/text` als deel van de {**module 1} ui.apps.** De standaardstijlen van paragraafelementen zijn reeds uitgevoerd in **ui.frontend**.

De [&#x200B; ontwerpen van het Artikel van WKND &#x200B;](assets/pages-templates/wknd-article-design.xd) bevatten een unieke stijl voor de component van de Tekst met een citaatblok:

![&#x200B; Stijl van het Blok van het Citaat - de Component van de Tekst &#x200B;](assets/style-system/quote-block-style.png)

### Tekstbeleid toevoegen

Voeg vervolgens een beleid toe voor de tekstcomponenten.

1. Navigeer aan het **Malplaatje van de Pagina van het Artikel** van: [&#x200B; http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html &#x200B;](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. In **wijze van de Structuur 0&rbrace;, op de belangrijkste** Container van de Lay-out **, selecteer het** pictogram van het Beleid **naast de** **component die van de Tekst onder *wordt vermeld Toegelaten Componenten*:**

   ![&#x200B; het Beleid van de Tekst vormt &#x200B;](assets/style-system/article-template-text-policy-icon.png)

1. Werk het componentenbeleid van de Tekst met de volgende waarden bij:

   *Titel van het Beleid&#42;*: **Tekst van de Inhoud**

   *Insteekmodules* > *Stijlen van de Paragraaf* > *laat paragraafstijlen* toe

   *het Lusje van Stijlen* > *voegt een nieuwe stijl* toe

   **Blok van het Citaat** : `cmp-text--quote`

   ![&#x200B; Beleid van de Component van de Tekst &#x200B;](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![&#x200B; Beleid van de Component van de Tekst 2 &#x200B;](assets/style-system/text-policy-enable-quotestyle.png)

   Klik **Gedaan** om de veranderingen in het beleid van de Tekst te bewaren.

### De stijl voor het aanhalingsteken toepassen

1. Navigeer aan **La Skateparks** artikel in de redacteur van AEM Sites bij: [&#x200B; http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html &#x200B;](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. Op **geef** wijze uit, kies een component van de Tekst. Bewerk de component om een aanhalingsteken te plaatsen:

   {de Configuratie van de Component van 0} Tekst ![&#128279;](assets/style-system/configure-text-component.png)

1. Selecteer de tekstcomponent en klik het **pictogram van het schilderpenseel** en selecteer de **stijl van het Blok van het Citaat**:

   ![&#x200B; pas de Stijl van het Blok van het Citaat toe &#x200B;](assets/style-system/quote-block-style-applied.png)

1. Gebruik de ontwikkelaarsgereedschappen van de browser om de markering te inspecteren. U ziet dat de klassenaam `cmp-text--quote` is toegevoegd aan de buitenste div van de component:

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### Implementeer de stijl voor het aanhalingsteken - ui.frontend

Daarna implementeren we de stijl van het Blok van het Citaat met behulp van de **module** ui.frontend van het AEM-project.

1. Als niet reeds lopend, begin het `watch` proces van binnen de **ui.frontend** module:

   ```shell
   $ npm run watch
   ```

1. Werk het bestand `text.scss` bij vanaf: `ui.frontend/src/main/webpack/components/_text.scss`

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > In dit geval worden onbewerkte HTML-elementen bepaald door de stijlen. De reden hiervoor is dat de component Text een Rich Text Editor biedt voor auteurs van inhoud. Het rechtstreeks maken van stijlen tegen RTE-inhoud moet met de nodige voorzichtigheid gebeuren en het is nog belangrijker om de stijlen strak uit te breiden.

1. Keer opnieuw aan browser en u zou moeten zien dat de het blokstijl van het Citaat wordt toegevoegd:

   ![&#x200B; zichtbare het blokstijl van het Citaat &#x200B;](assets/style-system/quoteblock-implemented.png)

1. Stop de webpack-ontwikkelingsserver.

## Vaste breedte - Container (Bonus) {#layout-container}

Containercomponenten zijn gebruikt om de basisstructuur van het artikelpaginasjabloon te maken en om de neerzetzones te bieden waar inhoudsauteurs inhoud aan een pagina kunnen toevoegen. Containers kunnen ook het Stijlsysteem gebruiken, waardoor de auteurs van inhoud nog meer opties voor het ontwerpen van lay-outs krijgen.

De **HoofdContainer** van het malplaatje van de Pagina van het Artikel bevat de twee auteur-able containers en heeft een vaste breedte.

![&#x200B; Belangrijkste Container &#x200B;](assets/style-system/main-container-article-page-template.png)

*Belangrijkste Container in het Malplaatje van de Pagina van het Artikel*.

Het beleid van de **Belangrijkste Container** plaatst het standaardelement als `main`:

![&#x200B; HoofdBeleid van de Container &#x200B;](assets/style-system/main-container-policy.png)

CSS die tot de **HoofdContainer** vast maakt wordt geplaatst in **ui.frontend** module bij `ui.frontend/src/main/webpack/site/styles/container_main.scss`:

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

In plaats van het richten van het `main` element van HTML, zou het Systeem van de Stijl kunnen worden gebruikt om a **Vaste breedte** stijl als deel van het beleid van de Container tot stand te brengen. Het Systeem van de Stijl kon gebruikers de optie geven om tussen **Vaste breedte** en **Dynamische breedte** containers van een knevel te voorzien.

1. **Uitdaging van de Bonen** - gebruiks lessen die van de vorige oefeningen worden geleerd en gebruik het Systeem van de Stijl om a **Vaste breedte** en **Fluid breedte** stijlen voor de component van de Container uit te voeren.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, de artikelpagina is bijna opgemaakt en u hebt praktijkervaring opgedaan met het AEM Style System.

### Volgende stappen {#next-steps}

Leer de stappen van begin tot eind om de Component van a [&#x200B; douaneAEM &#x200B;](custom-component.md) tot stand te brengen die inhoud toont die in een Dialoog wordt geschreven, en verkent het ontwikkelen van een het Verkopen Model om bedrijfslogica in te kapselen die HTML van de component bevolkt.

Bekijk de gebeëindigde code op [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van het Git `tutorial/style-system-solution` op.

1. Kloon de [&#x200B; github.com/adobe/aem-wknd-guides &#x200B;](https://github.com/adobe/aem-guides-wknd) bewaarplaats.
1. Bekijk de `tutorial/style-system-solution` -vertakking.
