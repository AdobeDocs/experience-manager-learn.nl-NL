---
title: Aan de slag met AEM Sites - Basisprincipes van componenten
description: Begrijp de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig "HelloWorld"voorbeeld. De onderwerpen van HTML, Verzamelmodellen, cliënt-zijbibliotheken en auteursdialogen worden onderzocht.
sub-product: sites
feature: Core Components, Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
topic: Inhoudsbeheer, ontwikkeling
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: fb6c56dfc85fbcb36a68210f068fd496849c352e
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 0%

---


# Basisbeginselen van componenten {#component-basics}

In dit hoofdstuk zullen wij de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig `HelloWorld` voorbeeld onderzoeken. Kleine wijzigingen worden aangebracht aan een bestaande component, die onderwerpen van creatie, HTML, Verschilderende Modellen, cliënt-zijbibliotheken behandelt.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](./overview.md#local-dev-environment).

winde die in de video&#39;s wordt gebruikt is [Visual Studio Code](https://code.visualstudio.com/) en [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) stop.

## Doel {#objective}

1. Leer de rol van HTML-sjablonen en -modellen om HTML dynamisch te renderen.
1. Begrijp hoe Dialoogvensters worden gebruikt om het schrijven van inhoud te vergemakkelijken.
1. Leer de basisbeginselen zelf van Client-side bibliotheken om CSS en JavaScript op te nemen om een component te steunen.

## Wat u {#what-you-will-build} wilt maken

In dit hoofdstuk zult u verscheidene wijzigingen in een zeer eenvoudige `HelloWorld` component uitvoeren. Tijdens het maken van updates van de `HelloWorld` component zult u over de belangrijkste gebieden van AEM componentenontwikkeling leren.

## Hoofdproject {#starter-project}

Dit hoofdstuk bouwt op een generisch project voort dat door [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) wordt geproduceerd. Bekijk de onderstaande video en bekijk de [voorwaarden](#prerequisites) om aan de slag te gaan!

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide kunt u het project hergebruiken en de stappen overslaan voor het uitchecken van het starterproject.

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

Open een nieuwe opdrachtregelterminal en voer de volgende handelingen uit.

1. In een lege directory kloont u de [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > Naar keuze, kunt u blijven gebruikend het project in het vorige hoofdstuk, [Opstelling van het Project ](./project-setup.md) wordt geproduceerd.

1. Navigeer in de `aem-guides-wknd` omslag.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Bouw en stel het project aan een lokaal geval van AEM met het volgende bevel op:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Als u AEM 6.5 of 6.4 gebruikt, voegt u het `classic`-profiel toe aan alle Maven-opdrachten.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Importeer het project in uw aangewezen winde door de instructies aan opstelling te volgen [lokale ontwikkelomgeving](overview.md#local-dev-environment).

## Componentontwerp {#component-authoring}

Componenten kunnen worden beschouwd als kleine modulaire bouwstenen van een webpagina. Om componenten opnieuw te gebruiken, moeten de componenten configureerbaar zijn. Dit gebeurt via het dialoogvenster van de auteur. Daarna zullen wij een eenvoudige component ontwerpen en inspecteren hoe de waarden van de dialoog in AEM worden voortgeduurd.

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Maak een nieuwe pagina met de naam **Basisbeginselen van componenten** onder **WKND-site** `>` **US** `>` **en**.
1. Voeg de **Hello World-component** toe aan de nieuw gemaakte pagina.
1. Open het dialoogvenster voor de component en voer tekst in. Sla de wijzigingen op om het bericht op de pagina weer te geven.
1. Schakel in naar de modus Ontwikkelaar en bekijk het inhoudspad in CRXDE-Lite en controleer de eigenschappen van de componentinstantie.
1. Gebruik CRXDE-Lite om het `cq:dialog` en `helloworld.html` manuscript te bekijken dat bij `/apps/wknd/components/content/helloworld` wordt gevestigd.

## HTML (HTML Template Language) en Dialoogvensters {#htl-dialogs}

HTML-sjabloontaal of **[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)** is een lichtgewicht sjabloontaal aan de serverzijde die door AEM componenten wordt gebruikt om inhoud te renderen.

**Met** dialoogvensters worden de beschikbare configuraties gedefinieerd die voor een component kunnen worden gemaakt.

Vervolgens wordt het HTML-script `HelloWorld` bijgewerkt om een extra begroeting vóór het tekstbericht weer te geven.

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Schakelaar aan winde en open het project aan `ui.apps` module.
1. Open het `helloworld.html` dossier en breng een verandering in de Prijsverhoging van HTML aan.
1. Gebruik de hulpmiddelen van winde zoals [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) om de dossierverandering met de lokale AEM instantie te synchroniseren.
1. Ga terug naar de browser en bekijk hoe de component is gerenderd.
1. Open het `.content.xml`-bestand dat het dialoogvenster definieert voor de `HelloWorld`-component op:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Werk het dialoogvenster bij om een extra tekstveld met de naam **Title** toe te voegen met de naam `./title`:

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

1. Open het bestand `helloworld.html` opnieuw, dat het hoofd-HTML-script vertegenwoordigt dat verantwoordelijk is voor het renderen van de `HelloWorld`-component in:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. Werk `helloworld.html` bij om de waarde van het tekstveld **Greeting** te renderen als onderdeel van een `H1`-tag:

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de plug-in voor ontwikkelaars of met behulp van uw Maven-vaardigheden.

## Sling-modellen {#sling-models}

Sling-modellen zijn annotaties die worden aangedreven door Java &quot;POJO&#39;s&quot; (Plain Old Java Objects) en die het gemakkelijker maken om gegevens van de JCR aan Java-variabelen toe te wijzen en die een aantal andere problemen bieden bij het ontwikkelen in de context van AEM.

Daarna, zullen wij sommige updates aan `HelloWorldModel` het Verkopen Model om wat bedrijfslogica op de waarden toe te passen die in JCR worden opgeslagen alvorens hen aan de pagina uit te voeren.

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. Open het bestand `HelloWorldModel.java`. Dit is het verkoopmodel dat wordt gebruikt met de component `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Voeg de volgende instructies voor importeren toe:

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. Werk de `@Model`-annotatie bij om een `DefaultInjectionStrategy` te gebruiken:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. Voeg de volgende regels toe aan de klasse `HelloWorldModel` om de waarden van de JCR-eigenschappen `title` en `text` van de component toe te wijzen aan Java-variabelen:

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

1. Voeg de volgende methode `getTitle()` aan `HelloWorldModel` klasse toe, die de waarde van het bezit genoemd `title` terugkeert. Deze methode voegt de extra logica toe om een waarde van het Koord van &quot;StandaardWaarde hier terug te keren!&quot; als de eigenschap `title` null of leeg is:

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. Voeg de volgende methode `getText()` aan `HelloWorldModel` klasse toe, die de waarde van het bezit genoemd `text` terugkeert. Deze methode transformeert de tekenreeks naar alle hoofdletters.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. De bundel maken en implementeren vanuit de module `core`:

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > Als u AEM 6.4/6.5 gebruikt `mvn clean install -PautoInstallBundle -Pclassic`

1. Werk het bestand `helloworld.html` om `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` bij om de nieuwe methoden van het `HelloWorld`-model te gebruiken:

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
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de Eclipse Developer-plug-in of met behulp van uw Maven-vaardigheden.

## Client-Side bibliotheken {#client-side-libraries}

Client-Side Libraries, clientlibs for short, biedt een mechanisme voor het organiseren en beheren van CSS- en JavaScript-bestanden die nodig zijn voor een AEM Sites-implementatie. Bibliotheken op de client zijn de standaardmanier om CSS en JavaScript op een pagina in AEM op te nemen.

Vervolgens worden enkele CSS-stijlen voor de component `HelloWorld` opgenomen om inzicht te krijgen in de basisbeginselen van bibliotheken op de client.

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Onder `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs` creeer een nieuwe omslag genoemd `clientlib-helloworld`.
1. Maak een map- en bestandsstructuur zoals hieronder `clientlibs`

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. Vul `helloworld.css` met het volgende:

   ```css
   .cmp-helloworld .cmp-helloworld__title {
       color: red;
   }
   ```

1. Vul `helloworld/clientlibs/css.txt` met het volgende:

   ```plain
   #base=css
   helloworld.css
   ```

1. Vul `helloworld/clientlibs/js/helloworld.js` met het volgende:

   ```js
   console.log("Hello World from Javascript!");
   ```

1. Vul `helloworld/clientlibs/js.txt` met het volgende:

   ```plain
   #base=js
   helloworld.js
   ```

1. Werk het `clientlib-helloworld/.conten.xml` dossier bij om de volgende eigenschappen te omvatten:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. Werk het `clientlibs/clientlib-base/.content.xml` dossier aan **embed** de `wknd.helloworld` categorie bij:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de plug-in voor ontwikkelaars of met behulp van uw Maven-vaardigheden.

   >[!NOTE]
   >
   > CSS en JavaScript worden vaak in cache geplaatst door de browser vanwege de prestaties. Als u niet onmiddellijk de verandering voor de cliëntbibliotheek ziet voer hard uit verfrist zich en ontruim browser geheim voorgeheugen. Het kan handig zijn om een incognitovenster te gebruiken voor een nieuwe cache.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt zojuist de basisbeginselen van componentontwikkeling in Adobe Experience Manager geleerd!

### Volgende stappen {#next-steps}

In het volgende hoofdstuk [Pagina&#39;s en sjablonen](pages-templates.md) leren u vertrouwd met Adobe Experience Manager-pagina&#39;s en -sjablonen. Begrijp hoe de Componenten van de Kern in het project proxied zijn en geavanceerde beleidsconfiguraties van editable malplaatjes leren om een goed-gestructureerd malplaatje van de Pagina van het Artikel te bouwen.

Bekijk de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van het Git `tutorial/component-basics-solution` op.
