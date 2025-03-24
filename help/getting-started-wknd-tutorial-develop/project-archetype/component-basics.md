---
title: Aan de slag met AEM Sites - Basisprincipes van componenten
description: Begrijp de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig "HelloWorld"voorbeeld. De onderwerpen van HTML, het Verdelen Modellen, cliënt-zijbibliotheken en auteursdialogen worden onderzocht.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
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
duration: 1715
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 0%

---

# Basisbeginselen van componenten {#component-basics}

In dit hoofdstuk, onderzoeken wij de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig `HelloWorld` voorbeeld. Kleine wijzigingen worden aangebracht aan een bestaande component, die onderwerpen van creatie, HTML, het Verdelen Modellen, cliënt-zijbibliotheken behandelt.

## Vereisten {#prerequisites}

Herzie het vereiste tooling en de instructies voor vestiging a [ lokale ontwikkelomgeving ](./overview.md#local-dev-environment).

winde in de video&#39;s wordt gebruikt is [ Code van Visual Studio ](https://code.visualstudio.com/) en de [ VSCode AEM 3} insteekmodule die van de Synchronisatie {.](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)

## Doelstelling {#objective}

1. Leer de rol van HTML-sjablonen en -modellen om HTML dynamisch te renderen.
1. Begrijp hoe Dialoogvensters worden gebruikt om het schrijven van inhoud te vergemakkelijken.
1. Leer de basisbeginselen zelf van Client-side bibliotheken om CSS en JavaScript op te nemen om een component te steunen.

## Wat u gaat bouwen {#what-build}

In dit hoofdstuk voert u verschillende wijzigingen uit in een eenvoudige `HelloWorld` -component. Wanneer u updates uitvoert naar de component `HelloWorld` , krijgt u meer informatie over de belangrijkste gebieden van de ontwikkeling van AEM-componenten.

## Hoofdstukstartproject {#starter-project}

Dit hoofdstuk bouwt op een generisch project voort dat door het [ Archetype van het Project van AEM ](https://github.com/adobe/aem-project-archetype) wordt geproduceerd. Bekijk de hieronder video en herzie de [ eerste vereisten ](#prerequisites) om begonnen te worden!

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide, kunt u het project opnieuw gebruiken en de stappen overslaan voor het uitchecken van het starterproject.

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

Open een nieuwe opdrachtregelterminal en voer de volgende handelingen uit.

1. In een lege folder, kloon de [ aem-gidsen-wint ](https://github.com/adobe/aem-guides-wknd) bewaarplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Naar keuze, kunt u blijven gebruikend het project dat in het vorige hoofdstuk, [ de Opstelling van het Project ](./project-setup.md) wordt geproduceerd.

1. Navigeer naar de map `aem-guides-wknd` .

   ```shell
   $ cd aem-guides-wknd
   ```

1. Bouw en stel het project aan een lokaal geval van AEM met het volgende bevel op:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Als u AEM 6.5 of 6.4 gebruikt, voegt u het `classic` -profiel toe aan Maven-opdrachten.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importeer het project in uw aangewezen winde door de instructies te volgen aan opstelling a [ lokale ontwikkelomgeving ](overview.md#local-dev-environment).

## Componentontwerp {#component-authoring}

Componenten kunnen worden beschouwd als kleine modulaire bouwstenen van een webpagina. Om componenten opnieuw te gebruiken, moeten de componenten configureerbaar zijn. Dit gebeurt via het dialoogvenster van de auteur. Laten we nu een eenvoudige component schrijven en controleren hoe waarden uit het dialoogvenster in AEM worden gepresteerd.

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

Hieronder vindt u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Creeer een pagina genoemd **Grondbeginselen van de Component** onder **Plaats WKND** `>` **VS** `>` ****.
1. Voeg de **Component van de Wereld van Hello** aan de pas gecreëerde pagina toe.
1. Open het dialoogvenster voor de component en voer tekst in. Sla de wijzigingen op om het bericht op de pagina weer te geven.
1. Schakel in naar de modus Ontwikkelaar en bekijk het inhoudspad in CRXDE-Lite en controleer de eigenschappen van de componentinstantie.
1. Gebruik CRXDE-Lite om het `cq:dialog` en `helloworld.html` script van `/apps/wknd/components/content/helloworld` weer te geven.

## HTML (HTML Template Language) en dialoogvensters {#htl-dialogs}

De Taal van het Malplaatje van HTML of **[HTML ](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)** is een licht-gewicht, server-zijhet bemonsteren taal die door de componenten van AEM wordt gebruikt om inhoud terug te geven.

**de Dialogen** bepalen de beschikbare configuraties die voor een component kunnen worden gemaakt.

Vervolgens werkt u het HTML-script van `HelloWorld` bij om een extra begroeting weer te geven vóór het tekstbericht.

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

Hieronder vindt u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Ga naar winde en open het project aan de `ui.apps` module.
1. Open het `helloworld.html` -bestand en werk de HTML Markup bij.
1. Gebruik de hulpmiddelen van winde zoals [ de Synchronisatie van AEM VSCode ](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) om de dossierverandering met de lokale instantie van AEM te synchroniseren.
1. Ga terug naar de browser en bekijk hoe de component is gerenderd.
1. Open het bestand `.content.xml` dat het dialoogvenster voor de `HelloWorld` -component definieert op:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Werk de dialoog bij om een extra genoemd tekstgebied **Titel** met een naam van `./title` toe te voegen:

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

1. Open het bestand `helloworld.html` opnieuw, dat het hoofd-HTML-script vertegenwoordigt dat verantwoordelijk is voor het renderen van de `HelloWorld` -component vanuit het onderliggende pad:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Update `helloworld.html` om de waarde van **Wensend** textfield als deel van een `H1` markering terug te geven:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de plug-in voor ontwikkelaars of met behulp van uw Maven-vaardigheden.

## Verkoopmodellen {#sling-models}

Sling-modellen zijn annotaties die worden aangedreven door Java™ &quot;POJO&#39;s&quot; (gewone oude Java™-objecten) en die het gemakkelijker maken gegevens van de JCR aan Java™-variabelen toe te wijzen. Ze bieden ook diverse andere genootschappen bij het ontwikkelen in de context van AEM.

Laten we nu enkele updates uitvoeren op het `HelloWorldModel` Sling-model om enige bedrijfslogica toe te passen op de waarden die zijn opgeslagen in de JCR voordat we deze op de pagina uitvoeren.

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. Open het bestand `HelloWorldModel.java` , het Sling-model dat wordt gebruikt met de component `HelloWorld` .

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Voeg de volgende instructies voor importeren toe:

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Werk de annotatie `@Model` bij als u een `DefaultInjectionStrategy` wilt gebruiken:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Voeg de volgende regels toe aan de klasse `HelloWorldModel` om de waarden van de JCR-eigenschappen `title` en `text` van de component toe te wijzen aan Java™-variabelen:

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

1. Voeg de volgende methode `getTitle()` toe aan de `HelloWorldModel` -klasse die de waarde van de eigenschap met de naam `title` retourneert. Deze methode voegt de extra logica toe om een waarde van het Koord van &quot;StandaardWaarde hier terug te keren!&quot; als de eigenschap `title` null of leeg is:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Voeg de volgende methode `getText()` toe aan de `HelloWorldModel` -klasse die de waarde van de eigenschap met de naam `text` retourneert. Deze methode transformeert de tekenreeks naar alle hoofdletters.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. De bundel maken en implementeren vanuit de module `core` :

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Gebruik voor AEM 6.4/6.5 `mvn clean install -PautoInstallBundle -Pclassic`

1. Werk het bestand `helloworld.html` bij `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` om de nieuwe methoden van het model `HelloWorld` te gebruiken.

   Het `HelloWorld` -model wordt voor deze componentinstantie geïnstantieerd via de HTL-instructie: `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"` , waarbij de instantie wordt opgeslagen in de variabele `model` .

   De modelinstantie `HelloWorld` is nu beschikbaar in de HTML via de variabele `model` met behulp van `HelloWord` . Deze methodeaanroepen kunnen bijvoorbeeld gebruik maken van een verkorte syntaxis: `${model.getTitle()}` kan worden ingekort tot `${model.title}` .

   Op dezelfde manier worden alle manuscripten HTML geïnjecteerd met [ globale voorwerpen ](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) die kunnen worden betreden gebruikend de zelfde syntaxis zoals de het Verdelen Modelvoorwerpen.

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

1. Implementeer de wijzigingen in een lokale versie van AEM met de plug-in Eclipse Developer of met behulp van uw Maven-vaardigheden.

## Client-Side bibliotheken {#client-side-libraries}

Client-Side Libraries, `clientlibs` for short, biedt een mechanisme voor het organiseren en beheren van CSS- en JavaScript-bestanden die nodig zijn voor een AEM Sites-implementatie. Bibliotheken op de client zijn de standaardmanier om CSS en JavaScript op een pagina in AEM op te nemen.

De {](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) module 0} ui.frontend is een ontkoppelde [ webpack ](https://webpack.js.org/) project dat in het bouwstijlproces wordt geïntegreerd. [ Hiermee kunt u populaire front-end bibliotheken gebruiken, zoals Sass, LESS en TypeScript. De `ui.frontend` module wordt verkend in meer diepte in het [ Client-Side hoofdstuk van Bibliotheken ](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md).

Werk vervolgens de CSS-stijlen voor de component `HelloWorld` bij.

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

Hieronder vindt u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Open een terminalvenster en navigeer in de map `ui.frontend`

1. Het zijn in `ui.frontend` folder stelt het `npm install npm-run-all --save-dev` bevel in werking om [ npm-looppas-alle ](https://www.npmjs.com/package/npm-run-all) knoopmodule te installeren. Deze stap wordt **vereist op Archetype 39 geproduceerd AEM project**, in de aanstaande versie van Archetype dit wordt niet vereist.

1. Voer vervolgens de opdracht `npm run watch` uit:

   ```shell
   $ npm run watch
   ```

1. Ga naar winde en open het project aan de `ui.frontend` module.
1. Open het bestand `ui.frontend/src/main/webpack/components/_helloworld.scss` .
1. Werk het bestand bij om een rode titel weer te geven:

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. In de terminal ziet u activiteit die aangeeft dat de module `ui.frontend` de wijzigingen compileert en synchroniseert met de lokale instantie van AEM.

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

   ![ update van de Basisbeginselen van de Component ](assets/component-basics/color-update.png)

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt de grondbeginselen van componentenontwikkeling in Adobe Experience Manager geleerd!

### Volgende stappen {#next-steps}

Krijg vertrouwd met de pagina&#39;s en de malplaatjes van Adobe Experience Manager in het volgende hoofdstuk [ Pagina&#39;s en Malplaatjes ](pages-templates.md). Begrijp hoe de Componenten van de Kern in het project proxied zijn en geavanceerde beleidsconfiguraties van editable malplaatjes leren om een goed-gestructureerd malplaatje van de Pagina van het Artikel te bouwen.

Bekijk de gebeëindigde code op [ GitHub ](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van het Git `tutorial/component-basics-solution` op.
