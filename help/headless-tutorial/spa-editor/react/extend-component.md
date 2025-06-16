---
title: Een kerncomponent uitbreiden | Aan de slag met de AEM SPA Editor en Reageren
description: Leer hoe te om het Model JSON voor een bestaande Component van de Kern uit te breiden die met de Redacteur van AEM SPA moet worden gebruikt. Het begrip hoe te om eigenschappen en inhoud aan een bestaande component toe te voegen is een krachtige techniek om de mogelijkheden van een implementatie van de Redacteur van AEM uit te breiden SPA. Leer om het delegatiepatroon te gebruiken voor het uitbreiden van Sling Models en eigenschappen van de Verzameling van Middel.
feature: SPA Editor, Core Components
version: Experience Manager as a Cloud Service
jira: KT-5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
duration: 316
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1058'
ht-degree: 0%

---

# Een kerncomponent uitbreiden {#extend-component}

{{spa-editor-deprecation}}

Leer hoe te om een bestaande Component van de Kern uit te breiden die met de Redacteur van AEM SPA moet worden gebruikt. Begrijpen hoe te om een bestaande component uit te breiden is een krachtige techniek om de mogelijkheden van een implementatie van de Redacteur van AEM aan te passen en uit te breiden SPA.

## Doelstelling

1. Breid een bestaande Component van de Kern met extra eigenschappen en inhoud uit.
2. Begrijp de basis van Componentovererving met het gebruik van `sling:resourceSuperType`.
3. Leer hoe te hefboomwerking het [ Patroon van de Delegatie ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) voor het Verdelen Modellen om bestaande logica en functionaliteit opnieuw te gebruiken.

## Wat u gaat maken

In dit hoofdstuk wordt de aanvullende code weergegeven die nodig is om een extra eigenschap toe te voegen aan een standaard `Image` -component om te voldoen aan de vereisten voor een nieuwe `Banner` -component. De `Banner` component bevat alle zelfde eigenschappen zoals de standaard `Image` component maar omvat een extra bezit voor gebruikers om de **Tekst van de Banner** te bevolken.

![ Definitief authored bannercomponent ](assets/extend-component/final-author-banner-component.png)

## Vereisten

