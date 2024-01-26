---
title: Aangepaste component
description: Omvat de verwezenlijking van begin tot eind van een component van de douanebylijn die authored inhoud toont. Omvat het ontwikkelen van een het Verschuiven Model om bedrijfslogica in te kapselen om de bylinecomponent en overeenkomstige HTML te bevolken om de component terug te geven.
version: 6.5, Cloud Service
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4072
mini-toc-levels: 1
thumbnail: 30181.jpg
doc-type: Tutorial
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
duration: 1209
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '3869'
ht-degree: 0%

---

# Aangepaste component {#custom-component}

In deze zelfstudie wordt uitgelegd hoe u een aangepaste versie van end to end kunt maken `Byline` AEM Component die inhoud toont authored in een Dialoog, en verkent het ontwikkelen van een het Verkopen Model om bedrijfslogica in te kapselen die HTML van de component bevolkt.

## Vereisten {#prerequisites}

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment).

### Starter-project

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide, kunt u het project opnieuw gebruiken en de stappen overslaan voor het uitchecken van het starterproject.

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Kijk uit de `tutorial/custom-component-start` vertakking van [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
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

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) of controleer de code plaatselijk door aan de tak over te schakelen `tutorial/custom-component-solution`.

## Doelstelling

1. Begrijp hoe te om een douane AEM component te bouwen
1. Leer om bedrijfslogica met het Verkopen Modellen in te kapselen
1. Begrijp hoe te om een het Verdelen Model van binnen een Manuscript van HTML te gebruiken

## Wat u gaat bouwen {#what-build}

In dit deel van de WKND-zelfstudie wordt een Byline-component gemaakt die wordt gebruikt om geschreven informatie over de auteur van een artikel weer te geven.

![voorbeeld van byline-component](assets/custom-component/byline-design.png)

*Byline-component*

De implementatie van de component Byline bevat een dialoogvenster waarin de inhoud van de bylijn wordt verzameld en een aangepast Sling-model waarin de details als volgt worden opgehaald:

* Naam
* Afbeelding
* Beroepen

## Byline-component maken {#create-byline-component}

Maak eerst de knooppuntstructuur van de component Byline en definieer een dialoogvenster. Dit vertegenwoordigt de Component in AEM en bepaalt impliciet het middeltype van de component door zijn plaats in JCR.

In het dialoogvenster wordt de interface weergegeven waarmee auteurs van inhoud de interface kunnen bieden. Voor deze implementatie, de AEM WCM Core Component **Afbeelding** wordt gebruikt om het ontwerpen en het teruggeven van het beeld van de Byline te behandelen, zodat moet het als component worden geplaatst `sling:resourceSuperType`.

### Componentdefinitie maken {#create-component-definition}

1. In de **ui.apps** module, navigeren naar `/apps/wknd/components` en maak een map met de naam `byline`.
1. Binnen de `byline` map, voeg een bestand met de naam `.content.xml`

   ![dialoogvenster om knooppunt te maken](assets/custom-component/byline-node-creation.png)

1. Vul de `.content.xml` bestand met het volgende:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   Het bovenstaande XML-bestand bevat de definitie voor de component, inclusief de titel, beschrijving en groep. De `sling:resourceSuperType` punten naar `core/wcm/components/image/v2/image`, die [Component Core Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html).

### HTML-script maken {#create-the-htl-script}

1. Binnen de `byline` map, bestand toevoegen `byline.html`, die verantwoordelijk is voor de HTML-presentatie van de component. De naam van het bestand moet gelijk zijn aan de naam van de map, omdat dit het standaardscript wordt dat Sling gebruikt om dit brontype te renderen.

