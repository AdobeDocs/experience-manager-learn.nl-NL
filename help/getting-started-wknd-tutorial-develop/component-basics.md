---
title: Aan de slag met AEM Sites - Basisprincipes van componenten
description: Begrijp de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig "HelloWorld"voorbeeld. De onderwerpen van HTML, Verzamelmodellen, cliënt-zijbibliotheken en auteursdialogen worden onderzocht.
sub-product: sites
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 0%

---


# Basisbeginselen van componenten {#component-basics}

In dit hoofdstuk zullen wij de onderliggende technologie van een Component van de Plaatsen van Adobe Experience Manager (AEM) door een eenvoudig `HelloWorld` voorbeeld onderzoeken. Kleine wijzigingen worden aangebracht aan een bestaande component, die onderwerpen van creatie, HTML, Verschilderende Modellen, cliënt-zijbibliotheken behandelt.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment).

## Doel {#objective}

1. Leer de rol van HTML-sjablonen en -modellen om HTML dynamisch te renderen.
1. Begrijp hoe Dialoogvensters worden gebruikt om het schrijven van inhoud te vergemakkelijken.
1. Leer de basisbeginselen zelf van Client-side bibliotheken om CSS en JavaScript op te nemen om een component te steunen.

## Wat u {#what-you-will-build} wilt maken

In dit hoofdstuk zult u verscheidene wijzigingen in een zeer eenvoudige `HelloWorld` component uitvoeren. Tijdens het maken van updates van de `HelloWorld` component zult u over de belangrijkste gebieden van AEM componentenontwikkeling leren.

## Hoofdproject {#starter-project}