Herzie het vereiste tooling en de instructies voor vestiging a [ lokale ontwikkelomgeving ](overview.md#local-dev-environment). Het wordt verondersteld op dit punt in de zelfstudie hebben de gebruikers een stevig inzicht in de eigenschap van de Redacteur van AEM SPA.

## Overerving met Sling Resource Super Type {#sling-resource-super-type}

Als u een bestaande component wilt uitbreiden, stelt u een eigenschap met de naam `sling:resourceSuperType` in voor de definitie van de component.  `sling:resourceSuperType` is a [ bezit ](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) dat op de definitie van een component van AEM kan worden geplaatst die aan een andere component richt. Hiermee wordt de component expliciet ingesteld om alle functionaliteit over te nemen van de component die wordt aangeduid als de `sling:resourceSuperType` .

Als we de component `Image` in `wknd-spa-react/components/image` willen uitbreiden, moeten we de code in de module `ui.apps` bijwerken.

1. Maak een nieuwe map onder de module `ui.apps` voor `banner` at `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner` .
1. Onder `banner` maakt u als volgt een componentdefinitie (`.content.xml` ):

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Hiermee wordt `wknd-spa-react/components/banner` ingesteld om alle functionaliteit van `wknd-spa-react/components/image` over te nemen.

## cq:editConfig {#cq-edit-config}

In het `_cq_editConfig.xml` -bestand wordt het gedrag voor slepen en neerzetten voorgeschreven in de AEM-ontwerpinterface. Wanneer het uitbreiden van de component van het Beeld is het belangrijk dat het middeltype de component zelf aanpast.

1. Maak in de module `ui.apps` nog een bestand onder `banner` genaamd `_cq_editConfig.xml` .
1. Vul `_cq_editConfig.xml` met de volgende XML:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="cq:EditConfig">
       <cq:dropTargets jcr:primaryType="nt:unstructured">
           <image
               jcr:primaryType="cq:DropTargetConfig"
               accept="[image/gif,image/jpeg,image/png,image/webp,image/tiff,image/svg\\+xml]"
               groups="[media]"
               propertyName="./fileReference">
               <parameters
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wknd-spa-react/components/banner"
                   imageCrop=""
                   imageMap=""
                   imageRotate=""/>
           </image>
       </cq:dropTargets>
       <cq:inplaceEditing
           jcr:primaryType="cq:InplaceEditingConfig"
           active="{Boolean}true"
           editorType="image">
           <inplaceEditingConfig jcr:primaryType="nt:unstructured">
               <plugins jcr:primaryType="nt:unstructured">
                   <crop
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*">
                       <aspectRatios jcr:primaryType="nt:unstructured">
                           <wideLandscape
                               jcr:primaryType="nt:unstructured"
                               name="Wide Landscape"
                               ratio="0.6180"/>
                           <landscape
                               jcr:primaryType="nt:unstructured"
                               name="Landscape"
                               ratio="0.8284"/>
                           <square
                               jcr:primaryType="nt:unstructured"
                               name="Square"
                               ratio="1"/>
                           <portrait
                               jcr:primaryType="nt:unstructured"
                               name="Portrait"
                               ratio="1.6180"/>
                       </aspectRatios>
                   </crop>
                   <flip
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="-"/>
                   <map
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff,image/svg+xml]"
                       features="*"/>
                   <rotate
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
                   <zoom
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
               </plugins>
               <ui jcr:primaryType="nt:unstructured">
                   <inline
                       jcr:primaryType="nt:unstructured"
                       toolbar="[crop#launch,rotate#right,history#undo,history#redo,fullscreen#fullscreen,control#close,control#finish]">
                       <replacementToolbars
                           jcr:primaryType="nt:unstructured"
                           crop="[crop#identifier,crop#unlaunch,crop#confirm]"/>
                   </inline>
                   <fullscreen jcr:primaryType="nt:unstructured">
                       <toolbar
                           jcr:primaryType="nt:unstructured"
                           left="[crop#launchwithratio,rotate#right,flip#horizontal,flip#vertical,zoom#reset100,zoom#popupslider]"
                           right="[history#undo,history#redo,fullscreen#fullscreenexit]"/>
                       <replacementToolbars jcr:primaryType="nt:unstructured">
                           <crop
                               jcr:primaryType="nt:unstructured"
                               left="[crop#identifier]"
                               right="[crop#unlaunch,crop#confirm]"/>
                           <map
                               jcr:primaryType="nt:unstructured"
                               left="[map#rectangle,map#circle,map#polygon]"
                               right="[map#unlaunch,map#confirm]"/>
                       </replacementToolbars>
                   </fullscreen>
               </ui>
           </inplaceEditingConfig>
       </cq:inplaceEditing>
   </jcr:root>
   ```

1. Het unieke aspect van het bestand is het knooppunt `<parameters>` dat het resourceType instelt op `wknd-spa-react/components/banner` .

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   Voor de meeste componenten is geen `_cq_editConfig` vereist. Afbeeldingscomponenten en afstammingen vormen hierop een uitzondering.

## Het dialoogvenster uitbreiden {#extend-dialog}

De component `Banner` vereist een extra tekstveld in het dialoogvenster om het `bannerText` vast te leggen. Aangezien wij het Verdelen overerving gebruiken, kunnen wij eigenschappen van de [ Verschuivende Fusie van het Middel ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) gebruiken om gedeelten van de dialoog met voeten te treden of uit te breiden. In dit voorbeeld is een nieuw tabblad toegevoegd aan het dialoogvenster om aanvullende gegevens van een auteur vast te leggen om de kaartcomponent te vullen.

1. Maak in de module `ui.apps` onder de map `banner` een map met de naam `_cq_dialog` .
1. Onder `_cq_dialog` maakt u een definitiebestand voor het dialoogvenster `.content.xml` . Vul de selectie met de volgende code:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Banner"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <text
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Text"
                           sling:orderBefore="asset"
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
                                               <textGroup
                                                   granite:hide="${cqDesign.titleHidden}"
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/well">
                                                   <items jcr:primaryType="nt:unstructured">
                                                       <bannerText
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           fieldDescription="Text to display on top of the banner."
                                                           fieldLabel="Banner Text"
                                                           name="./bannerText"/>
                                                   </items>
                                               </textGroup>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </text>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   De bovenstaande definitie van XML zal tot een nieuw lusje leiden genoemd **Tekst** en tot het *opdracht geven vóór* het bestaande **Activa** tabel. Het zal één enkel gebied **Tekst van de Banner** bevatten.

1. Het dialoogvenster ziet er als volgt uit:

   ![ Banner definitieve dialoog ](assets/extend-component/banner-dialog.png)

   Merk op dat wij niet de lusjes voor **Activa** of **Meta-gegevens** moesten bepalen. Deze worden overgeërfd via de eigenschap `sling:resourceSuperType` .

   Voordat we een voorvertoning van het dialoogvenster kunnen weergeven, moeten we de SPA-component en de functie `MapTo` implementeren.

## SPA-component implementeren {#implement-spa-component}

Om de component van de Banner met de Redacteur van het KUUROORD te gebruiken, moet een nieuwe component van het KUUROORD worden gecreeerd die aan `wknd-spa-react/components/banner` in kaart zal brengen. Dit gebeurt in de module `ui.frontend` .

1. Maak in de module `ui.frontend` een nieuwe map voor `Banner` at `ui.frontend/src/components/Banner` .
1. Maak een nieuw bestand met de naam `Banner.js` onder de map `Banner` . Vul de selectie met de volgende code:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   export const BannerEditConfig = {
       emptyLabel: 'Banner',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   
   export default class Banner extends Component {
   
       get content() {
           return <img     
                   className="Image-src"
                   src={this.props.src}
                   alt={this.props.alt}
                   title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       // display our custom bannerText property!
       get bannerText() {
           if(this.props.bannerText) {
               return <h4>{this.props.bannerText}</h4>;
           }
   
           return null;
       }
   
       render() {
           if (BannerEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
               <div className="Banner">
                   {this.bannerText}
                   <div className="BannerImage">{this.content}</div>
               </div>
           );
       }
   }
   
   MapTo('wknd-spa-react/components/banner')(Banner, BannerEditConfig);
   ```

   Deze SPA-component verwijst naar de eerder gemaakte AEM-component `wknd-spa-react/components/banner` .