1. Voeg de volgende code toe aan de `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

De `byline.html` is [later opnieuw bekeken](#byline-htl), zodra het verkoopmodel is gemaakt. Met de huidige status van het HTML-bestand kan de component in een lege staat worden weergegeven in de Pagina-editor van AEM Sites wanneer deze naar de pagina wordt gesleept en neergezet.

### De definitie van het dialoogvenster maken {#create-the-dialog-definition}

Definieer vervolgens een dialoogvenster voor de component Byline met de volgende velden:

* **Naam**: een tekstveld met de naam van de contribuant.
* **Afbeelding**: een verwijzing naar het biobeeld van de contribuant.
* **Beroepen**: een lijst van beroepen die aan de contribuant worden toegeschreven. De beroepen moeten alfabetisch in oplopende volgorde (a tot en met z) worden gesorteerd.

1. Binnen de `byline` map, een map maken met de naam `_cq_dialog`.
1. Binnen de `byline/_cq_dialog`, voegt u een bestand met de naam `.content.xml`. Dit is de XML-definitie voor het dialoogvenster. Voeg de volgende XML toe:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
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
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
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

   In deze definities van dialoogknooppunten worden de [Samenvoeging van verkoopbronnen](https://sling.apache.org/documentation/bundles/resource-merger.html) om te bepalen welke dialooglusjes van worden geërft `sling:resourceSuperType` in dit geval de **Afbeeldingscomponent van kerncomponenten**.

   ![voltooid dialoogvenster voor byline](assets/custom-component/byline-dialog-created.png)

### Het dialoogvenster Beleid maken {#create-the-policy-dialog}

Na de zelfde benadering zoals met de verwezenlijking van de Dialoog, creeer een dialoog van het Beleid (die vroeger als Dialoog van het Ontwerp wordt bekend) om ongewenste gebieden in de configuratie van het Beleid te verbergen die van de component van het Beeld van de Componenten van de Kern wordt geërft.

1. Binnen de `byline` map, een map maken met de naam `_cq_design_dialog`.
1. Binnen de `byline/_cq_design_dialog`, maakt u een bestand met de naam `.content.xml`. Werk het bestand bij met het volgende: met de volgende XML. Het is het eenvoudigst om de `.content.xml` en kopieer/plak de onderstaande XML.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   De basis voor de voorgaande **Beleidsdialoogvenster** XML is verkregen uit de [Component Core Components](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   Zoals in de configuratie van de Dialoog, [Samenvoeging van verkoopbronnen](https://sling.apache.org/documentation/bundles/resource-merger.html) wordt gebruikt om irrelevante velden te verbergen die anders van het `sling:resourceSuperType`, zoals in de nodedefinities met `sling:hideResource="{Boolean}true"` eigenschap.

### De code implementeren {#deploy-the-code}

1. Wijzigingen synchroniseren in `ui.apps` met uw IDE of het gebruiken van uw Maven vaardigheden.

   ![Exporteren naar AEM byline-component van de server](assets/custom-component/export-byline-component-aem.png)

## De component aan een pagina toevoegen {#add-the-component-to-a-page}

Om dingen eenvoudig te houden en zich op AEM componentenontwikkeling te concentreren, laten wij de component Byline in zijn huidige staat aan een pagina van het Artikel toevoegen om te verifiëren `cq:Component` de definitie van de knoop is correct. Ook om te verifiëren dat de AEM de nieuwe componentendefinitie en de de dialoogwerken van de component voor creatie erkent.

### Een afbeelding toevoegen aan de AEM Assets

Eerst uploadt u een voorbeeldkop die u naar AEM Assets hebt genomen om de afbeelding te vullen met de component Byline.

1. Navigeer naar de map LA Skateparks in AEM Assets: [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. De scherpte uploaden voor  **[stapelroswells.jpg](assets/custom-component/stacey-roswells.jpg)** naar de map.

   ![Titel geüpload naar AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### Auteur van de component {#author-the-component}

Voeg vervolgens de component Byline toe aan een pagina in AEM. Omdat de component Byline aan de **WKND-siteproject - Inhoud** Component Group, via de `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` definitie, is deze automatisch beschikbaar voor elke **Container** wiens **Beleid** stelt de **WKND-siteproject - Inhoud** groep. De pagina is dus beschikbaar in de container voor lay-out van de artikelpagina.

1. Navigeer naar het artikel LA Skatepark op: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. Sleep vanuit de linkerzijbalk een **Byline-component** op **bottom** van de container met lay-out van de geopende artikelpagina.

   ![byline-component toevoegen aan pagina](assets/custom-component/add-to-page.png)

1. Zorg ervoor dat de linkerzijbalk is geopend **en zichtbaar zijn, en de** Asset Finder** is geselecteerd.

1. Selecteer de **Plaatsaanduiding van Byline-component**, die op zijn beurt de actiebalk weergeeft en op de knop **moersleutel** om het dialoogvenster te openen.

1. Open de linkerzijbalk terwijl het dialoogvenster geopend is en het eerste tabblad (element) actief is. Sleep een afbeelding vanuit de zoekfunctie naar de dropzone Afbeelding. Zoek naar &quot;stapey&quot; om Stacey Roswells biopicture te vinden die in het WKND ui.content pakket wordt verstrekt.

   ![Afbeelding toevoegen aan dialoogvenster](assets/custom-component/add-image.png)

1. Nadat u een afbeelding hebt toegevoegd, klikt u op de knop **Eigenschappen** om de **Naam** en **Beroepen**.

   Voer de namen in bij het betreden van beroepen **omgekeerd alfabetisch** bestellen zodat de alfabetiserende bedrijfslogica die in het Sling Model wordt uitgevoerd wordt geverifieerd.

   Tik op de knop **Gereed** in de rechterbenedenhoek om de wijzigingen op te slaan.

   ![eigenschappen van byline-component vullen](assets/custom-component/add-properties.png)

   AEM auteurs configureren componenten en schrijven ze samen via de dialoogvensters. Op dit punt, in de ontwikkeling van de component Byline zijn de dialogen inbegrepen voor het verzamelen van de gegevens, nochtans is de logica om de geschreven inhoud terug te geven nog niet toegevoegd. Daarom wordt alleen de tijdelijke aanduiding weergegeven.

1. Nadat u het dialoogvenster hebt opgeslagen, navigeert u naar [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) en bekijk hoe de inhoud van de component wordt opgeslagen op het knooppunt met inhoud van de bylinecomponent onder de AEM pagina.

   Zoek het inhoudknooppunt van de component Byline onder de pagina LA Skate Parks, dat wil zeggen `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   Let op de eigenschapsnamen `name`, `occupations`, en `fileReference` worden opgeslagen op de **byline-knooppunt**.

   Let ook op de `sling:resourceType` van het knooppunt is ingesteld op `wknd/components/content/byline` Dit is wat deze inhoudsknoop aan de de componentenimplementatie van de Byline bindt.

   ![byline-eigenschappen in CRXDE](assets/custom-component/byline-properties-crxde.png)

