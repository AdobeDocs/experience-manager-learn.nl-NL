---
title: Het gebruiken van de Laag van Gegevens van de Cliënt van Adobe met AEM Componenten van de Kern
description: De gegevenslaag van de Cliënt van Adobe introduceert een standaardmethode om gegevens over een bezoekerservaring op een webpagina te verzamelen en op te slaan en dan het gemakkelijk te maken om tot deze gegevens toegang te hebben. De gegevenslaag van de Cliënt van Adobe is platform agnostic, maar is volledig geïntegreerd in de Componenten van de Kern voor gebruik met AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---


# Het gebruiken van de Laag van Gegevens van de Cliënt van Adobe met AEM Componenten van de Kern {#overview}

De gegevenslaag van de Cliënt van Adobe introduceert een standaardmethode om gegevens over een bezoekerservaring op een webpagina te verzamelen en op te slaan en dan het gemakkelijk te maken om tot deze gegevens toegang te hebben. De gegevenslaag van de Cliënt van Adobe is platform agnostic, maar is volledig geïntegreerd in de Componenten van de Kern voor gebruik met AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Wilt u de Adobe Client Data Layer op uw AEM-site inschakelen? [Zie de instructies hier](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## De gegevenslaag verkennen

U kunt een idee van de ingebouwde functionaliteit van de Laag van Gegevens van de Cliënt van Adobe krijgen enkel door de ontwikkelaarshulpmiddelen van uw browser en levende [WKND verwijzingsplaats](https://wknd.site/) te gebruiken.

>[!NOTE]
>
> Hieronder worden screenshots genomen vanuit de Chrome-browser.

1. Navigeer naar [https://wknd.site](https://wknd.site)
1. Open uw ontwikkelaarshulpmiddelen en ga het volgende bevel in **Console** in:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect de reactie om de huidige staat van de gegevenslaag op een AEM plaats te zien. U moet informatie over de pagina en de afzonderlijke componenten bekijken.

   ![Reactie Adobe-gegevenslaag](assets/data-layer-state-response.png)

1. Duw een gegevensvoorwerp op de gegevenslaag door het volgende in de console in te gaan:

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. Voer de opdracht `adobeDataLayer.getState()` nogmaals uit en zoek de vermelding voor `training-data`.
1. Voeg vervolgens een padparameter toe om alleen de specifieke status van een component te retourneren:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Hiermee wordt slechts één componentgegevensitem geretourneerd](assets/return-just-single-component.png)

## Werken met gebeurtenissen

Het is aan te raden aangepaste code te activeren op basis van een gebeurtenis uit de gegevenslaag. Hierna kunt u het registreren en luisteren naar verschillende gebeurtenissen.

1. Ga de volgende helpermethode in uw console in:

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   De bovenstaande code inspecteert het object `event` en gebruikt de methode `adobeDataLayer.getState` om de huidige status op te halen van het object dat de gebeurtenis heeft geactiveerd. De helpermethode zal dan de `filter` criteria inspecteren en slechts als de stroom `dataObject` aan de filter voldoet zal het zijn teruggekeerd.

   >[!CAUTION]
   >
   > Het zal belangrijk **niet** zijn om browser door deze oefening te verfrissen, anders zal de console JavaScript worden verloren.

1. Daarna, ga een gebeurtenismanager in die zal worden geroepen wanneer een **Taser** component binnen een **Carousel** wordt getoond.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   De `teaserShownHandler` roept de methode `getDataObjectHelper` aan en geeft een filter van `wknd/components/teaser` door als `@type` om gebeurtenissen uit te filteren die door andere componenten worden teweeggebracht.

1. Vervolgens drukt u een gebeurtenislistener op de gegevenslaag om te luisteren naar de gebeurtenis `cmp:show`.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   De gebeurtenis `cmp:show` wordt teweeggebracht door vele verschillende componenten, zoals wanneer een nieuwe dia in **Carousel** wordt getoond of wanneer een nieuw lusje in **Tab** component wordt geselecteerd.

1. Schakel op de pagina de carrouseldia&#39;s in en bekijk de consoleinstructies:

   ![Carousel in-/uitschakelen en gebeurtenislistener bekijken](assets/teaser-console-slides.png)

1. Verwijder de gebeurtenislistener uit de gegevenslaag om te stoppen met luisteren naar de gebeurtenis `cmp:show`:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Ga terug naar de pagina en schakel de carrouseldia&#39;s in of uit. Merk op dat er niet meer instructies worden geregistreerd en dat er niet naar de gebeurtenis wordt geluisterd.

1. Voer vervolgens een gebeurtenishandler in die wordt aangeroepen wanneer de gebeurtenis Pagina weergegeven wordt geactiveerd:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Het middeltype `wknd/components/page` wordt gebruikt om de gebeurtenis te filteren.

1. Vervolgens drukt u een gebeurtenislistener op de gegevenslaag om naar de gebeurtenis `cmp:show` te luisteren en `pageShownHandler` aan te roepen.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. U zou onmiddellijk een consoleverklaring moeten zien die met de paginagegevens in brand wordt gestoken:

   ![Gegevens van paginaweergave](assets/page-show-console-data.png)

   De gebeurtenis `cmp:show` voor de pagina wordt geactiveerd bij elke pagina die helemaal boven aan de pagina wordt geladen. U zou kunnen vragen, waarom werd de gebeurtenismanager teweeggebracht, wanneer de pagina duidelijk reeds is geladen.

   Dit is één van de unieke eigenschappen van de Laag van Gegevens van de Cliënt van Adobe, in die zin dat u gebeurtenisluisteraars **before** of **after** kunt registreren de Laag van Gegevens is geïnitialiseerd. Dit is een essentieel element om rassenvoorwaarden te voorkomen.

   De gegevenslaag handhaaft een rijserie van alle gebeurtenissen die in opeenvolging zijn voorgekomen. De laag van Gegevens door gebrek zal gebeurteniscallbacks voor gebeurtenissen teweegbrengen die in **voorbij** evenals gebeurtenissen in **future** voorkwamen. Het is mogelijk de gebeurtenissen naar net voorbij of in de toekomst te filteren. [Meer informatie vindt u in de documentatie](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Volgende stappen

Bekijk de volgende zelfstudie om te leren hoe u de gebeurtenisgestuurde Adobe Client Data-laag kunt gebruiken om paginagegevens te verzamelen en naar Adobe Analytics](../analytics/collect-data-analytics.md) te verzenden.[

Of leer hoe te om [de Laag van Gegevens van de Cliënt van Adobe met AEM Componenten ](./data-layer-customize.md) aan te passen


## Aanvullende bronnen {#additional-resources}

* [Documentatie gegevenslaag Adobe-client](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Het gebruiken van de Laag van Gegevens van de Cliënt van Adobe en de Documentatie van de Componenten van de Kern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