1. Werk `import-components.js` bij `ui.frontend/src/components/import-components.js` bij om de nieuwe `Banner` SPA-component op te nemen:

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. Op dit punt kan het project worden opgesteld aan AEM en kan de dialoog worden getest. Implementeer het project met behulp van uw Maven-vaardigheden:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Werk het beleid van het Malplaatje van het KUUROORD bij om de `Banner` component als **toegestane component** toe te voegen.

1. Navigeer naar een pagina van het KUUROORD en voeg de `Banner` component aan één van de pagina&#39;s van het KUUROORD toe:

   ![ voeg component Banner ](assets/extend-component/add-banner-component.png) toe

   >[!NOTE]
   >
   > De dialoog zal u toestaan om een waarde voor **Tekst van de Banner** te bewaren maar deze waarde wordt niet weerspiegeld in de component van het KUUROORD. Om toe te laten, moeten wij het het Verkopen Model voor de component uitbreiden.

## Java-interface toevoegen {#java-interface}

Als u uiteindelijk de waarden uit het dialoogvenster Component toegankelijk wilt maken voor de component React, moet u het Sling-model bijwerken dat de JSON voor de component `Banner` vult. Dit wordt gedaan in de `core` module die alle code van Java voor ons project van het KUUROORD bevat.

Eerst maken we een nieuwe Java-interface voor `Banner` die de Java-interface van `Image` uitbreidt.

1. Maak in de module `core` een nieuw bestand met de naam `BannerModel.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models` .
1. Vul `BannerModel.java` met het volgende:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   Hiermee worden alle methoden overgenomen van de interface Core Component `Image` en wordt één nieuwe methode `getBannerText()` toegevoegd.