## Byline Sling Model maken {#create-sling-model}

Daarna, creëren wij een het Verdelen Model om als gegevensmodel te handelen en de bedrijfslogica voor de component van de Byline te huisvesten.

Sling-modellen zijn annotatiegestuurde Java™ POJO&#39;s (Plain Old Java™ Objects) die het gemakkelijker maken gegevens van de JCR aan Java™-variabelen toe te wijzen en die efficiëntie bieden bij het ontwikkelen in de AEM context.

### GeMaven afhankelijkheden controleren {#maven-dependency}

Het Byline Sling-model is gebaseerd op verschillende Java™ API&#39;s van AEM. Deze API&#39;s zijn beschikbaar via de `dependencies` in de lijst `core` POM-bestand van de module. Het project dat voor deze zelfstudie wordt gebruikt, is gemaakt voor AEM as a Cloud Service. Nochtans is het uniek aangezien het achterwaarts compatibel met AEM 6.5/6.4 is. Daarom zijn zowel gebiedsdelen voor Cloud Service als AEM 6.x inbegrepen.

1. Open de `pom.xml` bestand onder `<src>/aem-guides-wknd/core/pom.xml`.
1. Zoek de afhankelijkheid voor `aem-sdk-api` - **Alleen AEM as a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   De [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) bevat alle openbare Java™ API&#39;s die door AEM worden weergegeven. De `aem-sdk-api` wordt gebruikt door gebrek wanneer het bouwen van dit project. De versie blijft behouden in de hoofdreactorpom vanaf de basis van het project bij `aem-guides-wknd/pom.xml`.

1. Zoek de afhankelijkheid voor de `uber-jar` - **Alleen AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   De `uber-jar` wordt alleen opgenomen als de `classic` profiel wordt aangeroepen, d.w.z `mvn clean install -PautoInstallSinglePackage -Pclassic`. Nogmaals, dit is uniek voor dit project. In een echt project, dat uit de Archetype van het Project van de AEM wordt geproduceerd `uber-jar` Dit is de standaardinstelling als de opgegeven versie van AEM 6.5 of 6.4 is.

   De [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) bevat alle openbare Java™ API&#39;s die door AEM 6.x worden weergegeven. De versie blijft behouden in de hoofdreactorpom van het project `aem-guides-wknd/pom.xml`.

