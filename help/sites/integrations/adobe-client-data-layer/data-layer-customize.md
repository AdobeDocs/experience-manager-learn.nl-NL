---
title: De gegevenslaag voor de Adobe-client aanpassen met AEM componenten
description: Leer hoe u de gegevenslaag Adobe Client aanpast met inhoud van aangepaste AEM Componenten. Leer hoe u API's van AEM Core Components kunt gebruiken om de gegevenslaag uit te breiden en aan te passen.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '2016'
ht-degree: 0%

---

# De gegevenslaag voor de Adobe-client aanpassen met AEM componenten {#customize-data-layer}

Leer hoe u de gegevenslaag Adobe Client aanpast met inhoud van aangepaste AEM Componenten. Leer hoe u API&#39;s kunt gebruiken die worden geleverd door [Core Components AEM om uit te breiden](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) en past u de gegevenslaag aan.

## Wat u gaat maken

![Byline-gegevenslaag](assets/adobe-client-data-layer/byline-data-layer-html.png)

In dit leerprogramma zult u diverse opties onderzoeken om de Laag van Gegevens van de Cliënt van Adobe uit te breiden door WKND bij te werken [Byline-component](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html). Dit is een douanecomponent en de lessen die in dit leerprogramma worden geleerd kunnen op andere douanecomponenten worden toegepast.

### Doelstellingen {#objective}

1. Injecteer componentgegevens in de gegevenslaag door een Sling Model en component HTML uit te breiden
1. Gebruik gegevenslaaghulpprogramma&#39;s van de Core Component om de inspanning te verminderen
1. Gegevenskenmerken van de kerncomponent gebruiken om deze aan te sluiten op bestaande gegevenslaaggebeurtenissen

## Vereisten {#prerequisites}

A **plaatselijke ontwikkelomgeving** is nodig om deze zelfstudie te voltooien. Schermafbeeldingen en video worden vastgelegd met de AEM as a Cloud Service SDK die op een macOS wordt uitgevoerd. Opdrachten en code zijn onafhankelijk van het lokale besturingssysteem, tenzij anders aangegeven.

**Nieuw bij AEM as a Cloud Service?** Kijk uit de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met behulp van de AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).