## Sling-model implementeren {#sling-model}

Implementeer vervolgens het Sling Model voor de interface `BannerModel` .

1. Maak in de module `core` een nieuw bestand met de naam `BannerModelImpl.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl` .

1. Vul `BannerModelImpl.java` met het volgende:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import com.adobe.aem.guides.wkndspa.react.core.models.BannerModel;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.via.ResourceSuperType;
   
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { BannerModel.class,ComponentExporter.class}, 
       resourceType = BannerModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
   public class BannerModelImpl implements BannerModel {
   
       // points to the the component resource path in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/banner";
   
       @Self
       private SlingHttpServletRequest request;
   
       // With sling inheritance (sling:resourceSuperType) we can adapt the current resource to the Image class
       // this allows us to re-use all of the functionality of the Image class, without having to implement it ourself
       // see https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models
       @Self
       @Via(type = ResourceSuperType.class)
       private Image image;
   
       // map the property saved by the dialog to a variable named `bannerText`
       @ValueMapValue
       private String bannerText;
   
       // public getter to expose the value of `bannerText` via the Sling Model and JSON output
       @Override
       public String getBannerText() {
           return bannerText;
       }
   
       // Re-use the Image class for all other methods:
   
       @Override
       public String getSrc() {
           return null != image ? image.getSrc() : null;
       }
   
       @Override
       public String getAlt() {
           return null != image ? image.getAlt() : null;
       }
   
       @Override
       public String getTitle() {
           return null != image ? image.getTitle() : null;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/banner`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return BannerModelImpl.RESOURCE_TYPE;
       }
   }
   ```

   Let op het gebruik van de `@Model` - en `@Exporter` -aantekeningen om ervoor te zorgen dat het Sling-model via de Sling Model Exporter met serienummering kan worden gecodeerd als JSON.

   `BannerModelImpl.java` gebruikt het [ patroon van de Delegatie voor het Verdelen Modellen ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) om te vermijden herschrijvend alle logica van de kerncomponent van het Beeld.

1. Controleer de volgende regels:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   In de bovenstaande aantekening wordt een afbeeldingsobject met de naam `image` geïnstantieerd op basis van de `sling:resourceSuperType` overerving van de component `Banner` .

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Het is dan mogelijk om het `image` -object eenvoudig te gebruiken voor het implementeren van methoden die zijn gedefinieerd door de `Image` -interface, zonder dat u zelf de logica hoeft te schrijven. Deze techniek wordt gebruikt voor `getSrc()` , `getAlt()` en `getTitle()` .

1. Open een terminalvenster en implementeer alleen de updates voor de module `core` met behulp van het profiel Maven `autoInstallBundle` uit de map `core` .

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## Alles samenvoegen {#put-together}

1. Ga terug naar AEM en open de pagina van het KUUROORD die de `Banner` component heeft.
1. Werk de `Banner` component bij om **Tekst van de Banner** te omvatten:

   ![ de Tekst van de Banner ](assets/extend-component/banner-text-dialog.png)

1. De component vullen met een afbeelding:

   ![ voeg beeld aan bannerdialoog toe ](assets/extend-component/banner-dialog-image.png)

   Sla de dialoogupdates op.

1. U zou nu de teruggegeven waarde van **Tekst van de Banner** moeten zien:

![ getoonde Tekst van de Banner ](assets/extend-component/banner-text-displayed.png)

1. Bekijk de JSON modelreactie bij: [ http://localhost:4502/content/wknd-spa-react/us/en.model.json ](http://localhost:4502/content/wknd-spa-react/us/en.model.json) en onderzoek naar `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   U ziet dat het JSON-model wordt bijgewerkt met extra sleutel-/waardeparen nadat het Sling-model is geïmplementeerd in `BannerModelImpl.java` .

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u geleerd hoe u een AEM-component kunt uitbreiden met de code en hoe modellen en dialoogvensters voor verkopers werken met het JSON-model.
