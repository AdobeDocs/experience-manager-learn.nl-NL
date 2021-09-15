---
title: Een kerncomponent uitbreiden | Aan de slag met de AEM SPA Editor en reageren
description: Leer hoe u het JSON-model kunt uitbreiden voor een bestaande Core Component die u wilt gebruiken met de AEM SPA Editor. Het begrip hoe te om eigenschappen en inhoud aan een bestaande component toe te voegen is een krachtige techniek om de mogelijkheden van een implementatie van AEM SPARedacteur uit te breiden. Leer om het delegatiepatroon te gebruiken voor het uitbreiden van Sling Models en eigenschappen van de Verzameling van Middel.
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
version: Cloud Service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1091'
ht-degree: 0%

---

# Een kerncomponent uitbreiden {#extend-component}

Leer hoe te om een bestaande Component van de Kern uit te breiden die met de Redacteur van de SPA van de AEM moet worden gebruikt. Begrijpen hoe een bestaande component wordt uitgebreid is een krachtige techniek om de mogelijkheden van een AEM SPA de implementatie van de Redacteur aan te passen en uit te breiden.

## Doelstelling

1. Breid een bestaande Component van de Kern met extra eigenschappen en inhoud uit.
2. Begrijp de basis van de Overerving van de Component met het gebruik van `sling:resourceSuperType`.
3. Leer hoe u het [Delegatiepatroon](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) voor Sling Models gebruikt om bestaande logica en functionaliteit opnieuw te gebruiken.

## Wat u gaat maken

Dit hoofdstuk illustreert de extra code nodig om een extra bezit aan een standaard `Image` component toe te voegen om aan de vereisten voor een nieuwe `Banner` component te voldoen. De component `Banner` bevat dezelfde eigenschappen als de standaardcomponent `Image`, maar bevat een extra eigenschap voor gebruikers om de **Bannertekst** te vullen.

