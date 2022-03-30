---
title: Ontwikkelen met de AEM SPA Editor - Hello World-zelfstudie
description: AEM SPA Editor biedt ondersteuning voor contextbewerkingen van een toepassing of SPA van één pagina. Deze zelfstudie is een inleiding op SPA ontwikkeling die moet worden gebruikt met AEM SPA Editor JS SDK. De zelfstudie breidt de app We.Retail Journal uit door een aangepaste Hello World-component toe te voegen. Gebruikers kunnen de zelfstudie voltooien met Reageren of Angulars.
version: 6.3, 6.4, 6.5
topic: SPA
feature: SPA Editor
role: Developer
level: Beginner
exl-id: e900301d-411c-4c02-8443-2a0fa56b65b5
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '3116'
ht-degree: 0%

---

# Ontwikkelen met de AEM SPA Editor - Hello World-zelfstudie {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> Deze zelfstudie is **verouderd**. Het wordt aanbevolen om: [Aan de slag met de AEM SPA Editor en Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/angular/overview.html) of [Aan de slag met de AEM SPA Editor en Reageren](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/spa-editor/react/overview.html)

AEM SPA Editor biedt ondersteuning voor contextbewerkingen van een toepassing of SPA van één pagina. Deze zelfstudie is een inleiding op SPA ontwikkeling die moet worden gebruikt met AEM SPA Editor JS SDK. De zelfstudie breidt de app We.Retail Journal uit door een aangepaste Hello World-component toe te voegen. Gebruikers kunnen de zelfstudie voltooien met Reageren of Angulars.

>[!NOTE]
>
> De eigenschap van de Redacteur van de Toepassing van de enig-Pagina (SPA) vereist AEM 6.4 de dienstpak 2 of nieuwer.
>
> De SPA Redacteur is de geadviseerde oplossing voor projecten die SPA kader gebaseerde cliënt-zijteruggeven (b.v. Reageren of Angular) vereisen.

## Vereiste lezing {#prereq}

Deze zelfstudie is bedoeld om de stappen te benadrukken die nodig zijn om een SPA component toe te wijzen aan een AEM component om in-context het uitgeven toe te laten. Gebruikers die deze zelfstudie starten, moeten vertrouwd zijn met de basisbeginselen van ontwikkeling met Adobe Experience Manager, AEM en met React of Angular frameworks. De zelfstudie behandelt zowel back-end als front-end ontwikkelingstaken.

U wordt aangeraden de volgende bronnen te controleren voordat u deze zelfstudie start:

* [Video over functies SPA Editor](spa-editor-framework-feature-video-use.md) - Een video-overzicht van de app SPA Editor en We.Retail Journal.
* [Zelfstudie React.js](https://reactjs.org/tutorial/tutorial.html) - Een inleiding tot ontwikkeling met het React-kader.
* [Zelfstudie angular](https://angular.io/tutorial) - Een inleiding tot ontwikkeling met Angular

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

Het basisconcept is om een SPA Component aan een AEM Component in kaart te brengen. AEM componenten, met serverfuncties, inhoud exporteren in de vorm van JSON. De JSON-inhoud wordt door de SPA verbruikt en wordt in de browser op de client uitgevoerd. Er wordt een 1:1-toewijzing gemaakt tussen SPA componenten en een AEM component.

![Componenttoewijzing SPA](assets/spa-editor-helloworld-tutorial-use/mapto.png)

Populaire frameworks [React JS](https://reactjs.org/) en [Angular](https://angular.io/) worden vanuit het vak ondersteund. Gebruikers kunnen deze zelfstudie voltooien in Angular of Reageren, afhankelijk van welk raamwerk ze het prettigst vinden.

## Projectinstelling {#project-setup}

SPA ontwikkeling heeft één voet in AEM ontwikkeling, en de andere. Het doel is om SPA ontwikkeling onafhankelijk te laten plaatsvinden, en (meestal) agnostisch aan AEM.

* SPA projecten kunnen onafhankelijk van het AEM project functioneren tijdens de ontwikkeling vooraf.
* Voor-end bouwstijlhulpmiddelen en technologieën zoals Webpack, NPM, [!DNL Grunt] en [!DNL Gulp]blijven gebruiken.
* Om voor AEM te bouwen, wordt het SPA project gecompileerd en automatisch inbegrepen in het AEM project.
* Standaard AEM Pakketten die worden gebruikt om de SPA in AEM op te stellen.

![Overzicht van artefacten en implementatie](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA ontwikkeling heeft een voet in AEM ontwikkeling en de andere een uitweg - zodat SPA ontwikkeling zelfstandig kan plaatsvinden en (meestal) onnodig aan AEM.*

Het doel van deze zelfstudie is om de Web.Retail App met een nieuwe component uit te breiden. Begin door de broncode voor de app van het Dagboek te downloaden We.Retail en aan een lokale AEM op te stellen.

1. **Downloaden** de meest recente [We.Retail Journal-code van GitHub](https://github.com/adobe/aem-sample-we-retail-journal).

   U kunt de gegevensopslagruimte ook klonen vanaf de opdrachtregel:

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >De zelfstudie werkt tegen de **master** vertakken met **1.2.1-MOMENTOPNAME** versie van het project.

1. De volgende structuur moet zichtbaar zijn:

   ![Projectmapstructuur](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   Het project bevat de volgende gemaskeerde modules:

   * `all`: Hiermee sluit u het hele project in één pakket in en installeert u dit.
   * `bundles`: Bevat twee OSGi-bundels: opmerkingen en kern die [!DNL Sling Models] en andere Java-code.
   * `ui.apps`: bevat de /apps-onderdelen van het project, dat wil zeggen JS &amp; CSS-clientlibs, componenten, specifieke configuraties voor de runmode.
   * `ui.content`: bevat structurele inhoud en configuraties (`/content`, `/conf`)
   * `react-app`: Wij.Retail Journal React-toepassing. Dit is zowel een module Maven als een webpack project.
   * `angular-app`: Wij.Retail Journal Angular application. Dit is allebei [!DNL Maven] en een webpack-project.

1. Open een nieuw terminalvenster en voer de volgende opdracht uit om de volledige app te maken en te implementeren in een lokale AEM-instantie die wordt uitgevoerd op [http://localhost:4502](http://localhost:4502).

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > In dit project is het Maven-profiel voor het bouwen en verpakken van het gehele project `autoInstallSinglePackage`

1. Ga naar:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   De Web.Retail Journal App moet worden weergegeven in de AEM Sites-editor.

1. In [!UICONTROL Edit] in de modus selecteert u de component die u wilt bewerken en werkt u de inhoud bij.

   ![Een component bewerken](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. Selecteer [!UICONTROL Page Properties] pictogram om het [!UICONTROL Page Properties]. Selecteren [!UICONTROL Edit Template] om de sjabloon van de pagina te openen.

   ![Menu Pagina-eigenschappen](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. In de meest recente versie van de SPA Editor, [Bewerkbare sjablonen](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) kan op de zelfde manier worden gebruikt zoals met traditionele implementaties van Plaatsen. Dit wordt later opnieuw bekeken met onze aangepaste component.

   >[!NOTE]
   >
   > Alleen AEM 6.5 en AEM 6.4 + **Service Pack 5** ondersteuning voor bewerkbare sjablonen.

## Overzicht van ontwikkeling {#development-overview}

![Overzichtsontwikkeling](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA ontwikkelherhalingen zijn onafhankelijk van AEM. Wanneer de SPA klaar is om te worden ingezet in AEM vinden de volgende stappen op hoog niveau plaats (zoals hierboven geïllustreerd).

1. Het AEM project bouwt wordt aangehaald, dat beurtelings een bouwstijl van het SPA project teweegbrengt. Het Wij.Retail Journal gebruikt de [**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin).
1. De SPA [**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) Hiermee sluit u de gecompileerde SPA in als een AEM Client Library in het AEM-project.
1. Het AEM project produceert een AEM pakket, met inbegrip van de gecompileerde SPA, plus andere ondersteunende AEM code.

## AEM maken {#aem-component}

**Persona: AEM Developer**

Eerst wordt een AEM gemaakt. De AEM component is verantwoordelijk voor het renderen van de JSON-eigenschappen die door de component React worden gelezen. De AEM component is ook verantwoordelijk voor het opgeven van een dialoogvenster voor alle bewerkbare eigenschappen van de component.

Gebruiken [!DNL Eclipse]of andere [!DNL IDE], importeert u het project We.Retail Journal Maven.

1. De reactor bijwerken **pom.xml** om de [!DNL Apache Rat] insteekmodule. Met deze insteekmodule wordt elk bestand gecontroleerd om er zeker van te zijn dat er een licentieheader is. Voor onze doeleinden hoeven we ons niet bezig te houden met deze functionaliteit.

   In **aem-sample-we-retail-journal/pom.xml** remove **apache-rate plug-in**:

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

1. In de **we-retail-journaal-content** (`<src>/aem-sample-we-retail-journal/ui.apps`) module een nieuw knooppunt onder maken `ui.apps/jcr_root/apps/we-retail-journal/components` benoemd **hellowereld** van het type **cq:Component**.
1. Voeg de volgende eigenschappen toe aan de **hellowereld** component, vertegenwoordigd in XML (`/helloworld/.content.xml`) hieronder:

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
   > Om de Bewerkbare eigenschap van Malplaatjes te illustreren hebben wij opzettelijk geplaatst `componentGroup="Custom Components"`. In een echt project, is het best om het aantal componentengroepen te minimaliseren, zodat zou een betere groep &quot;[!DNL We.Retail Journal]&quot; om overeen te komen met de andere inhoudscomponenten.
   >
   > Alleen AEM 6.5 en AEM 6.4 + **Service Pack 5** ondersteuning voor Bewerkbare sjablonen.

1. Daarna wordt een dialoogvenster gemaakt waarin een aangepast bericht kan worden geconfigureerd voor de **Hallo wereld** component. Beneath `/apps/we-retail-journal/components/helloworld` een knooppuntnaam toevoegen **cq:dialoogvenster** van **nt:ongestructureerd**.
1. De **cq:dialoogvenster** geeft één tekstveld weer waarin tekst doorloopt naar een eigenschap met de naam **[!DNL message]**. Onder het nieuwe gemaakte **cq:dialoogvenster** Voeg de volgende knooppunten en eigenschappen toe, weergegeven in XML hieronder (`helloworld/_cq_dialog/.content.xml`) :

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

   Met de bovenstaande XML-knooppuntdefinitie wordt een dialoogvenster gemaakt met één tekstveld waarmee een gebruiker een &quot;bericht&quot; kan invoeren. Let op de eigenschap `name="./message"` binnen de `<message />` knooppunt. Dit is de naam van de eigenschap die wordt opgeslagen in het JCR binnen AEM.

1. Vervolgens wordt een leeg beleidsdialoogvenster gemaakt (`cq:design_dialog`). Het dialoogvenster Beleid is nodig om de component weer te geven in de Sjablooneditor. Voor dit eenvoudige gebruiksgeval wordt het een leeg dialoogvenster.

   Beneath `/apps/we-retail-journal/components/helloworld` een knooppuntnaam toevoegen `cq:design_dialog` van `nt:unstructured`.

   De configuratie wordt hieronder in XML weergegeven (`helloworld/_cq_design_dialog/.content.xml`)

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

   In [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) Controleer of de component is geïmplementeerd door de map onder te inspecteren `/apps/we-retail-journal/components:`

   ![Ingevoerde componentenstructuur in CRXDE Lite](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Verkoopmodel maken {#create-sling-model}

**Persona: AEM Developer**

Volgende [!DNL Sling Model] is gemaakt om de [!DNL Hello World] component. In een traditioneel WCM-gebruiksgeval [!DNL Sling Model] implementeert elke bedrijfslogica en een rendering script (HTL) aan de serverzijde roept de [!DNL Sling Model]. Hierdoor blijft het renderscript relatief eenvoudig.

[!DNL Sling Models] worden ook gebruikt in het SPA gebruiksgeval om server-kant bedrijfslogica uit te voeren. Het verschil is dat in de [!DNL SPA] use case, de [!DNL Sling Models] blootstelt het methodes zoals geserialiseerde JSON.

>[!NOTE]
>
>Als beste praktijken, zouden de ontwikkelaars moeten proberen te gebruiken [AEM kerncomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) indien mogelijk. Core Components bieden onder andere [!DNL Sling Models] met JSON-uitvoer die &quot;SPA-klaar&quot; is, zodat ontwikkelaars zich meer kunnen richten op front-end presentaties.

1. In de redacteur van uw keus, open **wij-detailhandel-dagboek-gemeenschappelijke** project ( `<src>/aem-sample-we-retail-journal/bundles/commons`).
1. In het pakket `com.adobe.cq.sample.spa.commons.impl.models`:
   * Een nieuwe klasse maken met de naam `HelloWorld`.
   * Voeg een implementerende interface toe voor `com.adobe.cq.export.json.ComponentExporter.`

   ![Nieuwe wizard Java-klasse](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   De `ComponentExporter` de interface moet worden geïmplementeerd om [!DNL Sling Model] om compatibel te zijn met AEM Content Services.

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

1. Een statische variabele toevoegen met de naam `RESOURCE_TYPE` om de [!DNL HelloWorld] brontype van component:

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. Voeg de OSGi-annotaties toe voor `@Model` en `@Exporter`. De `@Model` annotatie registreert de klasse als een [!DNL Sling Model]. De `@Exporter` annotatie geeft de methoden weer als geserialiseerde JSON met behulp van de [!DNL Jackson Exporter] kader.

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

1. De methode implementeren `getDisplayMessage()` om de JCR-eigenschap te retourneren `message`. Gebruik de [!DNL Sling Model] aantekening `@ValueMapValue` om het gemakkelijk te maken om het bezit terug te winnen `message` opgeslagen onder de component. De `@Optional` annotatie is belangrijk omdat de component voor het eerst aan de pagina wordt toegevoegd.  `message`  wordt niet gevuld.

   Als onderdeel van de bedrijfslogica, een koord, &quot;**Hallo**&quot;, wordt aan het bericht toegevoegd.

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
   > De methodenaam `getDisplayMessage` is belangrijk. Wanneer de [!DNL Sling Model] is geserialiseerd met de [!DNL Jackson Exporter] het zal als bezit JSON worden blootgesteld: `displayMessage`. De [!DNL Jackson Exporter] zal serialiseren en allen blootstellen `getter` methoden die geen parameter gebruiken (tenzij expliciet gemarkeerd om te negeren). Later in de app React / Angular lezen we deze eigenschapswaarde en geven deze weer als onderdeel van de toepassing.

   De methode `getExportedType` is ook belangrijk. De waarde van de component `resourceType` worden gebruikt om de JSON-gegevens toe te wijzen aan de front-end component (Angular/React). We zullen dit in de volgende sectie onderzoeken.

1. De methode implementeren `getExportedType()` om het middeltype van terug te keren `HelloWorld` component.

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   De volledige code voor [**HelloWorld.java** is hier te vinden.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. Implementeer de code om te AEM met Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   Controleer de implementatie en registratie van de [!DNL Sling Model] door te navigeren naar [[!UICONTROL Status] > [!UICONTROL Sling Models]](http://localhost:4502/system/console/status-slingmodels) in de OSGi-console.

   U moet zien dat de `HelloWorld` Het verkoopmodel is gebonden aan de `we-retail-journal/components/helloworld` Het het middeltype van het verkopen en dat het als a wordt geregistreerd [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## React-component maken {#react-component}

**Persona: Front End Developer**

Vervolgens wordt de component React gemaakt. Open de **reageren-app** module ( `<src>/aem-sample-we-retail-journal/react-app`) met de editor van uw keuze.

>[!NOTE]
>
> Je kunt dit gedeelte vrij laten als je alleen geïnteresseerd bent in [Ontwikkeling van angulars](#angular-component).

1. Binnen de `react-app` naar de bijbehorende map src. Vouw de componentenmap uit om de bestaande React-componentbestanden weer te geven.

   ![structuur van componentbestand reageren](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. Een nieuw bestand toevoegen onder de map components met de naam `HelloWorld.js`.
1. Open `HelloWorld.js`. Voeg een importinstructie toe om de componentenbibliotheek React te importeren. Voeg een tweede importinstructie toe om het dialoogvenster `MapTo` helper verstrekt door Adobe. De `MapTo` helper verstrekt een afbeelding van de component React aan JSON van de AEM component.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. Onder de import wordt een nieuwe klasse gemaakt met de naam `HelloWorld` dat de Reactie uitbreidt `Component` interface. Voeg de vereiste `render()` aan de `HelloWorld` klasse.

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. De `MapTo` helper omvat automatisch een genoemd voorwerp `cqModel` als onderdeel van de props van de component React. De `cqModel` omvat alle eigenschappen die door [!DNL Sling Model].

   Onthoud de [!DNL Sling Model] gemaakt eerder bevat een methode `getDisplayMessage()`. `getDisplayMessage()` wordt vertaald als een JSON-sleutel met de naam `displayMessage` wanneer uitvoer.

   Implementeer de `render()` methode om een `h1` -tag die de waarde bevat van `displayMessage`. [JSX](https://reactjs.org/docs/introducing-jsx.html), een syntaxisextensie naar JavaScript, wordt gebruikt om de laatste opmaak van de component te retourneren.

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

1. Voer een Edit configuratiemethode uit. Deze methode wordt doorgegeven via de `MapTo` en verschaft de AEM editor informatie om een tijdelijke aanduiding weer te geven wanneer de component leeg is. Dit gebeurt wanneer de component aan de SPA wordt toegevoegd, maar nog niet is gemaakt. Voeg het volgende toe onder de `HelloWorld` klasse:

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

1. Aan het einde van het bestand roept u de `MapTo` helper, door de `HelloWorld` en de `HelloWorldEditConfig`. Hierdoor wordt de React Component toegewezen aan de AEM component op basis van het brontype van de AEM Component: `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   De ingevulde code voor [**HelloWorld.js** is hier te vinden.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Het bestand openen `ImportComponents.js`. U vindt deze op `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`.

   Voeg een lijn toe om te vereisen `HelloWorld.js` met de andere componenten in de gecompileerde JavaScript-bundel:

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. In de  `components`  map maken een nieuw bestand met de naam `HelloWorld.css` als een `HelloWorld.js.` Vul het bestand met het volgende om een basisstijl voor de `HelloWorld` component:

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. Opnieuw openen `HelloWorld.js` en onder de importinstructies bijwerken om `HelloWorld.css`:

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
   > **app.js** is de gebundelde React-app. De code is niet meer leesbaar voor mensen. De `npm run build` heeft een geoptimaliseerde build geactiveerd die gecompileerde JavaScript-gegevens genereert die door moderne browsers kunnen worden geïnterpreteerd.


## Angular-component maken {#angular-component}

**Persona: Front End Developer**

>[!NOTE]
>
> U kunt deze sectie overslaan als u alleen geïnteresseerd bent in React-ontwikkeling.

Vervolgens wordt de component Angular gemaakt. Open de **angular-app** module (`<src>/aem-sample-we-retail-journal/angular-app`) met de editor van uw keuze.

1. Binnen de `angular-app` map naar de map navigeren `src` map. Breid de componentenomslag uit om de bestaande de componentendossiers van de Angular te bekijken.

   ![Bestandsstructuur van angular](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. Een nieuwe map toevoegen onder de map components met de naam `helloworld`. Onder de `helloworld` map toevoegen, nieuwe bestanden met een naam toevoegen `helloworld.component.css, helloworld.component.html, helloworld.component.ts`.

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

1. Openen `helloworld.component.ts`. Een importinstructie toevoegen om de Angular te importeren `Component` en `Input` klassen. Maak een nieuwe component en wijs de `styleUrls` en `templateUrl` tot `helloworld.component.css` en `helloworld.component.html`. De klasse ten slotte exporteren `HelloWorldComponent` met de verwachte input van `displayMessage`.

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
   > Als u zich de [!DNL Sling Model] die eerder zijn gemaakt, was er een methode **getDisplayMessage()**. De geserialiseerde JSON van deze methode wordt **displayMessage**, die we nu in de app Angular lezen.

1. Openen `helloworld.component.html` om een `h1` -tag die de `displayMessage` eigenschap:

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

1. Bijwerken `helloworld.component.spec.ts` met de volgende testbank:

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

1. Volgende update `src/components/mapping.ts` de `HelloWorldComponent`. Voeg een `HelloWorldEditConfig` dat placeholder in de AEM redacteur zal merken alvorens de component is gevormd. Voeg ten slotte een lijn toe om de AEM component toe te wijzen aan de component Angular met de component `MapTo` helper.

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

   De volledige code voor [**mapping.ts** is hier te vinden.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. Bijwerken `src/app.module.ts` om de **NgModule**. Voeg de **`HelloWorldComponent`** als **declaratie** die toebehoort aan **AppModule**. Voeg ook de `HelloWorldComponent` als **entryComponent** zodat het wordt gecompileerd en dynamisch in de app wordt opgenomen wanneer het JSON-model wordt verwerkt.

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

   De ingevulde code voor [**app.module.ts** is hier te vinden.](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. Implementeer de code die u wilt AEM met Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. In [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) open `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`. Snel zoeken naar **HelloWorld** in `main.js` om te controleren of de component Angular is opgenomen.

   >[!NOTE]
   >
   > **main.js** is de gebundelde Angular-app. De code is niet meer leesbaar voor mensen. De npm run build-opdracht heeft een geoptimaliseerde build geactiveerd die gecompileerde JavaScript uitvoert die door moderne browsers kan worden geïnterpreteerd.

## De sjabloon bijwerken {#template-update}

1. Navigeer naar de bewerkbare sjabloon voor de reactieversie en/of Angular-versie:

   * (Angular) [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (Reageren) [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. Selecteer de hoofdmap [!UICONTROL Layout Container] en selecteert u de [!UICONTROL Policy] pictogram om het beleid te openen:

   ![Layoutbeleid selecteren](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   Onder **[!UICONTROL Properties]** > **[!UICONTROL Allowed Components]**, voert een zoekopdracht uit naar **[!DNL Custom Components]**. U moet de **[!DNL Hello World]** selecteert u deze. Sla de wijzigingen op door op het selectievakje rechtsboven in het scherm te klikken.

   ![Layout Container, beleidsconfiguratie](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. Nadat u het bestand hebt opgeslagen, ziet u de knop **[!DNL HelloWorld]** als een toegestane component in de [!UICONTROL Layout Container].

   ![Toegestane componenten bijgewerkt](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > Alleen AEM 6.5 en AEM 6.4.5 ondersteunen de functie Bewerkbare sjabloon van de SPA Editor. Als het gebruiken van AEM 6.4, zult u het beleid voor Toegestane Componenten via CRXDE Lite manueel moeten vormen: `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` of `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite die de bijgewerkte beleidsconfiguraties voor [!UICONTROL Allowed Components] in de [!UICONTROL Layout Container]:

   ![CRXDE Lite die de bijgewerkte beleidsconfiguraties voor Toegestane Componenten in de Container van de Lay-out toont](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## Alles samenvoegen {#putting-together}

1. Navigeer naar de pagina&#39;s Angular of Reageren:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. Zoek de **[!DNL Hello World]** en sleep de **[!DNL Hello World]** op de pagina.

   ![hello world drag + drop](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   De tijdelijke aanduiding moet worden weergegeven.

   ![Plaatsaanduiding voor Hello World](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. Selecteer de component en voeg een bericht toe in het dialoogvenster, namelijk &quot;Wereld&quot; of &quot;Uw naam&quot;. Sla de wijzigingen op.

   ![gerenderde component](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   De tekenreeks &#39;Hello&#39; wordt altijd aan het bericht toegevoegd. Dit is het resultaat van de logica in het `HelloWorld.java` [!DNL Sling Model].

## Volgende stappen {#next-steps}

[Voltooide oplossing voor de component HelloWorld](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* Volledige broncode voor [[!DNL We.Retail Journal] op GitHub](https://github.com/adobe/aem-sample-we-retail-journal)
* Ontdek een diepgaandere zelfstudie over het ontwikkelen van Reageren met [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## Problemen oplossen {#troubleshooting}

### Kan project niet maken in Eclipse {#unable-to-build-project-in-eclipse}

**Fout:** Een fout bij het importeren van de [!DNL We.Retail Journal] Project in Eclipse voor niet-herkende doeluitvoeringen:

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![wizard eclipfout](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**Resolutie**: Klik op Voltooien om deze later op te lossen. Dit mag de voltooiing van de zelfstudie niet in de weg staan.

**Fout**: De module React, `react-app`, kan niet worden gebouwd tijdens een Maven-build.

**Resolutie:** Probeer de `node_modules` map onder de **reageren-app**. De opdracht Apache Maven opnieuw uitvoeren `mvn  clean install -PautoInstallSinglePackage` uit de hoofdmap van het project.

### Ontevreden afhankelijkheden in AEM {#unsatisfied-dependencies-in-aem}

![Afhankelijkheidsfout pakketbeheer](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

Als niet aan een AEM wordt voldaan, wordt in de **[!UICONTROL AEM Package Manager]** of in de **[!UICONTROL AEM Web Console]** (Felix Console) geeft dit aan dat SPA Editor-functie niet beschikbaar is.

### Component wordt niet weergegeven

**Fout**: Zelfs na een geslaagde implementatie en nadat u hebt gecontroleerd of de gecompileerde versies van React/Angular-apps de bijgewerkte versie hebben `helloworld` wordt mijn component niet weergegeven wanneer ik deze naar de pagina versleep. Ik kan de component in AEM UI zien.

**Resolutie**: Wis de browsergeschiedenis/cache en/of open een nieuwe browser of gebruik de incognitomodus. Als dat niet werkt, maakt u de cache van de clientbibliotheek op de lokale AEM ongeldig. AEM probeert grote clientbibliotheken in cache te plaatsen om efficiënt te zijn. Soms is het handmatig ongeldig maken van de cache nodig om problemen op te lossen waarbij verouderde code in de cache wordt geplaatst.

Navigeren naar: [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) en klik op Cache ongeldig maken. Ga terug naar de pagina React/Angular en vernieuw de pagina.

![Client-bibliotheek opnieuw samenstellen](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
