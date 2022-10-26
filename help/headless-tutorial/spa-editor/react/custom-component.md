---
title: Een aangepaste weercomponent maken | Aan de slag met de AEM SPA Editor en reageren
description: Leer hoe u een aangepaste weercomponent maakt die u kunt gebruiken met de AEM SPA Editor. Leer hoe u dialoogvensters met auteurs en Sling Models ontwikkelt om het JSON-model uit te breiden en een aangepaste component te vullen. De Open Weather-API en React Open Weather-componenten worden gebruikt.
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
kt: 5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 82466e0e-b573-440d-b806-920f3585b638
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 0%

---

# Een aangepaste Weather-component maken {#custom-component}

Leer hoe u een aangepaste weercomponent maakt die u kunt gebruiken met de AEM SPA Editor. Leer hoe u dialoogvensters met auteurs en Sling Models ontwikkelt om het JSON-model uit te breiden en een aangepaste component te vullen. De [Weer-API openen](https://openweathermap.org) en [Reageren op Open Weather-component](https://www.npmjs.com/package/react-open-weather) worden gebruikt.

## Doelstelling

1. Begrijp de rol van Sling Models in het manipuleren van JSON model API die door AEM wordt verstrekt.
2. Begrijp hoe te om nieuwe AEM componentendialogen tot stand te brengen.
3. Leer een **aangepast** AEM Component die met het SPA redacteurskader compatibel is.

## Wat u gaat maken

Er wordt een eenvoudige weercomponent gemaakt. Deze component kan door inhoudsauteurs aan de SPA worden toegevoegd. Met behulp van een AEM kunnen auteurs de locatie instellen voor het weer dat wordt weergegeven.  De implementatie van deze component illustreert de stappen die nodig zijn om een netto-nieuwe AEM te maken die compatibel is met het kader van de AEM SPA Editor.

![De Open Weather-component configureren](assets/custom-component/enter-dialog.png)

## Vereisten

Controleer de vereiste gereedschappen en instructies voor het instellen van een [plaatselijke ontwikkelomgeving](overview.md#local-dev-environment). Dit hoofdstuk is een voortzetting van het [Navigatie en routering](navigation-routing.md) hoofdstuk, nochtans om langs alles te volgen u wenst is een SPA-toegelaten AEM project dat aan een lokale AEM instantie wordt opgesteld.

### API-sleutel voor weezelaar openen

Een API-sleutel van [Weer openen](https://openweathermap.org/) is nodig om de zelfstudie te volgen. [Aanmelden is gratis](https://home.openweathermap.org/users/sign_up) voor een beperkte hoeveelheid API-aanroepen.

## De AEM component definiëren

Een AEM component wordt gedefinieerd als een knooppunt en eigenschappen. In het project worden deze knooppunten en eigenschappen vertegenwoordigd als XML-bestanden in de `ui.apps` module. Maak vervolgens de AEM component in het dialoogvenster `ui.apps` module.

>[!NOTE]
>
> Een snelle herhaling op de [de grondbeginselen van AEM componenten kunnen nuttig zijn](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. Open in de IDE van uw keuze de `ui.apps` map.
2. Navigeren naar `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components` en maak een nieuwe map met de naam `open-weather`.
3. Een nieuw bestand maken met de naam `.content.xml` onder de `open-weather` map. Vul de `open-weather/.content.xml` met het volgende:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![Aangepaste componentdefinitie maken](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` - identificeert dat dit knooppunt een AEM component is.

   `jcr:title` Dit is de waarde die wordt weergegeven voor auteurs van inhoud en de `componentGroup` bepaalt de groepering van componenten in auteursUI.

4. Onder de `custom-component` map, een andere map maken met de naam `_cq_dialog`.
5. Onder de `_cq_dialog` map maken een nieuw bestand met de naam `.content.xml` en vult deze met het volgende:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Open Weather"
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
                                               <label
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The label to display for the component"
                                                   fieldLabel="Label"
                                                   name="./label"/>
                                               <lat
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The latitude of the location."
                                                   fieldLabel="Latitude"
                                                   step="any"
                                                   name="./lat" />
                                               <lon
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The longitude of the location."
                                                   fieldLabel="Longitude"
                                                   step="any"
                                                   name="./lon"/>
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

   Het bovenstaande XML-bestand genereert een zeer eenvoudig dialoogvenster voor het `Weather Component`. Het kritieke deel van het bestand is de binnenzijde `<label>`, `<lat>` en `<lon>` knooppunten. Dit dialoogvenster bevat twee `numberfield`s en a `textfield` dat een gebruiker toestaat om het weer te vormen dat moet worden getoond.

   Er wordt een verkoopmodel gemaakt naast de waarde van de optie `label`,`lat` en `long` eigenschappen via het JSON-model.

   >[!NOTE]
   >
   > Je kunt nog veel meer bekijken [voorbeelden van dialoogvensters door de definities van de Component van de Kern te bekijken](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). U kunt ook aanvullende formuliervelden weergeven, zoals `select`, `textarea`, `pathfield`, beschikbaar onder `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   Met een traditionele AEM [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html) is doorgaans vereist. Aangezien de SPA de component zal teruggeven, is geen manuscript van HTML nodig.

## Het verkoopmodel maken

Sling-modellen zijn annotaties die worden aangedreven door Java &quot;POJO&#39;s&quot; (Plain Old Java Objects) en die het gemakkelijker maken gegevens van de JCR aan Java-variabelen toe te wijzen. [Verkoopmodellen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models) typisch functie om complexe server-kant bedrijfslogica voor AEM Componenten in te kapselen.

In de context van de Redacteur van de SPA, stelt het Verdelen Modellen de inhoud van een component door het model JSON door een eigenschap bloot gebruikend [Verkoopmodel exporteren](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html).

1. Open in de IDE van uw keuze de `core` module bij `aem-guides-wknd-spa.react/core`.
1. Een bestand maken met de naam `OpenWeatherModel.java` om `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Vullen `OpenWeatherModel.java` met het volgende:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.export.json.ComponentExporter;
   
   // Sling Models intended to be used with SPA Editor must extend ComponentExporter interface
   public interface OpenWeatherModel extends ComponentExporter {
       public String getLabel();
       public double getLat();
       public double getLon();
   }
   ```

   Dit is de Java-interface voor onze component. Om ons verkoopmodel compatibel te maken met het SPA Editor-kader moet het de `ComponentExporter` klasse.

1. Een map maken met de naam `impl` beneide `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. Een bestand met de naam `OpenWeatherModelImpl.java` beneide `impl` en vullen met het volgende:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import com.adobe.aem.guides.wkndspa.react.core.models.OpenWeatherModel;
   
   // Sling Model annotation
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { OpenWeatherModel.class, ComponentExporter.class }, 
       resourceType = OpenWeatherModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter( //Exporter annotation that serializes the modoel as JSON
       name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
       extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class OpenWeatherModelImpl implements OpenWeatherModel {
   
       @ValueMapValue
       private String label; //maps variable to jcr property named "label" persisted by Dialog
   
       @ValueMapValue
       private double lat; //maps variable to jcr property named "lat"
   
       @ValueMapValue
       private double lon; //maps variable to jcr property named "lon"
   
       // points to AEM component definition in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/open-weather";
   
       // public getter method to expose value of private variable `label`
       // adds additional logic to default the label to "(Default)" if not set.
       @Override
       public String getLabel() {
           return StringUtils.isNotBlank(label) ? label : "(Default)";
       }
   
       // public getter method to expose value of private variable `lat`
       @Override
       public double getLat() {
           return lat;
       }
   
       // public getter method to expose value of private variable `lon`
       @Override
       public double getLon() {
           return lon;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/open-weather`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return OpenWeatherModelImpl.RESOURCE_TYPE;
       }
   } 
   ```

   De variabele static `RESOURCE_TYPE` moet wijzen naar het pad in `ui.apps` van de component. De `getExportedType()` wordt gebruikt om de JSON-eigenschappen via `MapTo`. `@ValueMapValue` is een aantekening die de jcr-eigenschap leest die door het dialoogvenster is opgeslagen.

## De SPA bijwerken

Werk vervolgens de React-code bij en voeg de [Reageren op Open Weather-component](https://www.npmjs.com/package/react-open-weather) en deze aan de AEM component laten toewijzen die in de vorige stappen is gemaakt.

1. De component React Open Weather installeren als een **npm** afhankelijkheid:

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. Een nieuwe map maken met de naam `OpenWeather` om `ui.frontend/src/components/OpenWeather`.
1. Een bestand met de naam toevoegen `OpenWeather.js` en vult deze met het volgende:

   ```js
   import React from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   import ReactWeather, { useOpenWeather } from 'react-open-weather';
   
   // Open weather API Key
   // For simplicity it is hard coded in the file, ideally this is extracted in to an environment variable
   const API_KEY = 'YOUR_API_KEY';
   
   // Logic to render placeholder or component
   const OpenWeatherEditConfig = {
   
       emptyLabel: 'Weather',
       isEmpty: function(props) {
           return !props || !props.lat || !props.lon || !props.label;
       }
   };
   
   // Wrapper function that includes react-open-weather component
   function ReactWeatherWrapper(props) {
       const { data, isLoading, errorMessage } = useOpenWeather({
           key: API_KEY,
           lat: props.lat, // passed in from AEM JSON
           lon: props.lon, // passed in from AEM JSON
           lang: 'en',
           unit: 'imperial', // values are (metric, standard, imperial)
       });
   
       return (
           <div className="cmp-open-weather">
               <ReactWeather
                   isLoading={isLoading}
                   errorMessage={errorMessage}
                   data={data}
                   lang="en"
                   locationLabel={props.label} // passed in from AEM JSON
                   unitsLabels={{ temperature: 'F', windSpeed: 'mph' }}
                   showForecast={false}
                 />
           </div>
       );
   }
   
   export default function OpenWeather(props) {
   
           // render nothing if component not configured
           if (OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. Bijwerken `import-components.js` om `ui.frontend/src/components/import-components.js` de `OpenWeather` component:

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. Stel alle updates aan een lokaal AEM milieu van de wortel van de projectfolder op, gebruikend uw Maven vaardigheden:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## Sjabloonbeleid bijwerken

Navigeer vervolgens naar AEM om de updates te controleren en de `OpenWeather` aan de SPA toe te voegen.

1. Controleer de registratie van het nieuwe verkoopmodel door naar [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   De bovenstaande twee regels geven de `OpenWeatherModelImpl` is gekoppeld aan de `wknd-spa-react/components/open-weather` en dat deze is geregistreerd via de verkoopmodel-exportfunctie.

1. Navigeer naar de SPA paginasjabloon op [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Werk het beleid van de Container van de Lay-out bij om nieuwe toe te voegen `Open Weather` als toegestane component:

   ![Layoutcontainerbeleid bijwerken](assets/custom-component/custom-component-allowed.png)

   Sla de wijzigingen in het beleid op en bekijk de `Open Weather` als toegestane component:

   ![Aangepaste component als toegestane component](assets/custom-component/custom-component-allowed-layout-container.png)

## Auteur van de Open component Weather

Nu, auteur `Open Weather` met de AEM SPA Editor.

1. Navigeren naar [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html).
1. In `Edit` modus, voegt u de `Open Weather` aan de `Layout Container`:

   ![Nieuwe component invoegen](assets/custom-component/insert-custom-component.png)

1. Open het dialoogvenster van de component en voer een **Label**, **Breedte**, en **Lengtegraad**. Bijvoorbeeld **San Diego**, **32,7157**, en **-117,1611**. De getallen op het westelijke halfrond en het zuidelijke halfrond worden weergegeven als negatieve getallen met de Open Weather API

   ![De Open Weather-component configureren](assets/custom-component/enter-dialog.png)

   Dit is het dialoogvenster dat is gemaakt op basis van het XML-bestand dat eerder in het hoofdstuk is opgenomen.

1. Sla de wijzigingen op. Waarnemen dat het weer **San Diego** wordt nu weergegeven:

   ![component Weather bijgewerkt](assets/custom-component/weather-updated.png)

1. U kunt het JSON-model weergeven door naar [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Zoeken naar `wknd-spa-react/components/open-weather`:

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   De JSON-waarden worden uitgevoerd door het Sling-model. Deze JSON-waarden worden als props doorgegeven aan de component React.

## Gefeliciteerd! {#congratulations}

U hebt geleerd hoe u een aangepaste AEM kunt maken die u met de SPA Editor wilt gebruiken. U hebt ook geleerd hoe dialoogvensters, JCR-eigenschappen en Sling Models communiceren met de uitvoer van het JSON-model.

### Volgende stappen {#next-steps}

[Een kerncomponent uitbreiden](extend-component.md) - Leer hoe u een bestaande AEM Core Component kunt uitbreiden voor gebruik met de AEM SPA Editor. Het begrip hoe te om eigenschappen en inhoud aan een bestaande component toe te voegen is een krachtige techniek om de mogelijkheden van een implementatie van AEM SPARedacteur uit te breiden.