Dit hoofdstuk bouwt op een generisch project voort dat door [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) wordt geproduceerd. Bekijk de onderstaande video en bekijk de [voorwaarden](#prerequisites) om aan de slag te gaan!

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

Open een nieuwe opdrachtregelterminal en voer de volgende handelingen uit.

1. In een lege directory kloont u de [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repository:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > Naar keuze, kunt u [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip) tak direct downloaden.

1. Navigeer in de `aem-guides-wknd` folder:

   ```shell
   $ cd aem-guides-wknd
   ```

1. Overschakelen naar de `component-basics/start`-vertakking:

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. Bouw en stel het project aan een lokaal geval van AEM met het volgende bevel op:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. Importeer het project in uw aangewezen winde door de instructies aan opstelling te volgen [lokale ontwikkelomgeving](overview.md#local-dev-environment).

## Componentontwerp {#component-authoring}

Componenten kunnen worden beschouwd als kleine modulaire bouwstenen van een webpagina. Om componenten opnieuw te gebruiken, moeten de componenten configureerbaar zijn. Dit gebeurt via het dialoogvenster van de auteur. Daarna zullen wij een eenvoudige component ontwerpen en inspecteren hoe de waarden van de dialoog in AEM worden voortgeduurd.

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Maak een nieuwe pagina met de naam **Basisbeginselen van componenten** onder **WKND-site** `>` **US** `>` **en**.
1. Voeg de **Hello World-component** toe aan de nieuw gemaakte pagina.
1. Open het dialoogvenster voor de component en voer tekst in. Sla de wijzigingen op om het bericht op de pagina weer te geven.
1. Schakel in naar de modus Ontwikkelaar en bekijk het inhoudspad in CRXDE-Lite en controleer de eigenschappen van de componentinstantie.
1. Gebruik CRXDE-Lite om het `cq:dialog` en `helloworld.html` manuscript te bekijken dat bij `/apps/wknd/components/content/helloworld` wordt gevestigd.

## HTML-sjabloontaal (HTL) {#htl-templates}

HTML-sjabloontaal of [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html) is een lichtgewicht, serversjabloontaal die door AEM componenten wordt gebruikt om inhoud te renderen.

Vervolgens wordt het HTML-script `HelloWorld` bijgewerkt om een extra begroeting vóór het tekstbericht weer te geven.

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Schakelaar aan winde van de Verduistering en open het project aan `ui.apps` module.
1. Open het `.content.xml`-bestand dat het dialoogvenster definieert voor de `HelloWorld`-component op:

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. Werk het dialoogvenster bij om een extra tekstveld met de naam **Wensing** toe te voegen met de naam `./greeting`:

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
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
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

1. Open het bestand `helloworld.html`, dat het hoofd-HTML-script vertegenwoordigt dat verantwoordelijk is voor het renderen van de `HelloWorld`-component in:

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. Werk `helloworld.html` bij om de waarde van het tekstveld **Greeting** te renderen als onderdeel van een `H1`-tag:

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de Eclipse Developer-plug-in of met behulp van uw Maven-vaardigheden.

## Sling-modellen {#sling-models}

Sling-modellen zijn annotaties die worden aangedreven door Java &quot;POJO&#39;s&quot; (Plain Old Java Objects) en die het gemakkelijker maken om gegevens van de JCR aan Java-variabelen toe te wijzen en die een aantal andere problemen bieden bij het ontwikkelen in de context van AEM.

Daarna, zullen wij sommige updates aan `HelloWorldModel` het Verkopen Model om wat bedrijfslogica op de waarden toe te passen die in JCR worden opgeslagen alvorens hen aan de pagina uit te voeren.

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. Open het bestand `HelloWorldModel.java`. Dit is het verkoopmodel dat wordt gebruikt met de component `HelloWorld`.

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. Voeg de volgende regels toe aan de klasse `HelloWorldModel` om de waarden van de JCR-eigenschappen `greeting` en `text` van de component toe te wijzen aan Java-variabelen:

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. Voeg de volgende methode `getGreeting()` aan `HelloWorldModel` klasse toe, die de waarde van het bezit genoemd `greeting` terugkeert. Deze methode voegt de extra logica toe om een waarde van het Koord van &quot;Hello&quot;terug te keren als het bezit `greeting` ongeldig of leeg is:

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. Voeg de volgende methode `getTextUpperCase()` aan `HelloWorldModel` klasse toe, die de waarde van het bezit genoemd `text` terugkeert. Deze methode transformeert de tekenreeks naar alle tekens in het hoofdlettergebruik.

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. Werk het bestand `helloworld.html` om `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` bij om de nieuwe methoden van het `HelloWorld`-model te gebruiken:

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de Eclipse Developer-plug-in of met behulp van uw Maven-vaardigheden.

## Client-Side bibliotheken {#client-side-libraries}

Client-Side Libraries, clientlibs for short, biedt een mechanisme voor het organiseren en beheren van CSS- en JavaScript-bestanden die nodig zijn voor een AEM Sites-implementatie. Bibliotheken op de client zijn de standaardmanier om CSS en JavaScript op een pagina in AEM op te nemen.

Vervolgens worden enkele CSS-stijlen voor de component `HelloWorld` opgenomen om inzicht te krijgen in de basisbeginselen van bibliotheken op de client.

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

Hieronder ziet u de stappen op hoog niveau die in de bovenstaande video worden uitgevoerd.

1. Onder `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` creeer een nieuw knooppunt genoemd `clientlibs` met een knooptype van `cq:ClientLibraryFolder`.
1. Maak een map- en bestandsstructuur zoals hieronder `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. Vul `helloworld/clientlibs/css/helloworld.css` met het volgende:

   ```css
   .cmp-helloworld h1 {
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
   console.log("hello world!");
   ```

1. Vul `helloworld/clientlibs/js.txt` met het volgende:

   ```plain
   #base=js
   helloworld.js
   ```

1. Werk de `clientlibs` knoopeigenschappen bij om de volgende twee eigenschappen te omvatten:

   | Naam | Type | Waarde |
   |------|------|-------|
   | categorieën | Tekenreeks | wknd.base |
   | allowProxy | Boolean | true |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. Implementeer de wijzigingen in een lokale versie van AEM met de Eclipse Developer-plug-in of met behulp van uw Maven-vaardigheden.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt zojuist de basisbeginselen van componentontwikkeling in Adobe Experience Manager geleerd!

### Volgende stappen {#next-steps}

In het volgende hoofdstuk [Pagina&#39;s en sjablonen](pages-templates.md) leren u vertrouwd met Adobe Experience Manager-pagina&#39;s en -sjablonen. Begrijp hoe de Componenten van de Kern in het project proxied zijn en geavanceerde beleidsconfiguraties van editable malplaatjes leren om een goed-gestructureerd malplaatje van de Pagina van het Artikel te bouwen.

Bekijk de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de schakelaar van de Git `component-basics/solution` op.

1. Clone the [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repository.
1. De `component-basics/solution`-vertakking uitchecken
