---
title: Aan de slag met AEM Sites - Basisprincipes van componenten
description: Begrijp de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig "HelloWorld"voorbeeld. De onderwerpen van HTML, het Verdelen Modellen, cliënt-zijbibliotheken en auteursdialogen worden onderzocht.
version: 6.5, Cloud Service
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4081
thumbnail: 30177.jpg
doc-type: Tutorial
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
duration: 1778
source-git-commit: 0400242f6a99bc5209a8b483469d5fd88eac077e
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 0%

---

# Basisbeginselen van componenten {#component-basics}

In dit hoofdstuk, onderzoeken wij de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig `HelloWorld` voorbeeld. Kleine wijzigingen worden aangebracht aan een bestaande component, die onderwerpen van creatie, HTML, het Verdelen Modellen, cliënt-zijbibliotheken behandelt.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](./overview.md#local-dev-environment).

De IDE die in de video&#39;s wordt gebruikt is [Visual Studio-code](https://code.visualstudio.com/) en de [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) insteekmodule.

## Doelstelling {#objective}

1. Leer de rol van malplaatjes HTML en het Verdelen Modellen om HTML dynamisch terug te geven.
1. Begrijp hoe Dialoogvensters worden gebruikt om het schrijven van inhoud te vergemakkelijken.
1. Leer de basisbeginselen zelf van Client-side bibliotheken om CSS en JavaScript op te nemen om een component te steunen.

## Wat u gaat bouwen {#what-build}

In dit hoofdstuk voert u verschillende wijzigingen in een eenvoudig `HelloWorld` component. Tijdens het uitvoeren van updates voor de `HelloWorld` leert u over de belangrijkste gebieden van de ontwikkeling van AEM component.

## Hoofdstukstartproject {#starter-project}

Dit hoofdstuk bouwt op een generisch project voort dat door wordt geproduceerd [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype). Bekijk de onderstaande video en bekijk de [voorwaarden](#prerequisites) om te beginnen!

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide, kunt u het project opnieuw gebruiken en de stappen overslaan voor het uitchecken van het starterproject.

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

Open een nieuwe opdrachtregelterminal en voer de volgende handelingen uit.

1. In een lege map kloont u de [aem-hulplijnen](https://github.com/adobe/aem-guides-wknd) opslagplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > U kunt desgewenst het in het vorige hoofdstuk gegenereerde project blijven gebruiken. [Projectinstelling](./project-setup.md).

1. Ga in  `aem-guides-wknd` map.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Bouw en stel het project aan een lokaal geval van AEM met het volgende bevel op:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Indien u AEM 6.5 of 6.4 gebruikt, voegt u de `classic` aan om het even welke Gemaakt bevelen.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importeer het project in uw gewenste IDE door de instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment).

## Componentontwerp {#component-authoring}

Componenten kunnen worden beschouwd als kleine modulaire bouwstenen van een webpagina. Om componenten opnieuw te gebruiken, moeten de componenten configureerbaar zijn. Dit gebeurt via het dialoogvenster van de auteur. Laten we nu de auteur van een eenvoudige component bekijken en controleren hoe waarden uit het dialoogvenster in AEM blijven.

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

Hieronder vindt u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Een pagina maken met de naam **Basisbeginselen van componenten** beneide **WKND-site** `>` **VS** `>` **en**.
1. Voeg de **Hello World-component** naar de nieuwe pagina.
1. Open het dialoogvenster voor de component en voer tekst in. Sla de wijzigingen op om het bericht op de pagina weer te geven.
1. Schakel in naar de modus Ontwikkelaar en bekijk het inhoudspad in CRXDE-Lite en controleer de eigenschappen van de componentinstantie.
1. CRXDE-Lite gebruiken om de `cq:dialog` en `helloworld.html` script van `/apps/wknd/components/content/helloworld`.

## HTL (Sjabloontaal HTML) en dialoogvensters {#htl-dialogs}

HTML Template Language of **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)** is een lichtgewicht, server-side sjabloontaal die door AEM componenten wordt gebruikt om inhoud te renderen.

**Dialoogvensters** definieert de beschikbare configuraties die voor een component kunnen worden gemaakt.

Laten we nu de `HelloWorld` HTML- manuscript om een extra groet vóór het tekstbericht te tonen.

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

Hieronder vindt u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Schakelaar aan winde en open het project aan `ui.apps` -module.
1. Open de `helloworld.html` en werk de HTML Markup bij.
1. Gebruik de hulpmiddelen van winde zoals [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) om de bestandswijziging te synchroniseren met de lokale AEM.
1. Ga terug naar de browser en bekijk hoe de component is gerenderd.
1. Open de `.content.xml` bestand dat het dialoogvenster voor het `HelloWorld` component bij:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Het dialoogvenster bijwerken om een extra tekstveld met de naam **Titel** met een naam van `./title`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. Bestand opnieuw openen `helloworld.html`, die het belangrijkste HTML-script vertegenwoordigt dat verantwoordelijk is voor het renderen van de `HelloWorld` component van onder pad:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Bijwerken `helloworld.html` om de waarde van de **Wenskaart** tekstveld als onderdeel van een `H1` tag:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de plug-in voor ontwikkelaars of met behulp van uw Maven-vaardigheden.

## Verkoopmodellen {#sling-models}

Sling-modellen zijn annotaties die worden aangedreven door Java™ &quot;POJO&#39;s&quot; (gewone oude Java™-objecten) en die het gemakkelijker maken gegevens van de JCR aan Java™-variabelen toe te wijzen. Ze bieden ook diverse andere genootschappen bij het ontwikkelen in het kader van AEM.

Laten we nu enkele updates uitvoeren voor de `HelloWorldModel` Het Model van de verkoop om wat bedrijfslogica op de waarden toe te passen die in JCR worden opgeslagen alvorens hen aan de pagina uit te voeren.

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. Het bestand openen `HelloWorldModel.java`, dat het verkoopmodel is dat wordt gebruikt met de `HelloWorld` component.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Voeg de volgende instructies voor importeren toe:

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Werk de `@Model` aantekening om een `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Voeg de volgende regels toe aan de `HelloWorldModel` klasse om de waarden van de JCR-eigenschappen van de component in kaart te brengen `title` en `text` naar Java™-variabelen:

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. De volgende methode toevoegen `getTitle()` aan de `HelloWorldModel` klasse, die de waarde van het genoemde bezit terugkeert `title`. Deze methode voegt de extra logica toe om een waarde van het Koord van &quot;StandaardWaarde hier terug te keren!&quot; if de eigenschap `title` is null of leeg:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. De volgende methode toevoegen `getText()` aan de `HelloWorldModel` klasse, die de waarde van het genoemde bezit terugkeert `text`. Deze methode transformeert de tekenreeks naar alle hoofdletters.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. De bundel maken en implementeren vanuit de `core` module:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Voor AEM 6.4/6.5 `mvn clean install -PautoInstallBundle -Pclassic`

1. Het bestand bijwerken `helloworld.html` om `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` om de nieuw gecreëerde methodes van `HelloWorld` model.

   De `HelloWorld` model wordt voor deze componentinstantie geïnstantieerd via de HTL-instructie: `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"`, de instantie opslaan in de variabele `model`.

   De `HelloWorld` modelexemplaar is nu beschikbaar in het HTML via `model` variabele die de `HelloWord`. Deze methodeaanroepen kunnen bijvoorbeeld een verkorte syntaxis gebruiken: `${model.getTitle()}` kan worden ingekort tot `${model.title}`.

   Op dezelfde manier worden alle HTML-scripts geïnjecteerd met [algemene objecten](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) die kunnen worden benaderd met dezelfde syntaxis als de objecten van het verkoopmodel.

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld" 
       data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
   </div>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de Eclipse Developer-plug-in of met behulp van uw Maven-vaardigheden.

## Client-Side bibliotheken {#client-side-libraries}

Client-Side Libraries, `clientlibs` Kortom, biedt een mechanisme voor het organiseren en beheren van CSS- en JavaScript-bestanden die nodig zijn voor een AEM Sites-implementatie. Bibliotheken op de client zijn de standaardmanier om CSS en JavaScript op een pagina in AEM op te nemen.

De [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) module is een ontkoppelde [webpack](https://webpack.js.org/) project dat in het bouwstijlproces wordt geïntegreerd. Hiermee kunt u populaire front-end bibliotheken gebruiken, zoals Sass, LESS en TypeScript. De `ui.frontend` wordt meer in detail besproken in de [Hoofdstuk Client-Side Bibliotheken](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md).

Werk vervolgens de CSS-stijlen bij voor de `HelloWorld` component.

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

Hieronder vindt u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Open een terminalvenster en navigeer in de `ui.frontend` directory

1. Aanmelden `ui.frontend` directory voert de `npm install npm-run-all --save-dev` om de [npm-run-all](https://www.npmjs.com/package/npm-run-all) knooppuntmodule. Deze stap is **vereist voor AEM project Archetype 39**, in de komende versie van Archetype is dit niet vereist.

1. Voer vervolgens het `npm run watch` opdracht:

   ```shell
   $ npm run watch
   ```

1. Schakelaar aan winde en open het project aan `ui.frontend` -module.
1. Het bestand openen `ui.frontend/src/main/webpack/components/_helloworld.scss`.
1. Werk het bestand bij om een rode titel weer te geven:

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. In de terminal moet u activiteit zien die erop wijst dat de id `ui.frontend` de wijzigingen worden gecompileerd en gesynchroniseerd met de lokale AEM.

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. Ga terug naar de browser en controleer of de titelkleur is gewijzigd.

   ![Basisbeginselen van componenten bijwerken](assets/component-basics/color-update.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt de grondbeginselen van componentenontwikkeling in Adobe Experience Manager geleerd!

### Volgende stappen {#next-steps}

In het volgende hoofdstuk leren u vertrouwd met Adobe Experience Manager-pagina&#39;s en -sjablonen [Pagina&#39;s en sjablonen](pages-templates.md). Begrijp hoe de Componenten van de Kern in het project proxied zijn en geavanceerde beleidsconfiguraties van editable malplaatjes leren om een goed-gestructureerd malplaatje van de Pagina van het Artikel te bouwen.

De voltooide code weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van de it op `tutorial/component-basics-solution`.