**Nieuw bij AEM 6.5?** Kijk uit de [volgende gids voor het opzetten van een lokale ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## De WKND-referentiesite downloaden en implementeren {#set-up-wknd-site}

Deze zelfstudie breidt de component Byline in de WKND-verwijzingssite uit. Klonen en de WKND-codebasis installeren in uw lokale omgeving.

1. Een lokale QuickStart starten **auteur** instantie van AEM die wordt uitgevoerd op [http://localhost:4502](http://localhost:4502).
1. Open een eindvenster en kloon de WKND codebasis gebruikend Git:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Implementeer de WKND-codebasis in een lokale instantie van AEM:

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Als u AEM 6.5 en het nieuwste servicepack gebruikt, voegt u de `classic` profiel voor de Maven-opdracht:
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. Open een nieuw browservenster en meld u aan bij AEM. Een **Tijdschrift** pagina als: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![Byline-component op pagina](assets/adobe-client-data-layer/byline-component-onpage.png)

   Er wordt een voorbeeld weergegeven van de component Naamregel die aan de pagina is toegevoegd als onderdeel van een ervaringsfragment. U kunt het ervaringsfragment bekijken op [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. Open de ontwikkelaarsgereedschappen en voer de volgende opdracht in het dialoogvenster **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect de reactie om de huidige staat van de gegevenslaag op een AEM plaats te zien. U moet informatie over de pagina en de afzonderlijke componenten bekijken.

   ![Reactie Adobe-gegevenslaag](assets/data-layer-state-response.png)

   Merk op dat de component Byline niet in de Laag van Gegevens vermeld is.

## Het model voor bylineverkoop bijwerken {#sling-model}

Als u gegevens over de component in de gegevenslaag wilt injecteren, moet u eerst het Sling-model van de component bijwerken. Werk vervolgens de Java-interface en implementatie van het Sling Model van de Naamregel bij om een nieuwe methode toe te voegen `getData()`. Deze methode zal de eigenschappen bevatten wij in de gegevenslaag willen injecteren.

1. Open in de IDE van uw keuze de `aem-guides-wknd` project. Ga naar de `core` module.
1. Het bestand openen `Byline.java` om `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

   ![Naamregel Java-interface](assets/adobe-client-data-layer/byline-java-interface.png)

1. Voeg een nieuwe methode aan de interface toe:

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. Het bestand openen `BylineImpl.java` om `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

   Dit is de uitvoering van de `Byline` en wordt geïmplementeerd als een Sling Model.

1. Voeg de volgende instructies voor importeren toe aan het begin van het bestand:

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   De `fasterxml.jackson` API&#39;s worden gebruikt voor het serieel ordenen van de gegevens die als JSON beschikbaar moeten worden gemaakt. De `ComponentUtils` van AEM Core Components worden gebruikt om te controleren of de gegevenslaag is ingeschakeld.

1. Niet-geïmplementeerde methode toevoegen `getData()` tot `BylineImple.java`:

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   In bovenstaande methode wordt een nieuwe `HashMap` wordt gebruikt om de eigenschappen vast te leggen die als JSON beschikbaar moeten worden gemaakt. Bestaande methoden zoals `getName()` en `getOccupations()` worden gebruikt. `@type` vertegenwoordigt het unieke middeltype van de component, staat dit een cliënt toe om gebeurtenissen en/of trekkers gemakkelijk te identificeren die op het type van component worden gebaseerd.

   De `ObjectMapper` wordt gebruikt om de eigenschappen van een serienummer te voorzien en een JSON-tekenreeks te retourneren. Deze JSON-tekenreeks kan vervolgens in de gegevenslaag worden geïnjecteerd.

1. Open een terminalvenster. Alleen de `core` met uw Maven-vaardigheden:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## De HTML voor naamregel bijwerken {#htl}

Werk vervolgens de `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL (de Taal van het Malplaatje van HTML) is het malplaatje wordt gebruikt om de HTML van de component terug te geven die.

Een speciaal gegevenskenmerk `data-cmp-data-layer` op elke AEM wordt Component gebruikt om zijn gegevenslaag bloot te stellen.  JavaScript verstrekt door AEM de Componenten van de Kern zoekt dit gegevensattribuut, de waarvan waarde met het Koord JSON wordt bevolkt dat door het Model van de Sling van de Byline is teruggekeerd `getData()` methode, en injecteert de waarden in de laag van Gegevens van de Cliënt van Adobe.

1. In winde open `aem-guides-wknd` project. Ga naar de `ui.apps` module.
1. Het bestand openen `byline.html` om `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![Byline HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. Bijwerken `byline.html` de `data-cmp-data-layer` kenmerk:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   De waarde van `data-cmp-data-layer` is ingesteld op `"${byline.data}"` waar `byline` Dit is het verkoopmodel dat eerder is bijgewerkt. `.data` is de standaardnotatie voor het aanroepen van een Java Getter-methode in HTML van `getData()` die in de vorige exercitie zijn uitgevoerd.

1. Open een terminalvenster. Alleen de `ui.apps` met uw Maven-vaardigheden:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Ga terug naar de browser en open de pagina opnieuw met een component Byline: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. Open de ontwikkelaarsgereedschappen en inspecteer de HTML-bron van de pagina voor de Byline-component:

   ![Byline-gegevenslaag](assets/adobe-client-data-layer/byline-data-layer-html.png)

   U moet zien dat de `data-cmp-data-layer` is gevuld met de JSON-tekenreeks uit het Sling-model.

1. Open de ontwikkelaarsgereedschappen van de browser en voer de volgende opdracht in het dialoogvenster **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navigeren onder de reactie onder `component` om de instantie van het `byline` is toegevoegd aan de gegevenslaag:

   ![Bylinedeel van de gegevenslaag](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   U zou een ingang als het volgende moeten zien:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   Merk op dat de belichte eigenschappen dezelfde zijn als die zijn toegevoegd in de `HashMap` in het verkoopmodel.

## Een klikgebeurtenis toevoegen {#click-event}

De gegevenslaag van de Cliënt van de Adobe is gebeurtenis gedreven en één van de gemeenschappelijkste gebeurtenissen om een actie teweeg te brengen is `cmp:click` gebeurtenis. Met de AEM Core-componenten kunt u uw component eenvoudig registreren met behulp van het gegevenselement: `data-cmp-clickable`.

De klikbare elementen zijn gewoonlijk een knoop CTA of een navigatiekoppeling. Jammer genoeg heeft de component van de Byte geen van deze maar wij zullen het hoe dan ook registreren aangezien dit voor andere douanecomponenten zou kunnen gemeenschappelijk zijn.

1. Open de `ui.apps` module in uw winde
1. Het bestand openen `byline.html` om `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. Bijwerken `byline.html` de `data-cmp-clickable` kenmerk op de Byline **name** element:

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. Open een nieuwe terminal. Alleen de `ui.apps` met uw Maven-vaardigheden:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Ga terug naar de browser en open de pagina opnieuw met de component Byline toegevoegd: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   Als u onze gebeurtenis wilt testen, voegen we handmatig JavaScript toe met behulp van de ontwikkelaarsconsole. Zie [Het gebruiken van de Laag van Gegevens van de Cliënt van Adobe met AEM Componenten van de Kern](data-layer-overview.md) voor een video over hoe u dit kunt doen.

1. Open de ontwikkelaarsgereedschappen van de browser en voer de volgende methode in het dialoogvenster **Console**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   Deze eenvoudige methode moet de klik van de naam van de component Byline afhandelen.

1. Voer de volgende methode in het dialoogvenster **Console**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   De bovenstaande methode duwt een gebeurtenisluisteraar op de gegevenslaag om naar `cmp:click` gebeurtenis en roept de `bylineClickHandler`.

   >[!CAUTION]
   >
   > Het is belangrijk **niet** om de browser door deze oefening te vernieuwen, anders gaat de console JavaScript verloren.

1. In de browser, met de **Console** Open, klik de naam van de auteur in de component Byline:

   ![Byline-component geklikt](assets/adobe-client-data-layer/byline-component-clicked.png)

   U zou het consolebericht moeten zien `Byline Clicked!` en de naam van de Naamregel.

   De `cmp:click` De gebeurtenis is het eenvoudigst om in te sluiten. Voor complexere componenten en om ander gedrag te volgen is het mogelijk om douane javascript toe te voegen en nieuwe gebeurtenissen te registreren. Een goed voorbeeld is de Carousel-component, die een `cmp:show` gebeurtenis telkens wanneer een dia wordt geschakeld. Zie de [broncode voor meer informatie](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## Het hulpprogramma DataLayerBuilder gebruiken {#data-layer-builder}

Wanneer het verkoopmodel [bijgewerkt](#sling-model) eerder in het hoofdstuk hebben we ervoor gekozen de JSON-tekenreeks te maken met behulp van een `HashMap` en de eigenschappen handmatig in te stellen. Deze methode werkt prima voor kleine eenmalige componenten, maar voor componenten die de AEM Core Components uitbreiden, kan dit in veel extra code resulteren.

Een hulpprogrammaklasse, `DataLayerBuilder`, bestaat om het grootste deel van de zware hef uit te voeren. Hierdoor kunnen implementaties alleen de gewenste eigenschappen uitbreiden. Werk het verkoopmodel bij om de `DataLayerBuilder`.

1. Terugkeer aan winde en navigeer aan winde `core` module.
1. Het bestand openen `Byline.java` om `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. De `getData()` methode om een type van terug te keren `ComponentData`

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` is een object dat wordt geleverd door AEM Core Components. Het resulteert in een Koord JSON, enkel zoals in het vorige voorbeeld, maar voert ook veel extra werk uit.

1. Het bestand openen `BylineImpl.java` om `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. Voeg de volgende instructies voor importeren toe:

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. Vervang de `getData()` methode met het volgende:

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   De component Byline hergebruikt gedeelten van de component van de Kern van het Beeld om een beeld te tonen die de auteur vertegenwoordigen. In het bovenstaande fragment worden de [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) wordt gebruikt om de gegevenslaag van uit te breiden `Image` component. Hierdoor wordt het JSON-object vooraf gevuld met alle gegevens over de gebruikte afbeelding. Het doet ook enkele routinetaken zoals het plaatsen van `@type` en de unieke id van de component. De methode is erg klein.

   De enige eigenschap breidde de `withTitle` die wordt vervangen door de waarde van `getName()`.

1. Open een terminalvenster. Alleen de `core` met uw Maven-vaardigheden:

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. Terugkeren naar de IDE en de `byline.html` bestand onder `ui.apps`
1. HTML bijwerken voor gebruik `byline.data.json` om de `data-cmp-data-layer` kenmerk:

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   We retourneren nu een object van het type `ComponentData`. Dit object bevat een methode getter `getJson()` en dit wordt gebruikt om de `data-cmp-data-layer` kenmerk.

1. Open een terminalvenster. Alleen de `ui.apps` met uw Maven-vaardigheden:

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. Ga terug naar de browser en open de pagina opnieuw met de component Byline toegevoegd: [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. Open de ontwikkelaarsgereedschappen van de browser en voer de volgende opdracht in het dialoogvenster **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. Navigeren onder de reactie onder `component` om de instantie van het `byline` component:

   ![Byline-gegevenslaag bijgewerkt](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   U zou een ingang als het volgende moeten zien:

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   Let op: er is nu een `image` object binnen `byline` componentitem. Dit heeft veel meer informatie over de activa in de DAM. Let er ook op dat de `@type` en de unieke id (in dit geval `byline-136073cfcb`) automatisch zijn ingevuld, en `repo:modifyDate` die aangeeft wanneer de component is gewijzigd.

## Aanvullende voorbeelden {#additional-examples}

1. Een ander voorbeeld om de gegevenslaag uit te breiden kan worden bekeken door het inspecteren van `ImageList` component in de WKND-codebasis:
   * `ImageList.java` - Java-interface in de `core` module.
   * `ImageListImpl.java` - Verkoopmodel in het dialoogvenster `core` module.
   * `image-list.html` - HTML-sjabloon in het `ui.apps` module.

   >[!NOTE]
   >
   > Het is iets moeilijker om aangepaste eigenschappen op te nemen, zoals `occupation` wanneer u de [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). Als u echter een Core-component uitbreidt die een afbeelding of pagina bevat, bespaart het hulpprogramma veel tijd.

   >[!NOTE]
   >
   > Als het bouwen van een geavanceerde Laag van Gegevens voor voorwerpen die door een implementatie opnieuw worden gebruikt, wordt het geadviseerd om de elementen van de Laag van Gegevens in hun eigen gegevenslaag specifieke Voorwerpen van Java uit te pakken. Zo hebben de Componenten van de Kern van de Handel interfaces voor toegevoegd `ProductData` en `CategoryData` aangezien deze in het kader van een handelsuitvoering voor veel onderdelen kunnen worden gebruikt. Controleren [de code in de reactie aem-cif-core-components](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) voor meer informatie .

## Gefeliciteerd! {#congratulations}

U verkende enkel een paar manieren om de Laag van Gegevens van de Cliënt van de Adobe uit te breiden en aan te passen met AEM componenten!

## Aanvullende bronnen {#additional-resources}

* [Documentatie gegevenslaag Adobe-client](https://github.com/adobe/adobe-client-data-layer/wiki)
* [De Integratie van de Laag van gegevens met de Componenten van de Kern](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [Het gebruiken van de Laag van Gegevens van de Cliënt van Adobe en de Documentatie van de Componenten van de Kern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