1. Zoek de afhankelijkheid voor `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   Dit zijn de volledige openbare Java™ API&#39;s die worden weergegeven door AEM Core Components. AEM Core Components is een project dat buiten AEM wordt onderhouden en heeft daarom een aparte releasecyclus. Daarom is het een afhankelijkheid die afzonderlijk moet worden opgenomen en die **niet** opgenomen in de `uber-jar` of `aem-sdk-api`.

   Net als de uber-jar blijft de versie voor deze afhankelijkheid behouden in het Parent-reactorpomabestand van `aem-guides-wknd/pom.xml`.

   Verderop in deze zelfstudie wordt de klasse Core Component Image gebruikt om de afbeelding weer te geven in de component Byline. Het is noodzakelijk om de afhankelijkheid van de Component van de Kern te hebben om het het Verkopen Model te bouwen en te compileren.

### Byline-interface {#byline-interface}

Maak een openbare Java™-interface voor de naamregel. De `Byline.java` bepaalt de openbare methodes nodig om de `byline.html` HTML-script.

1. Binnen, `core` binnen de `core/src/main/java/com/adobe/aem/guides/wknd/core/models` map maken een bestand met de naam `Byline.java`

   ![byline-interface maken](assets/custom-component/create-byline-interface.png)

1. Bijwerken `Byline.java` met de volgende methoden:

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   De eerste twee methoden maken de waarden voor de **name** en **beroepen** voor de component Byline.

   De `isEmpty()` wordt gebruikt om te bepalen als de component om het even welke inhoud heeft om terug te geven of als het wacht om worden gevormd.

   Er is geen methode voor de afbeelding. [deze wordt later herzien](#tackling-the-image-problem).

1. Java™-pakketten die openbare Java™-klassen, in dit geval een Sling Model, bevatten, moeten zijn voorzien van een versienummer via de  `package-info.java` bestand.

   Sinds het Java™-pakket van de WKND-bron `com.adobe.aem.guides.wknd.core.models` declareert versie van `1.0.0`en er wordt een vaste openbare interface en methoden toegevoegd, moet de versie worden uitgebreid tot `1.1.0`. Open het bestand op `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` en bijwerken `@Version("1.0.0")` tot `@Version("2.1.0")`.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

Wanneer de bestanden in dit pakket worden gewijzigd, wordt [pakketversie moet semantisch worden aangepast](https://semver.org/). Zo niet, dan is het Maven-project [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd) detecteert een ongeldige pakketversie en verbreekt de gebouwde versie. Gelukkig meldt de Maven-plug-in bij een storing de ongeldige Java™-pakketversie en de versie die deze moet hebben. Werk de `@Version("...")` declaratie in de schending van het Java™-pakket `package-info.java` naar de versie die door de plug-in wordt aanbevolen om te repareren.

### Bylineimplementatie {#byline-implementation}

De `BylineImpl.java` is de implementatie van het Sling Model dat het `Byline.java` eerder gedefinieerde interface. De volledige code voor `BylineImpl.java` vindt u onder aan deze sectie.

1. Een map maken met de naam `impl` beneide `core/src/main/java/com/adobe/aem/guides/core/models`.
1. In de `impl` map, een bestand maken `BylineImpl.java`.

   ![Byline-iml-bestand](assets/custom-component/byline-impl-file.png)

1. Openen `BylineImpl.java`. Opgeven dat het de `Byline` interface. Gebruik de auto-volledige eigenschappen van winde of werk manueel het dossier bij om de methodes te omvatten nodig om uit te voeren `Byline` interface:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. Annotaties van het verkoopmodel toevoegen door bijwerken `BylineImpl.java` met de volgende klasseniveau-annotaties. Dit `@Model(..)`annotatie is wat van de klasse een Sling Model maakt.

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   Laten we deze annotatie en de bijbehorende parameters bekijken:

   * De `@Model` annotatieregisters BylineImpl als Sling Model wanneer het aan AEM wordt opgesteld.
   * De `adaptables` parameter specificeert dat dit model door het verzoek kan worden aangepast.
   * De `adapters` parameter staat toe dat de implementatieklasse onder de interface Byline wordt geregistreerd. Hierdoor kan het HTML-script het Sling Model via de interface aanroepen (in plaats van de implementatie rechtstreeks). [Meer informatie over adapters vindt u hier](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * De `resourceType` verwijst naar het brontype van de component Byline (eerder gecreeerd) en helpt om het correcte model op te lossen als er veelvoudige implementaties zijn. [Meer details over het associëren van een modelklasse met een middeltype kunnen hier worden gevonden](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### Implementatie van de methoden van het verkoopmodel {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

De eerste methode die wordt geïmplementeerd, is `getName()`, wordt de opgeslagen waarde geretourneerd naar het JCR-inhoudsknooppunt van de byline onder de eigenschap `name`.

Hiervoor `@ValueMapValue` De het verkopen modelaantekening wordt gebruikt om de waarde in een gebied te injecteren Java™ gebruikend ValueMap van het middel van het Verzoek.


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

Omdat de JCR-eigenschap de naam deelt als het Java™-veld (beide zijn &quot;name&quot;), `@ValueMapValue` lost automatisch deze vereniging op en injecteert de waarde van het bezit in het gebied Java™.

#### getOccupations() {#implementing-get-occupations}

De volgende methode die moet worden geïmplementeerd is `getOccupations()`. Deze methode laadt de beroepen die in het bezit JCR worden opgeslagen `occupations` en retourneert een gesorteerde (alfabetische) verzameling ervan.

Dezelfde techniek gebruiken die in `getName()` De eigenschapswaarde kan in het veld Sling Model worden geïnjecteerd.

Zodra de JCR-eigenschapswaarden beschikbaar zijn in het Sling Model via het geïnjecteerde Java™ veld `occupations`, kan de sorteerbedrijfslogica worden toegepast in de `getOccupations()` methode.


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

De laatste methode public is `isEmpty()` dat bepaalt wanneer de component zich &quot;authored genoeg&quot;zou moeten overwegen om terug te geven.

Voor deze component zijn alle drie de gebieden het bedrijfsvereiste: `name, image and occupations` moet worden ingevuld *voor* de component kan worden gerenderd.


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### Het probleem van de &quot;afbeelding&quot; aanpakken {#tackling-the-image-problem}

Het controleren van de naam en de gebruiksomstandigheden zijn triviaal en de Apache Commons Lang3 geeft het handige [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) klasse. Het is echter niet duidelijk hoe **aanwezigheid van de afbeelding** kan worden gevalideerd, aangezien de component Core Components Image wordt gebruikt om de afbeelding te bedekken.

Er zijn twee manieren om dit aan te pakken:

Controleer of de `fileReference` De JCR-eigenschap wordt omgezet in een element. *OF* Converteer deze resource naar een Core Component Image Sling Model en zorg ervoor dat de `getSrc()` methode is niet leeg.

Laten we de **seconde** aanpak. De eerste aanpak is waarschijnlijk voldoende, maar in deze zelfstudie wordt deze laatste gebruikt om andere kenmerken van Sling Models te verkennen.

1. Maak een methode van het type private waarmee de afbeelding wordt opgehaald. Deze methode wordt privé gelaten omdat er geen behoefte is om het voorwerp van het Beeld in HTML zelf bloot te stellen, en het wordt slechts gebruikt om te drijven `isEmpty().`

   Voeg de volgende methode van het type private toe voor `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   Zoals hierboven vermeld, zijn er nog twee manieren om de **Afbeeldingsmodel**:

   De eerste toepassing van de `@Self` annotatie, om het huidige verzoek automatisch aan te passen aan de Component van de Kern `Image.class`

   De tweede functie gebruikt de [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) De dienst OSGi, die een handige dienst is, en helpt ons het Verdelen Modellen van andere types in code tot stand brengen Java™.

   Laten we de tweede aanpak gebruiken.

   >[!NOTE]
   >
   >In een real-world implementatie, benadering &quot;Één&quot;, gebruikend `@Self` heeft de voorkeur omdat het de eenvoudigere, elegantere oplossing is. In deze zelfstudie wordt de tweede aanpak gebruikt, omdat er meer facetten van Sling Models moeten worden verkend die handig zijn: complexere componenten!

   Omdat Sling Models Java™ POJO&#39;s zijn, en niet OSGi Services, zijn de gebruikelijke OSGi injectieannotaties `@Reference` **kan** worden gebruikt, in plaats daarvan bieden Sling Models een speciale **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** annotatie die vergelijkbare functionaliteit biedt.