![Uiteindelijke authoring bannercomponent](assets/extend-component/final-author-banner-component.png)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [lokale ontwikkelomgeving](overview.md#local-dev-environment). Op dit punt wordt aangenomen dat gebruikers van de zelfstudie een goed inzicht hebben in de functie AEM SPA Editor.

## Overerving met Sling Resource Super Type {#sling-resource-super-type}

Om een bestaande component uit te breiden plaats een bezit genoemd `sling:resourceSuperType` op de definitie van uw component.  `sling:resourceSuperType`is een  [](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) eigenschap die kan worden ingesteld op de definitie van een AEM die naar een andere component wijst. Dit plaatst uitdrukkelijk de component om alle functionaliteit van de component te erven die als `sling:resourceSuperType` wordt geïdentificeerd.

Als wij `Image` component bij `wknd-spa-react/components/image` willen uitbreiden moeten wij de code in `ui.apps` module bijwerken.

1. Maak een nieuwe map onder de module `ui.apps` voor `banner` op `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. Onder `banner` creeer een definitie van de Component (`.content.xml`) als het volgende:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   Hiermee stelt u `wknd-spa-react/components/banner` in om alle functionaliteit van `wknd-spa-react/components/image` over te nemen.

## cq:editConfig {#cq-edit-config}

Het `_cq_editConfig.xml` dossier dicteert het belemmering en dalingsgedrag in AEM auteursUI. Wanneer het uitbreiden van de component van het Beeld is het belangrijk dat het middeltype de component zelf aanpast.

1. Maak in de module `ui.apps` een ander bestand onder `banner` met de naam `_cq_editConfig.xml`.
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

1. Het unieke aspect van het bestand is het knooppunt `<parameters>` dat het resourceType instelt op `wknd-spa-react/components/banner`.

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

Onze `Banner` component vereist een extra tekstgebied in de dialoog om `bannerText` te vangen. Aangezien wij het Verkopen overerving gebruiken, kunnen wij eigenschappen van [het Verspreiden Samenvoegen van het Middel ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) gebruiken om gedeelten van de dialoog met voeten te treden of uit te breiden. In dit voorbeeld is een nieuw tabblad toegevoegd aan het dialoogvenster om aanvullende gegevens van een auteur vast te leggen om de kaartcomponent te vullen.

1. Maak in de module `ui.apps` onder de map `banner` een map met de naam `_cq_dialog`.
1. Onder `_cq_dialog` maak een Dialog definition file `.content.xml`. Vul de selectie met de volgende code:

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

   De bovenstaande XML-definitie maakt een nieuw tabblad met de naam **Text** en geeft deze *voor* de bestaande tab **Asset**. Het zal één enkel gebied **Banner Tekst** bevatten.

1. Het dialoogvenster ziet er als volgt uit:

   ![Dialoogvenster Banner](assets/extend-component/banner-dialog.png)

   Merk op dat wij niet de lusjes voor **Activum** of **Metadata** moesten bepalen. Deze worden geërft via het `sling:resourceSuperType` bezit.

   Voordat we een voorvertoning van het dialoogvenster kunnen weergeven, moeten we de functie SPA en `MapTo` implementeren.

## SPA {#implement-spa-component}

Als u de component Banner wilt gebruiken met de SPA Editor, moet u een nieuwe SPA maken die wordt toegewezen aan `wknd-spa-react/components/banner`. Dit zal in `ui.frontend` module worden gedaan.

1. Maak in de module `ui.frontend` een nieuwe map voor `Banner` op `ui.frontend/src/components/Banner`.
1. Maak een nieuw bestand met de naam `Banner.js` onder de map `Banner`. Vul de selectie met de volgende code:

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
           if(BannerEditConfig.isEmpty(this.props)) {
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

   Deze SPA component verwijst naar de AEM component `wknd-spa-react/components/banner` die eerder is gemaakt.

1. `import-components.js` om `ui.frontend/src/components/import-components.js` bijwerken om de nieuwe `Banner` SPA component op te nemen:

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. Op dit punt kan het project worden opgesteld aan AEM en de dialoog kan worden getest. Implementeer het project met behulp van uw Maven-vaardigheden:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Werk het beleid van het SPA van het Malplaatje bij om de `Banner` component als **toegestane component** toe te voegen.

1. Navigeer naar een SPA pagina en voeg de component `Banner` toe aan een van de SPA pagina&#39;s:

   ![Bannercomponent toevoegen](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > In het dialoogvenster kunt u een waarde opslaan voor **Bannertekst**, maar deze waarde wordt niet weerspiegeld in de SPA. Om toe te laten, moeten wij het het Verkopen Model voor de component uitbreiden.

## Java-interface toevoegen {#java-interface}

Om uiteindelijk de waarden van de componentendialoog aan de component van de Reactie bloot te stellen moeten wij het het Schipen Model bijwerken dat JSON voor de `Banner` component bevolkt. Dit zal in `core` module worden gedaan die alle code van Java voor ons SPA project bevat.

Eerst maken we een nieuwe Java-interface voor `Banner` die de Java-interface `Image` uitbreidt.

1. Maak in de module `core` een nieuw bestand met de naam `BannerModel.java` op `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
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

   Hierdoor worden alle methoden overgenomen van de interface Core Component `Image` en wordt één nieuwe methode `getBannerText()` toegevoegd.

## Sling-model implementeren {#sling-model}

Implementeer vervolgens het Sling Model voor de `BannerModel`-interface.

1. Maak in de module `core` een nieuw bestand met de naam `BannerModelImpl.java` op `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

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

   Let op het gebruik van de `@Model`- en `@Exporter`-annotaties om ervoor te zorgen dat het Sling Model kan worden geserialiseerd als JSON via de Sling Model Exporter.

   `BannerModelImpl.java` gebruikt het patroon van de  [Delegatie voor het Verdelen ](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) Modellen om te vermijden herschrijvend alle logica van de kerncomponent van het Beeld.

1. Neem de volgende regels in acht:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   Met de bovenstaande annotatie wordt een afbeeldingsobject met de naam `image` geïnstantieerd op basis van de overerving `sling:resourceSuperType` van de component `Banner`.

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   Het is dan mogelijk om het `image` voorwerp eenvoudig te gebruiken om methodes uit te voeren die door de `Image` interface worden bepaald, zonder het moeten zelf de logica schrijven. Deze techniek wordt gebruikt voor `getSrc()`, `getAlt()` en `getTitle()`.

1. Open een terminalvenster en implementeer alleen de updates voor de `core`-module met behulp van het profiel Maven `autoInstallBundle` uit de map `core`.

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## Alles samenvoegen {#put-together}

1. Ga terug naar AEM en open de SPA pagina met de component `Banner`.
1. Werk de `Banner` component bij om **Bannertekst** op te nemen:

   ![Bannertekst](assets/extend-component/banner-text-dialog.png)

1. De component vullen met een afbeelding:

   ![Afbeelding toevoegen aan bannerdialoogvenster](assets/extend-component/banner-dialog-image.png)

   Sla de updates van het dialoogvenster op.

1. U zou nu de teruggegeven waarde van **Banner Tekst** moeten zien:

   ![Bannertekst weergegeven](assets/extend-component/banner-text-displayed.png)

1. Bekijk de JSON-modelreactie op: [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) en zoek naar `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   U ziet dat het JSON-model wordt bijgewerkt met extra sleutel/waardeparen na implementatie van het Sling-model in `BannerModelImpl.java`.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, hebt u geleerd hoe u een AEM component kunt uitbreiden met de code en hoe de Sling-modellen en -dialoogvensters werken met het JSON-model.
