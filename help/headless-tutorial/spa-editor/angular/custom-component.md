---
title: Een aangepaste component maken | Aan de slag met de AEM SPA Editor en Angular
description: Leer hoe u een aangepaste component maakt die wordt gebruikt met de AEM SPA Editor. Leer hoe u dialoogvensters met auteurs en Sling Models ontwikkelt om het JSON-model uit te breiden en een aangepaste component te vullen.
feature: SPA Editor
version: Cloud Service
jira: KT-5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
duration: 393
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# Een aangepaste component maken {#custom-component}

Leer hoe u een aangepaste component maakt die wordt gebruikt met de AEM SPA Editor. Leer hoe u dialoogvensters met auteurs en Sling Models ontwikkelt om het JSON-model uit te breiden en een aangepaste component te vullen.

## Doelstelling

1. Begrijp de rol van Sling Models in het manipuleren van JSON model API die door AEM wordt verstrekt.
2. Begrijp hoe u AEM componentdialoogvensters maakt.
3. Leer een **aangepast** AEM Component die met het SPA redacteurskader compatibel is.

## Wat u gaat maken

De focus van vorige hoofdstukken was het ontwikkelen van SPA componenten en het in kaart brengen ervan aan *bestaand* AEM Core Components. Dit hoofdstuk richt zich op het creëren en uitbreiden van *new* AEM componenten en bewerk het JSON-model dat AEM aanbiedt.

Een eenvoudig `Custom Component` illustreert de stappen nodig om een net-nieuwe AEM component tot stand te brengen.

![Bericht weergegeven in Kapitalen](assets/custom-component/message-displayed.png)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment).

### De code ophalen

1. Download het beginpunt voor deze zelfstudie via Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. Implementeer de basis van de code op een lokale AEM met Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Als u [AEM 6,x](overview.md#compatibility) voeg toe `classic` profiel:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. Het voltooide pakket installeren voor de traditionele [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest). De afbeeldingen van [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest) worden opnieuw gebruikt op de WKND-SPA. Het pakket kan worden geïnstalleerd met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Pakketbeheer installeren wknd.all](./assets/map-components/package-manager-wknd-all.png)

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) of controleer de code plaatselijk door aan de tak over te schakelen `Angular/custom-component-solution`.

## De AEM component definiëren

Een AEM component wordt gedefinieerd als een knooppunt en eigenschappen. In het project worden deze knooppunten en eigenschappen vertegenwoordigd als XML-bestanden in de `ui.apps` -module. Maak vervolgens de AEM component in het dialoogvenster `ui.apps` -module.

