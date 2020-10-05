---
title: Ontwikkelen met de AEM SPA Editor - Hello World-zelfstudie
description: AEM de Redacteur van het KUUROORD verleent steun voor in-context het uitgeven van Één enkele Toepassing of KUUROORD van de Pagina. Dit leerprogramma is een inleiding aan de ontwikkeling van het KUUROORD die met AEM Redacteur JS SDK van het KUUROORD moet worden gebruikt. De zelfstudie breidt de app We.Retail Journal uit door een aangepaste Hello World-component toe te voegen. Gebruikers kunnen de zelfstudie voltooien met Reageren of Hoekframes.
sub-product: sites, content-services
feature: spa-editor
topics: development, single-page-applications
audience: developer
doc-type: tutorial
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '3153'
ht-degree: 0%

---


# Ontwikkelen met de AEM SPA Editor - Hello World-zelfstudie {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> Deze zelfstudie is **verouderd**. Het wordt aanbevolen om: [Begonnen het worden met de Redacteur van het AEMKUUROORD en Hoekig](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-angular-tutorial/overview.html) of [Begonnen met de Redacteur van het KUUROORD en Reageren](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-react-tutorial/overview.html)

AEM de Redacteur van het KUUROORD verleent steun voor in-context het uitgeven van Één enkele Toepassing of KUUROORD van de Pagina. Dit leerprogramma is een inleiding aan de ontwikkeling van het KUUROORD die met AEM Redacteur JS SDK van het KUUROORD moet worden gebruikt. De zelfstudie breidt de app We.Retail Journal uit door een aangepaste Hello World-component toe te voegen. Gebruikers kunnen de zelfstudie voltooien met Reageren of Hoekframes.

>[!NOTE]
>
> De eigenschap van de Redacteur van de Toepassing van de Enige-Pagina (SPA) vereist AEM 6.4 de dienstpak 2 of nieuwer.
>
> De redacteur van het KUUROORD is de geadviseerde oplossing voor projecten die het kader van het KUUROORD gebaseerde cliënt-kant teruggeven (b.v. Reageren of Hoekig) vereisen.

## Vereiste lezing {#prereq}

Dit leerprogramma is bedoeld om de stappen te benadrukken nodig om een component van het KUUROORD aan een AEM component in kaart te brengen om in-context het uitgeven toe te laten. Gebruikers die deze zelfstudie starten, moeten vertrouwd zijn met de basisconcepten van ontwikkeling met Adobe Experience Manager, AEM en zich ontwikkelen met React of Angular frameworks. De zelfstudie behandelt zowel back-end als front-end ontwikkelingstaken.

U wordt aangeraden de volgende bronnen te controleren voordat u deze zelfstudie start:

* [De Video](spa-editor-framework-feature-video-use.md) van de Eigenschap van de Redacteur van het KUUROORD - een videooverzicht van de Redacteur van het KUUROORD en de app van het Dagboek.
* [Zelfstudie](https://reactjs.org/tutorial/tutorial.html) React.js - Een inleiding tot ontwikkeling met het React-kader.
* [Hoekzelfstudie](https://angular.io/tutorial) - Een inleiding tot ontwikkeling met hoekige vormgeving

## Lokale ontwikkelomgeving {#local-dev}

Deze zelfstudie is ontworpen voor:

[Adobe Experience Manager 6.5](https://helpx.adobe.com/experience-manager/6-5/release-notes.html) of [Adobe Experience Manager 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [Service Pack 5](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)

In deze zelfstudie moeten de volgende technologieën en gereedschappen worden geïnstalleerd:

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) en npm 5.6.0+ (npm is geïnstalleerd met node.js)

Controleer de installatie van de bovenstaande gereedschappen door een nieuwe terminal te openen en het volgende uit te voeren:

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## Overzicht {#overview}

Het basisconcept moet een Component van het KUUROORD aan een AEMComponent in kaart brengen. AEM componenten, met serverfuncties, inhoud exporteren in de vorm van JSON. De inhoud JSON wordt verbruikt door het KUUROORD, lopend cliënt-kant in browser. Een afbeelding 1:1 tussen de componenten van het KUUROORD en een AEM wordt gecreeerd.

![SPA-componenttoewijzing](assets/spa-editor-helloworld-tutorial-use/mapto.png)

Populaire frameworks [React JS](https://reactjs.org/) en [Angular](https://angular.io/) worden uit de verpakking ondersteund. Gebruikers kunnen deze zelfstudie onder de hoek of Reageren voltooien, afhankelijk van welk framework ze het prettigst zijn.

## Projectinstelling {#project-setup}

De ontwikkeling van SBZ heeft één voet in AEM ontwikkeling, en andere uit. Het doel is de ontwikkeling van het KUUROORD toe te staan om onafhankelijk, en (meestal) agnostisch aan AEM voor te komen.

* De projecten van het KUUROORD kunnen onafhankelijk van het AEM project tijdens front-end ontwikkeling werken.
* Voor-kant bouwt hulpmiddelen en technologieën zoals Webpack, NPM, [!DNL Grunt] en [!DNL Gulp]blijft worden gebruikt.
* Om voor AEM te bouwen, wordt het project van het KUUROORD gecompileerd en automatisch inbegrepen in het AEM project.
* Standaard AEM Pakketten die worden gebruikt om het KUUROORD in AEM op te stellen.

![Overzicht van artefacten en implementatie](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*De ontwikkeling van SPA heeft één voet in AEM ontwikkeling, en andere - die de ontwikkeling van SPA onafhankelijk toestaat, en (meestal) agnostisch aan AEM.*

Het doel van deze zelfstudie is om de Web.Retail App met een nieuwe component uit te breiden. Begin door de broncode voor de app van het Dagboek te downloaden We.Retail en aan een lokale AEM op te stellen.

1. **Download** de recentste Code van het Dagboek [We.Retail van GitHub](https://github.com/adobe/aem-sample-we-retail-journal).

   U kunt de gegevensopslagruimte ook klonen vanaf de opdrachtregel:

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >Het leerprogramma zal tegen de **master** tak met versie **1.2.1-SNAPSHOT** van het project werken.

1. De volgende structuur moet zichtbaar zijn:

   ![Projectmapstructuur](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   Het project bevat de volgende gemaskeerde modules:

   * `all`: Hiermee sluit u het hele project in één pakket in en installeert u dit.
   * `bundles`: Bevat twee OSGi-bundels: komma&#39;s en core die [!DNL Sling Models] en andere Java-code bevatten.
   * `ui.apps`: bevat de /apps-onderdelen van het project, dat wil zeggen JS &amp; CSS-clientlibs, componenten, specifieke configuraties voor de runmode.
   * `ui.content`: bevat structurele inhoud en configuraties (`/content`, `/conf`)
   * `react-app`: Wij.Retail Journal React-toepassing. Dit is zowel een module Maven als een webpack project.
   * `angular-app`: Wij.Retail Journal-toepassing. Dit is zowel een [!DNL Maven] module als een webpack-project.

1. Open een nieuw terminalvenster en voer de volgende opdracht uit om de volledige app te maken en te implementeren in een lokale AEM-instantie die wordt uitgevoerd op [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > In dit project is het Maven-profiel voor het bouwen en verpakken van het gehele project `autoInstallSinglePackage`

   >[!CAUTION]
   >
   > Als u een fout tijdens het bouwen ontvangt, [zorg ervoor uw Maven.xml- dossier Adobe Maven monfact bewaarplaats](https://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html)omvat.

1. Ga naar:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   De Web.Retail Journal App moet worden weergegeven in de AEM Sites-editor.

1. Selecteer in de [!UICONTROL Edit] modus een component die u wilt bewerken en werk de inhoud bij.

   ![Een component bewerken](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. Selecteer het [!UICONTROL Page Properties] pictogram om het [!UICONTROL Page Properties]te openen. Selecteer deze optie [!UICONTROL Edit Template] om de paginasjabloon te openen.

   ![Menu Pagina-eigenschappen](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. In de recentste versie van de Redacteur van het KUUROORD, kunnen de [Bewerkbare malplaatjes](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) op de zelfde manier zoals met traditionele implementaties van Plaatsen worden gebruikt. Dit wordt later opnieuw bekeken met onze aangepaste component.

   >[!NOTE]
   >
   > Alleen AEM 6.5 en AEM 6.4 + **Service Pack 5** ondersteunen bewerkbare sjablonen.

## Overzicht van ontwikkeling {#development-overview}

![Overzichtsontwikkeling](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

De ontwikkelingsteriteraties van SPA komen onafhankelijk van AEM voor. Wanneer het KUUROORD klaar is om in AEM worden opgesteld vinden de volgende stappen op hoog niveau plaats (zoals hierboven geïllustreerd).

1. Het AEM project bouwt wordt aangehaald, dat beurtelings een bouwstijl van het project van het KUUROORD teweegbrengt. The We.Retail Journal gebruikt de [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. De [**aem-client-generator**](https://www.npmjs.com/package/aem-clientlib-generator) van het project van het KUUROORD sluit het gecompileerde KUUROORD als AEM Bibliotheek van de Cliënt in het AEM project in.
1. Het AEM project produceert een AEM pakket, met inbegrip van gecompileerde SPA, plus een andere ondersteunende AEM code.

## AEM maken {#aem-component}

**Persona: AEM Developer**

Eerst wordt een AEM gemaakt. De AEM component is verantwoordelijk voor het renderen van de JSON-eigenschappen die door de component React worden gelezen. De AEM component is ook verantwoordelijk voor het opgeven van een dialoogvenster voor alle bewerkbare eigenschappen van de component.

Gebruikend [!DNL Eclipse], of andere [!DNL IDE], voer het Web.Retail Journal Maven project in.

1. Werk de reactor **pom.xml** bij om de [!DNL Apache Rat] insteekmodule te verwijderen. Met deze insteekmodule wordt elk bestand gecontroleerd om er zeker van te zijn dat er een licentieheader is. Voor onze doeleinden hoeven we ons niet bezig te houden met deze functionaliteit.

   Verwijder in **aem-sample-we-retail-journal/pom.xml** de **apache-rate-plug-in**:

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. In de **wij-kleinhandel-dagboek-inhoud** (`<src>/aem-sample-we-retail-journal/ui.apps`) module creeert een nieuw knooppunt onder `ui.apps/jcr_root/apps/we-retail-journal/components` genoemd **helloworld** van type **cq:Component**.
1. Voeg de volgende eigenschappen aan de **helloworld** component toe, die in hieronder XML (`/helloworld/.content.xml`) wordt vertegenwoordigd:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Hello World-component](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > Om de Bewerkbare eigenschap van Malplaatjes te illustreren hebben wij opzettelijk geplaatst `componentGroup="Custom Components"`. In een echt project, is het best om het aantal componentengroepen te minimaliseren, zodat zou een betere groep &quot;[!DNL We.Retail Journal]&quot;zijn om de andere inhoudcomponenten aan te passen.
   >
   > Alleen AEM 6.5 en AEM 6.4 + **Service Pack 5** ondersteunen bewerkbare sjablonen.

1. Vervolgens wordt een dialoogvenster gemaakt waarin een aangepast bericht kan worden geconfigureerd voor de **Hello World** -component. Voeg hieronder `/apps/we-retail-journal/components/helloworld` een knooppuntnaam **cq:dialog** van **nt:unstructure** toe.
1. In het dialoogvenster **cq:dialog** wordt één tekstveld weergegeven waarin de tekst doorloopt in een eigenschap met de naam **[!DNL message]**. Onder nieuw gemaakte **cq:dialog** voegt u de volgende knooppunten en eigenschappen toe, weergegeven in XML hieronder (`helloworld/_cq_dialog/.content.xml`):

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![bestandsstructuur](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   Met de bovenstaande XML-knooppuntdefinitie wordt een dialoogvenster gemaakt met één tekstveld waarmee een gebruiker een &quot;bericht&quot; kan invoeren. Noteer de eigenschap `name="./message"` in het `<message />` knooppunt. Dit is de naam van de eigenschap die wordt opgeslagen in het JCR binnen AEM.

1. Vervolgens wordt een leeg beleidsdialoogvenster gemaakt (`cq:design_dialog`). Het dialoogvenster Beleid is nodig om de component weer te geven in de Sjablooneditor. Voor dit eenvoudige gebruiksgeval wordt het een leeg dialoogvenster.

   Voeg hieronder `/apps/we-retail-journal/components/helloworld` een knooppuntnaam `cq:design_dialog` van toe `nt:unstructured`.

   De configuratie wordt hieronder weergegeven in XML (`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. Stel de codebasis aan AEM van de bevellijn op:

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   In [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) bevestig de component is opgesteld door de omslag onder te inspecteren `/apps/we-retail-journal/components:`

   ![Ingevoerde componentenstructuur in CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Verkoopmodel maken {#create-sling-model}

**Persona: AEM Developer**

Vervolgens [!DNL Sling Model] wordt een back-up van de [!DNL Hello World] component gemaakt. In een traditioneel WCM gebruiksgeval [!DNL Sling Model] voert de implementatie om het even welke bedrijfslogica uit en een server-kant teruggevend manuscript (HTL) zal een vraag aan het [!DNL Sling Model]maken. Hierdoor blijft het renderscript relatief eenvoudig.

[!DNL Sling Models] worden ook gebruikt in het het gebruiksgeval van het KUUROORD om server-kant bedrijfslogica uit te voeren. Het verschil is dat in het [!DNL SPA] gebruiksgeval, het methodes als geserialiseerde JSON [!DNL Sling Models] blootstelt.

>[!NOTE]
>
>Als beste praktijken, zouden de ontwikkelaars moeten kijken om [AEM de Componenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) van de Kern te gebruiken waar mogelijk. Onder andere, verstrekken de Componenten van de Kern [!DNL Sling Models] van output JSON die &quot;SPA-klaar&quot;is, toestaand ontwikkelaars om zich meer op front-end presentatie te concentreren.

1. In de redacteur van uw keus, open het **wij-detailhandel-dagboek-gemeenschappelijke** project ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. In de verpakking `com.adobe.cq.sample.spa.commons.impl.models`:
   * Maak een nieuwe klasse met de naam `HelloWorld`.
   * Voeg een implementerende interface toe voor `com.adobe.cq.export.json.ComponentExporter.`

   ![Nieuwe wizard Java-klasse](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   De `ComponentExporter` interface moet worden uitgevoerd opdat de [!DNL Sling Model] interface compatibel is met AEM Content Services.

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. Voeg een statische variabele toe genoemd `RESOURCE_TYPE` om het middeltype van de [!DNL HelloWorld] component te identificeren:

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. Voeg de OSGi-annotaties toe voor `@Model` en `@Exporter`. De `@Model` aantekening registreert de klasse als een [!DNL Sling Model]. De `@Exporter` aantekening zal de methodes als geserialiseerde JSON gebruikend het [!DNL Jackson Exporter] kader blootstellen.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. Implementeer de methode `getDisplayMessage()` om de eigenschap JCR te retourneren `message`. Gebruik de [!DNL Sling Model] annotatie van `@ValueMapValue` om het gemakkelijk te maken om het bezit terug te winnen dat onder de component wordt `message` opgeslagen. De `@Optional` `message` aantekening is belangrijk omdat de component niet wordt gevuld wanneer deze voor het eerst aan de pagina wordt toegevoegd.

   Als deel van de bedrijfslogica, zal een koord, &quot;**Hello**&quot;, aan het bericht worden toegevoegd.

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > De methodenaam `getDisplayMessage` is belangrijk. Wanneer het [!DNL Sling Model] [!DNL Jackson Exporter] met in series wordt vervaardigd zal het als bezit worden blootgesteld JSON: `displayMessage`. Het [!DNL Jackson Exporter] `getter` zal alle methodes in series vervaardigen en blootstellen die geen parameter (tenzij uitdrukkelijk duidelijk maken om te negeren) nemen. Later in de app React / Angular lezen we deze eigenschapswaarde en geven deze weer als onderdeel van de toepassing.

   De methode `getExportedType` is ook belangrijk. De waarde van de component `resourceType` wordt gebruikt om de JSON-gegevens toe te wijzen aan de front-end component (Angular / React). We zullen dit in de volgende sectie onderzoeken.

1. Voer de methode uit `getExportedType()` om het middeltype van de `HelloWorld` component terug te keren.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   Klik hier voor de volledige code voor [**HelloWorld.java** .](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Implementeer de code om te AEM met Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   Verifieer de plaatsing en de registratie van het [!DNL Sling Model] door aan [[!UICONTROL Status] > [!UICONTROL Sling Models]](http://localhost:4502/system/console/status-slingmodels) in de console te navigeren OSGi.

   U zou moeten zien dat het Model van het `HelloWorld` Verkopen aan het het Verdelen middeltype gebonden is en dat het als `we-retail-journal/components/helloworld` [!DNL Sling Model Exporter Servlet]: wordt geregistreerd

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## React-component maken {#react-component}

**Persona: Front End Developer**

Vervolgens wordt de component React gemaakt. Open de module voor **reageren-app** ( `<src>/aem-sample-we-retail-journal/react-app`) met de editor van uw keuze.

>[!NOTE]
>
> U kunt deze sectie overslaan als u alleen geïnteresseerd bent in [hoekontwikkeling](#angular-component).

1. Navigeer in de `react-app` map naar de bijbehorende map src. Vouw de componentenmap uit om de bestaande React-componentbestanden weer te geven.

   ![structuur van componentbestand reageren](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. Voeg een nieuw bestand toe onder de map components met de naam `HelloWorld.js`.
1. Open `HelloWorld.js`. Voeg een importinstructie toe om de componentenbibliotheek React te importeren. Voeg een tweede importinstructie toe om de `MapTo` helper die door Adobe wordt geleverd, te importeren. De `MapTo` helper verstrekt een afbeelding van de React component aan JSON van de AEM component.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. Onder de importbewerkingen maakt u een nieuwe klasse met de naam `HelloWorld` die de React- `Component` interface uitbreidt. Voeg de vereiste `render()` methode toe aan de `HelloWorld` klasse.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. De `MapTo` hulplijn bevat automatisch een object dat is genoemd `cqModel` als onderdeel van de props van de component React. Het `cqModel` omvat alle eigenschappen die door het [!DNL Sling Model]worden blootgesteld.

   Onthoud dat de eerder [!DNL Sling Model] gemaakte methode een methode bevat `getDisplayMessage()`. `getDisplayMessage()` wordt vertaald als een JSON-toets met de naam `displayMessage` bij uitvoer.

   Implementeer de `render()` methode om een `h1` tag uit te voeren die de waarde van `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html), een syntaxisextensie voor JavaScript, wordt gebruikt om de definitieve opmaak van de component te retourneren.

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. Voer een Edit configuratiemethode uit. Deze methode wordt doorgegeven via de `MapTo` hulplijn en verschaft de AEM editor informatie om een tijdelijke aanduiding weer te geven als de component leeg is. Dit komt voor wanneer de component aan SPA wordt toegevoegd maar nog niet is authored. Voeg het volgende toe onder de `HelloWorld` klasse:

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. Roep aan het einde van het bestand de `MapTo` hulplijn aan en geef de `HelloWorld` klasse en de `HelloWorldEditConfig`klasse door. Hierdoor wordt de React Component toegewezen aan de AEM component op basis van het brontype van de AEM Component: `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   De voltooide code voor [**HelloWorld.js** is hier te vinden.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Open the file `ImportComponents.js`. Het is te vinden op `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   Voeg een regel toe om de regel `HelloWorld.js` met de andere componenten in de gecompileerde JavaScript-bundel te vereisen:

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. Maak in de `components` map een nieuw bestand met de naam `HelloWorld.css` op hetzelfde niveau `HelloWorld.js.` Vul het bestand met de volgende code in om een basisstijl voor de `HelloWorld` component te maken:

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Open de importinstructies opnieuw `HelloWorld.js` en werk deze bij om `HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. Implementeer de code om te AEM met Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. In [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) open `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`. U kunt snel zoeken naar HelloWorld in app.js om te controleren of de component React is opgenomen in de gecompileerde app.

   >[!NOTE]
   >
   > **app.js** is de gebundelde React-app. De code is niet meer leesbaar voor mensen. De `npm run build` opdracht heeft een geoptimaliseerde build geactiveerd die gecompileerde JavaScript-code uitvoert die door moderne browsers kan worden geïnterpreteerd.


## Hoekcomponent maken {#angular-component}

**Persona: Front End Developer**

>[!NOTE]
>
> U kunt deze sectie overslaan als u alleen geïnteresseerd bent in React-ontwikkeling.

Vervolgens wordt de hoekcomponent gemaakt. Open de **hoekige app** module (`<src>/aem-sample-we-retail-journal/angular-app`) met de editor van uw keuze.

1. Navigeer in de `angular-app` map naar de bijbehorende `src` map. Breid de componentenomslag uit om de bestaande Hoekcomponentendossiers te bekijken.

   ![Hoekbestandsstructuur](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. Voeg een nieuwe map toe onder de map components met de naam `helloworld`. Voeg onder de `helloworld` map nieuwe bestanden toe met de naam `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. Open `helloworld.component.ts`. Voeg een importinstructie toe om de hoekige `Component` en `Input` klassen te importeren. Maak een nieuwe component, waarbij u de component `styleUrls` en `templateUrl` naar `helloworld.component.css` en `helloworld.component.html`wijst. Exporteer ten slotte de klasse `HelloWorldComponent` met de verwachte invoer van `displayMessage`.

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > Als je de eerder [!DNL Sling Model] gemaakte methode terloopt, was er een methode **getDisplayMessage()**. De geserialiseerde JSON van deze methode is **displayMessage**, die we nu in de hoekige app lezen.

1. Openen `helloworld.component.html` om een `h1` tag op te nemen waarmee de `displayMessage` eigenschap wordt afgedrukt:

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. Bijwerken `helloworld.component.css` om enkele basisstijlen voor de component op te nemen.

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Werk dit bij `helloworld.component.spec.ts` met de volgende testbank:

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. Volgende update `src/components/mapping.ts` om de `HelloWorldComponent`code op te nemen. Voeg een `HelloWorldEditConfig` die placeholder in de AEM redacteur zal merken alvorens de component is gevormd. Voeg ten slotte een lijn toe om de AEM aan de hoekcomponent met de `MapTo` helper toe te wijzen.

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   Klik hier voor de volledige code voor [**mapping.ts** .](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. Update `src/app.module.ts` om de **NgModule** bij te werken. Voeg **`HelloWorldComponent`** als **verklaring** toe die tot **AppModule** behoort. Voeg ook de code `HelloWorldComponent` als een **entryComponent** toe, zodat deze wordt gecompileerd en dynamisch wordt opgenomen in de app wanneer het JSON-model wordt verwerkt.

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   De voltooide code voor [**app.module.ts** vindt u hier.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Implementeer de code die u wilt AEM met Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. In [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) open `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. Voer snel een onderzoek naar **HelloWorld** in uit `main.js` om de Hoekcomponent te verifiëren is inbegrepen.

   >[!NOTE]
   >
   > **main.js** is de gebundelde hoekige app. De code is niet meer leesbaar voor mensen. De npm run build-opdracht heeft een geoptimaliseerde build geactiveerd die gecompileerde JavaScript uitvoert die door moderne browsers kan worden geïnterpreteerd.

## De sjabloon bijwerken {#template-update}

1. Navigeer naar de bewerkbare sjabloon voor de reactieversie en/of hoekversie:

   * (Hoekig) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. Selecteer het hoofdmenu [!UICONTROL Layout Container] en selecteer het [!UICONTROL Policy] pictogram om het bijbehorende beleid te openen:

   ![Layoutbeleid selecteren](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   Voer onder **[!UICONTROL Properties]** > **[!UICONTROL Allowed Components]** een zoekopdracht uit naar **[!DNL Custom Components]**. U moet de **[!DNL Hello World]** component zien, selecteren. Sla de wijzigingen op door op het selectievakje rechtsboven in het scherm te klikken.

   ![Layout Container, beleidsconfiguratie](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. Nadat u het bestand hebt opgeslagen, ziet u de **[!DNL HelloWorld]** component als een toegestane component in de [!UICONTROL Layout Container]component.

   ![Toegestane componenten bijgewerkt](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > Slechts steunen AEM 6.5 en AEM 6.4.5 de Editable eigenschap van het Malplaatje van de Redacteur van het KUUROORD. Als het gebruiken van AEM 6.4, zult u het beleid voor Toegestane Componenten via CRXDE Lite manueel moeten vormen: `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` of `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite die de bijgewerkte beleidsconfiguraties voor [!UICONTROL Allowed Components] in [!UICONTROL Layout Container]: toont

   ![CRXDE Lite die de bijgewerkte beleidsconfiguraties voor Toegestane Componenten in de Container van de Lay-out toont](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## Alles samenvoegen {#putting-together}

1. Navigeer naar de pagina&#39;s Hoekig of Reageren:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. Zoek de **[!DNL Hello World]** component en sleep de **[!DNL Hello World]** component naar de pagina.

   ![hello world drag + drop](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   De tijdelijke aanduiding moet worden weergegeven.

   ![Plaatsaanduiding voor Hello World](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. Selecteer de component en voeg een bericht toe in het dialoogvenster, namelijk &quot;Wereld&quot; of &quot;Uw naam&quot;. Sla de wijzigingen op.

   ![gerenderde component](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   De tekenreeks &#39;Hello&#39; wordt altijd aan het bericht toegevoegd. Dit is het resultaat van de logica in het `HelloWorld.java` document [!DNL Sling Model].

## Volgende stappen {#next-steps}

[Voltooide oplossing voor de component HelloWorld](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* Volledige broncode voor [[!DNL We.Retail Journal] op GitHub](https://github.com/adobe/aem-sample-we-retail-journal)
* Ontdek een meer diepgaande zelfstudie over het ontwikkelen van Reactie met [[!DNL Aan de slag met de AEM SPA Editor - WKND-zelfstudie]](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## Problemen oplossen {#troubleshooting}

### Kan project niet maken in Eclipse {#unable-to-build-project-in-eclipse}

**Fout:** Een fout bij het importeren van het [!DNL We.Retail Journal] project in Eclipse voor niet-herkende doeluitvoeringen:

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![wizard eclipfout](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**Resolutie**: Klik op Voltooien om deze later op te lossen. Dit mag de voltooiing van de zelfstudie niet in de weg staan.

**Fout**: De React module, `react-app`, bouwt niet met succes tijdens een Maven bouwt.

**Resolutie:** Probeer de `node_modules` map onder de **reactie-app** te verwijderen. Voer de opdracht Apache Maven opnieuw uit vanaf de `mvn  clean install -PautoInstallSinglePackage` basis van het project.

### Ontevreden afhankelijkheden in AEM {#unsatisfied-dependencies-in-aem}

![Afhankelijkheidsfout pakketbeheer](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

Als een AEM gebiedsdeel niet wordt tevredengesteld, in of de **[!UICONTROL AEM Package Manager]** of in **[!UICONTROL AEM Web Console]** (de Console van Felix), wijst dit erop dat de Eigenschap van de Redacteur van het KUUROORD niet beschikbaar is.

### Component wordt niet weergegeven

**Fout**: Zelfs na een geslaagde implementatie en het controleren of de gecompileerde versies van React/Hoekige apps de bijgewerkte `helloworld` component hebben, wordt mijn component niet weergegeven wanneer ik het naar de pagina sleep. Ik kan de component in AEM UI zien.

**Resolutie**: Wis de browsergeschiedenis/cache en/of open een nieuwe browser of gebruik de incognitomodus. Als dat niet werkt, maakt u de cache van de clientbibliotheek op de lokale AEM ongeldig. AEM probeert grote clientbibliotheken in cache te plaatsen om efficiënt te zijn. Soms is het handmatig ongeldig maken van de cache nodig om problemen op te lossen waarbij verouderde code in de cache wordt geplaatst.

Navigeren naar: [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) en klik op Cache ongeldig maken. Ga terug naar de pagina React/Angular en vernieuw de pagina.

![Client-bibliotheek opnieuw samenstellen](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