1. Bijwerken `BylineImpl.java` om de `OSGiService` annotatie voor het injecteren van `ModelFactory`:

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   Met de `ModelFactory` Beschikbaar, kan een Model van de Verkoop van het Beeld van de Component van de Kern tot stand worden gebracht gebruikend:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   Nochtans, vereist deze methode zowel een verzoek als een middel, nog niet beschikbaar in het het Verdelen Model. Om deze te verkrijgen, worden meer verkoopmodelannotaties gebruikt!

   Om het huidige verzoek te krijgen **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** Er kan een annotatie worden gebruikt om de `adaptable` (gedefinieerd in het `@Model(..)` als `SlingHttpServletRequest.class`, in een Java™-klasseveld.

1. Voeg de **@Self** annotatie om de **SlingHttpServletRequest-verzoek**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   Herinneren, gebruiken `@Self Image image` om het Core Component Image Sling Model te injecteren was een optie hierboven - de `@Self` annotatie probeert het aanpasbare object te injecteren (in dit geval een SlingHttpServletRequest) en zich aan te passen aan het veldtype annotatie. Aangezien het model voor het instellen van de afbeelding van de kerncomponent kan worden aangepast aan SlingHttpServletRequest-objecten, zou dit hebben gewerkt en is het minder code dan meer verkennend `modelFactory` aanpak.

   Nu worden de variabelen die nodig zijn om het afbeeldingsmodel te instantiëren via de ModelFactory-API geïnjecteerd. Laten we Verkoopmodel gebruiken **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** aantekening om dit object te verkrijgen nadat het Sling-model is geïnstantieerd.

   `@PostConstruct` is ongelooflijk nuttig en handelt in een zelfde hoedanigheid als aannemer, echter, wordt het aangehaald nadat de klasse wordt geconcretiseerd en alle geannoteerde gebieden Java™ worden geïnjecteerd. overwegende dat andere annotaties van het Sling Model Java™-klassevelden (variabelen) annoteren, `@PostConstruct` annotaties toevoegen aan een void, parametermethode nul, doorgaans genaamd `init()` (maar kan een willekeurige naam hebben).

1. Toevoegen **@PostConstruct** methode:

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   Onthoud dat modellen voor verkoop zijn **NOT** OSGi Services, zodat is het veilig om klassenstaat te handhaven. Vaak `@PostConstruct` Hiermee wordt de klassestatus Sling Model afgeleid en ingesteld voor later gebruik, vergelijkbaar met wat een normale constructor doet.

   Als de `@PostConstruct` methode genereert een uitzondering, het Sling-model wordt niet geïnstantieerd en is null.

1. **getImage()** kan nu worden bijgewerkt om het afbeeldingsobject te retourneren.

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. Laten we teruggaan naar `isEmpty()` en voltooi de implementatie:

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   Noteer meerdere aanroepen van `getImage()` is geen probleem omdat de geïnitialiseerde `image` class variable en wordt niet aangeroepen `modelFactory.getModelFromWrappedRequest(...)` wat niet te duur is, maar het is de moeite waard om onnodig te bellen.

1. De definitieve `BylineImpl.java` zou als moeten kijken:


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## Naamregel HTL {#byline-htl}

In de `ui.apps` module, openen `/apps/wknd/components/byline/byline.html` die in de vroegere opstelling van de Component van de AEM werd gecreeerd.

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

Laten we bekijken wat dit HTML-script tot nu toe doet:

* De `placeholderTemplate` verwijst naar de tijdelijke aanduiding voor kerncomponenten, die wordt weergegeven wanneer de component niet volledig is geconfigureerd. In de AEM Sites Page Editor wordt dit weergegeven als een vak met de componenttitel, zoals hierboven gedefinieerd in het dialoogvenster `cq:Component`s  `jcr:title` eigenschap.

* De `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` laadt de `placeholderTemplate` hierboven gedefinieerd en doorgegeven in een booleaanse waarde (momenteel hard gecodeerd tot `false`) in de plaatsaanduidingssjabloon. Wanneer `isEmpty` is waar, geeft het placeholder malplaatje het grijze vakje terug, anders geeft het niets terug.

### Byline HTML bijwerken

1. Bijwerken **byline.html** met de volgende skelet-HTML-structuur:

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   Let op: de CSS-klassen volgen de [Benaming BEM-overeenkomst](https://getbem.com/naming/). Hoewel het gebruik van BEM-conventies niet verplicht is, wordt BEM aanbevolen omdat het wordt gebruikt in CSS-klassen voor kerncomponenten en doorgaans resulteert in schone, leesbare CSS-regels.

### Instantiëren van objecten van het Sling Model in HTML {#instantiating-sling-model-objects-in-htl}

De [Instructie blok gebruiken](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) wordt gebruikt om Sling Model-objecten te instantiëren in het HTML-script en deze toe te wijzen aan een HTML-variabele.

De `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` gebruikt de interface Byline (com.adobe.aem.guides.wknd.models.Byline) die door BylineImpl wordt uitgevoerd en past het huidige SlingHttpServletRequest aan het aan, en het resultaat wordt opgeslagen in een HTML veranderlijke naam byline ( `data-sly-use.<variable-name>`).

1. De buitenste versie bijwerken `div` de **Naamregel** Verkoopmodel via openbare interface:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Methoden van het verkoopmodel openen {#accessing-sling-model-methods}

HTML leent vanuit JSTL en gebruikt dezelfde verkorting van namen van Java™ getter-methoden.

Bijvoorbeeld, het aanhalen van het Model van het Sling van de Byline `getName()` kan worden ingekort tot `byline.name`, op dezelfde manier in plaats van `byline.isEmpty`kan worden ingekort tot `byline.empty`. Met volledige methodenamen, `byline.getName` of `byline.isEmpty`, werkt ook. Noteer de `()` worden nooit gebruikt om methoden in HTML aan te roepen (vergelijkbaar met JSTL).

Java™-methoden die een parameter vereisen **kan** worden gebruikt in HTML. Dit is door ontwerp om de logica in HTML eenvoudig te houden.

1. De naam van de Naamregel kan aan de component worden toegevoegd door de component aan te roepen `getName()` methode op het Byline Sling Model, of in HTML: `${byline.name}`.

   Werk de `h2` tag:

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### HTML-expressieopties gebruiken {#using-htl-expression-options}

[Opties voor HTL-expressies](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) fungeren als modifiers voor inhoud in HTML en variëren van datumnotatie tot i18n-vertaling. Expressies kunnen ook worden gebruikt om lijsten of arrays met waarden met elkaar te verbinden. Dit zijn de expressies die nodig zijn om de beroepen in een door komma&#39;s gescheiden indeling weer te geven.

Expressies worden toegevoegd via de `@` in de HTML-expressie.

1. Als u de lijst met beroepen wilt samenvoegen met &quot;, &quot;, wordt de volgende code gebruikt:

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### De tijdelijke aanduiding voorwaardelijk weergeven {#conditionally-displaying-the-placeholder}

De meeste HTML-scripts voor AEM Components gebruiken de **plaatsaanduidingsparadigma** auteurs een visuele aanwijzing geven **erop wijzen dat een component verkeerd is ontworpen en niet op AEM Publish wordt getoond**. De conventie die dit besluit moet aansturen, is een methode op het ondersteunende Sling Model van de component uit te voeren, in dit geval: `Byline.isEmpty()`.

De `isEmpty()` De methode wordt aangeroepen op het Byline Sling-model en het resultaat (of liever gezegd negatief, via de `!` operator) wordt opgeslagen in een HTML-variabele met de naam `hasContent`:

1. De buitenste versie bijwerken `div` om een genoemde variabele van HTML te bewaren `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   Het gebruik van `data-sly-test`, de HTL `test` blok is sleutel, plaatst het allebei een variabele van HTML en geeft/geeft niet het HTML element terug het is. Het is gebaseerd op het resultaat van de evaluatie van de HTL-expressie. Indien &quot;true&quot;, wordt het HTML-element weergegeven, anders wordt het niet gerenderd.

   Deze HTL-variabele `hasContent` kan nu opnieuw worden gebruikt om de tijdelijke aanduiding voorwaardelijk weer te geven of te verbergen.

1. Werk de voorwaardelijke vraag aan bij `placeholderTemplate` onder aan het bestand met de volgende gegevens:

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### De afbeelding weergeven met behulp van kerncomponenten {#using-the-core-components-image}

Het HTML-script voor `byline.html` is nu bijna voltooid en de afbeelding ontbreekt.

Als de `sling:resourceSuperType` wijst naar de component Image van de Component van de Kern aan auteur het beeld, kan de component van het Beeld van de Component van de Kern worden gebruikt om het beeld terug te geven.

Voor dit, laten wij het huidige bylinemiddel omvatten, maar dwingen het middeltype van de component van het Beeld van de Component van de Kern, gebruikend middeltype `core/wcm/components/image/v2/image`. Dit is een krachtig patroon voor hergebruik van componenten. Voor dit, HTML `data-sly-resource` wordt gebruikt.

1. Vervang de `div` met een klasse van `cmp-byline__image` met het volgende:

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   Dit `data-sly-resource`, inclusief de huidige bron via het relatieve pad `'.'`en forceert de opname van de huidige bron (of de bron van de bylineinhoud) met het resourcetype van `core/wcm/components/image/v2/image`.

   Het middeltype van de Component van de Kern wordt gebruikt direct, en niet via een volmacht, omdat dit een in-manuscriptgebruik is, en het nooit aan de inhoud voortgeduurd.

2. Voltooid `byline.html` hieronder:

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. Implementeer de basis van de code naar een lokale AEM-instantie. Aangezien er wijzigingen zijn aangebracht in `core` en `ui.apps` beide modules moeten worden ingezet .

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   Als u wilt implementeren naar AEM 6.5/6.4, roept u de `classic` profiel:

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > U kon het volledige project van de wortel ook bouwen gebruikend het Geweven profiel `autoInstallSinglePackage` maar hierdoor kunnen de wijzigingen in de inhoud op de pagina worden overschreven. Dit komt omdat de `ui.content/src/main/content/META-INF/vault/filter.xml` is gewijzigd voor de begincode van de zelfstudie om de bestaande AEM-inhoud volledig te overschrijven. In een echt scenario is dit geen kwestie.

### De naamloze component Byline bekijken {#reviewing-the-unstyled-byline-component}

1. Na het opstellen van de update, navigeer aan [Ultieme gids voor LA Skateparks](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) of waar u de component Byline eerder in het hoofdstuk hebt toegevoegd.

1. De **image**, **name**, en **beroepen** wordt nu weergegeven en er wordt een naamloze, maar werkende Byline-component weergegeven.

   ![bylinecomponent zonder stijl](assets/custom-component/unstyled.png)

### De registratie van het verkoopmodel bekijken {#reviewing-the-sling-model-registration}

De [AEM de statusweergave van Sling Models van de webconsole](http://localhost:4502/system/console/status-slingmodels) geeft alle geregistreerde verkoopmodellen in AEM weer. Het Byline Sling-model kan worden gevalideerd als geïnstalleerd en herkend door deze lijst te herzien.

Als de **BylineImpl** wordt niet weergegeven in deze lijst, is het waarschijnlijk een probleem met de annotaties van het verkoopmodel of het model is niet toegevoegd aan het juiste pakket (`com.adobe.aem.guides.wknd.core.models`) in het kernproject.

![Byline Sling Model geregistreerd](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## Byline-stijlen {#byline-styles}

Als u de component Byline wilt uitlijnen met het meegeleverde creatieve ontwerp, moet u deze stijl gebruiken. Dit wordt bereikt door het SCSS-bestand te gebruiken en het bestand in het dialoogvenster **ui.frontend** -module.

### Een standaardstijl toevoegen

Standaardstijlen toevoegen voor de component Byline.

1. Terugkeren naar de IDE en de **ui.frontend** project `/src/main/webpack/components`:
1. Een bestand met de naam `_byline.scss`.

   ![verkenner van byline-project](assets/custom-component/byline-style-project-explorer.png)

1. Voeg de Byline implementaties CSS (geschreven als SCSS) in toe `_byline.scss`:

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. Open een terminal en navigeer in de `ui.frontend` -module.
1. Start de `watch` proces met het volgende npm bevel:

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. Ga terug naar de browser en ga naar de [LA SkateParks, artikel](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). De bijgewerkte stijlen worden weergegeven in de component.

   ![voltooide byline-component](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > Mogelijk moet u de cache van de browser wissen om ervoor te zorgen dat CSS niet wordt weergegeven en moet u de pagina vernieuwen met de component Byline om de volledige stijl te verkrijgen.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt een volledig nieuw aangepast onderdeel gemaakt met Adobe Experience Manager!

### Volgende stappen {#next-steps}

Ga verder met de ontwikkeling van AEM Component door te verkennen hoe u JUnit-tests voor de Byline Java™-code schrijft om ervoor te zorgen dat alles op de juiste wijze is ontwikkeld en dat de geïmplementeerde bedrijfslogica juist en volledig is.

* [Eenheidstests of AEM-onderdelen schrijven](unit-testing.md)

De voltooide code weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van de it op `tutorial/custom-component-solution`.

1. Klonen met [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) opslagplaats.
1. Kijk uit de `tutorial/custom-component-solution` vertakking