>[!NOTE]
>
> Een snelle herhaling op de [de grondbeginselen van AEM componenten kunnen nuttig zijn](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Open de `ui.apps` in IDE van uw keuze.
2. Navigeren naar `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` en maak een map met de naam `custom-component`.
3. Een bestand met de naam `.content.xml` onder de `custom-component` map. Vul de `custom-component/.content.xml` met het volgende:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![Aangepaste componentdefinitie maken](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identificeert dat dit knooppunt een AEM component is.

   `jcr:title` Dit is de waarde die wordt weergegeven voor auteurs van inhoud en de `componentGroup` bepaalt de groepering van componenten in auteursUI.

4. Onder de `custom-component` map, een andere map maken met de naam `_cq_dialog`.
5. Onder de `_cq_dialog` map maken een bestand met de naam `.content.xml` en vult deze met het volgende:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
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
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   ![Aangepaste componentdefinitie](assets/custom-component/dialog-custom-component-defintion.png)

   Het bovenstaande XML-bestand genereert een eenvoudig dialoogvenster voor het `Custom Component`. Het kritieke deel van het bestand is de binnenzijde `<message>` knooppunt. Dit dialoogvenster bevat een eenvoudig `textfield` benoemd `Message` en blijft de waarde van het tekstveld behouden in een eigenschap met de naam `message`.

   Naast de waarde van de optie `message` via het JSON-model.

   >[!NOTE]
   >
   > Je kunt nog veel meer bekijken [voorbeelden van dialoogvensters door de definities van de Component van de Kern te bekijken](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). U kunt ook aanvullende formuliervelden weergeven, zoals `select`, `textarea`, `pathfield`, beschikbaar onder `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Met een traditionele AEM [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) is doorgaans vereist. Aangezien de SPA de component rendert, is er geen HTML-script nodig.

## Het verkoopmodel maken

Sling-modellen zijn annotaties die worden aangedreven door Java™ &quot;POJO&#39;s&quot; (gewone oude Java™-objecten) en die het gemakkelijker maken gegevens van de JCR aan Java™-variabelen toe te wijzen. [Verkoopmodellen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) typisch functie om complexe server-kant bedrijfslogica voor AEM Componenten in te kapselen.

In de context van de Redacteur van de SPA, stelt het Verdelen Modellen de inhoud van een component door het model JSON door een eigenschap bloot gebruikend [Verkoopmodel exporteren](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html).

1. Open in de IDE van uw keuze de `core` -module. `CustomComponent.java` en `CustomComponentImpl.java` zijn al gemaakt en uitgestald als onderdeel van de hoofdstukstartcode.

   >[!NOTE]
   >
   > Als het gebruiken van winde van de Code van Visual Studio, kan het nuttig zijn om te installeren [extensies voor Java™](https://code.visualstudio.com/docs/java/extensions).

2. De Java™-interface openen `CustomComponent.java` om `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java-interface](assets/custom-component/custom-component-interface.png)

   Dit is de Java™ interface die door het het Verkopen Model wordt uitgevoerd.

3. Bijwerken `CustomComponent.java` zodat het de `ComponentExporter` interface:

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   De uitvoering van `ComponentExporter` -interface is een vereiste dat het Sling-model automatisch wordt opgenomen door de JSON-model-API.

   De `CustomComponent` interface bevat één methode getter `getMessage()`. Dit is de methode die de waarde van de auteurdialoog door het model JSON blootstelt. Alleen methoden getter met lege parameters `()` worden geëxporteerd in het JSON-model.

4. Openen `CustomComponentImpl.java` om `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   Dit is de uitvoering van de `CustomComponent` interface. De `@Model` Met aantekening wordt de Java™-klasse aangeduid als een Sling Model. De `@Exporter` Met aantekening kan de Java™-klasse via serienummering worden geserialiseerd en geëxporteerd via de Sling Model Exporter.

5. De variabele static bijwerken `RESOURCE_TYPE` naar de AEM-component `wknd-spa-angular/components/custom-component` gemaakt in de vorige oefening.

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   Het middeltype van de component is wat het Sling Model aan de AEM component bindt en uiteindelijk aan de component van de Angular in kaart brengt.

6. Voeg de `getExportedType()` aan de `CustomComponentImpl` klasse om het type van componentenmiddel terug te keren:

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   Deze methode is vereist bij de implementatie van de `ComponentExporter` en stelt het middeltype bloot dat de afbeelding aan de component van de Angular toestaat.

7. Werk de `getMessage()` methode om de waarde van de `message` eigenschap is door het dialoogvenster van de auteur blijvend. Gebruik de `@ValueMap` annotatie wordt toegewezen aan de JCR-waarde `message` aan een Java™-variabele:

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   Er worden extra &#39;bedrijfslogica&#39; toegevoegd om de waarde van het bericht als hoofdletter te retourneren. Hierdoor kunnen we het verschil zien tussen de onbewerkte waarde die is opgeslagen door het dialoogvenster van de auteur en de waarde die wordt weergegeven door het model Sling.

   >[!NOTE]
   >
   U kunt de [voltooid CustomComponentImpl.java hier](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## De component Angular bijwerken

De code van de Angular voor de Component van de Douane is reeds gecreeerd. Voer vervolgens een aantal updates uit om de component Angular toe te wijzen aan de AEM.

1. In de `ui.frontend` -module het bestand openen `ui.frontend/src/app/components/custom/custom.component.ts`
2. Waarnemen van `@Input() message: string;` lijn. Verwacht wordt dat de getransformeerde hoofdletterwaarde aan deze variabele wordt toegewezen.
3. Het dialoogvenster Importeren `MapTo` -object van de AEM SPA Editor JS SDK en deze gebruiken om toe te wijzen aan de AEM-component:

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. Openen `cutom.component.html` en merkt op dat de `{{message}}` wordt weergegeven op een zijde van `<h2>` -tag.
5. Openen `custom.component.css` en voeg de volgende regel toe:

   ```css
   :host-context {
       display: block;
   }
   ```

   De tijdelijke aanduiding voor de AEM Editor moet correct worden weergegeven wanneer de component leeg is. `:host-context` of een `<div>` moet worden ingesteld op `display: block;`.

6. Stel de updates aan een lokaal AEM milieu van de wortel van de projectfolder op, gebruikend uw Maven vaardigheden:

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Sjabloonbeleid bijwerken

Navigeer vervolgens naar AEM om de updates te controleren en de `Custom Component` aan de SPA toe te voegen.

1. Controleer de registratie van het nieuwe verkoopmodel door naar [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   De bovenstaande twee regels geven de `CustomComponentImpl` is gekoppeld aan de `wknd-spa-angular/components/custom-component` en dat deze is geregistreerd via de verkoopmodel-exportfunctie.

2. Navigeer naar de SPA paginasjabloon op [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. Werk het beleid van de Container van de Lay-out bij om nieuwe toe te voegen `Custom Component` als toegestane component:

   ![Layoutcontainerbeleid bijwerken](assets/custom-component/custom-component-allowed.png)

   Sla de wijzigingen in het beleid op en bekijk de `Custom Component` als toegestane component:

   ![Aangepaste component als toegestane component](assets/custom-component/custom-component-allowed-layout-container.png)

## Auteur van de aangepaste component

Nu, auteur `Custom Component` met de AEM SPA Editor.

1. Navigeren naar [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. In `Edit` in de modus, voegt u de `Custom Component` aan de `Layout Container`:

   ![Nieuwe component invoegen](assets/custom-component/insert-custom-component.png)

3. Open het dialoogvenster van de component en voer een bericht in dat kleine letters bevat.

   ![De aangepaste component configureren](assets/custom-component/enter-dialog-message.png)

   Dit is het dialoogvenster dat is gemaakt op basis van het XML-bestand dat eerder in het hoofdstuk is opgenomen.

4. Sla de wijzigingen op. Merk op dat het getoonde bericht in alle gekapitaliseerd is.

   ![Bericht weergegeven in Kapitalen](assets/custom-component/message-displayed.png)

5. U kunt het JSON-model weergeven door naar [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). Zoeken naar `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   De JSON-waarde wordt ingesteld op alle hoofdletters op basis van de logica die aan het Sling-model is toegevoegd.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u geleerd hoe u een aangepaste AEM component kunt maken en hoe de modellen en dialoogvensters voor verkopers werken met het JSON-model.

U kunt de voltooide code altijd weergeven op [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) of controleer de code plaatselijk door aan de tak over te schakelen `Angular/custom-component-solution`.

### Volgende stappen {#next-steps}

[Een kerncomponent uitbreiden](extend-component.md) - Leer hoe u een bestaande Core Component kunt uitbreiden voor gebruik met de AEM SPA Editor. Het begrip hoe te om eigenschappen en inhoud aan een bestaande component toe te voegen is een krachtige techniek om de mogelijkheden van een implementatie van AEM SPARedacteur uit te breiden.
