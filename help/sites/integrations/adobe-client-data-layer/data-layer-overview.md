---
title: Het gebruiken van de Laag van de Gegevens van de Cliënt van de Adobe met AEM Componenten van de Kern
description: De gegevenslaag van de Cliënt van de Adobe introduceert een standaardmethode om gegevens over de ervaring van een bezoeker op een webpagina te verzamelen en op te slaan en dan het gemakkelijk te maken om tot deze gegevens toegang te hebben. De gegevenslaag van de Cliënt van de Adobe is platform agnostic, maar is volledig geïntegreerd in de Componenten van de Kern voor gebruik met AEM.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
jira: KT-6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
doc-type: Tutorial
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 0%

---

# Het gebruiken van de Laag van de Gegevens van de Cliënt van de Adobe met AEM Componenten van de Kern {#overview}

De gegevenslaag van de Cliënt van de Adobe introduceert een standaardmethode om gegevens over de ervaring van een bezoeker op een webpagina te verzamelen en op te slaan en dan het gemakkelijk te maken om tot deze gegevens toegang te hebben. De gegevenslaag van de Cliënt van de Adobe is platform agnostic, maar is volledig geïntegreerd in de Componenten van de Kern voor gebruik met AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Wilt u de Laag van Gegevens van de Cliënt van de Adobe op uw AEM plaats toelaten? [Zie hier de instructies](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## De gegevenslaag verkennen

U kunt een idee van de ingebouwde functionaliteit van de Laag van Gegevens van de Cliënt van de Adobe enkel krijgen door de ontwikkelaarshulpmiddelen van uw browser en levende te gebruiken [WKND-referentiesite](https://wknd.site/us/en.html).

>[!NOTE]
>
> Hieronder worden screenshots genomen vanuit de Chrome-browser.

1. Navigeren naar [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Open de ontwikkelaarsgereedschappen en voer de volgende opdracht in het dialoogvenster **Console**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Om de huidige staat van de gegevenslaag op een AEM plaats te zien inspecteer de reactie. U moet informatie over de pagina en de afzonderlijke componenten bekijken.

   ![Reactie gegevenslaag Adoben](assets/data-layer-state-response.png)

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

1. De opdracht uitvoeren `adobeDataLayer.getState()` en zoek de informatie voor `training-data`.
1. Voeg vervolgens een padparameter toe om alleen de specifieke status van een component te retourneren:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![Hiermee wordt slechts één componentgegevensitem geretourneerd](assets/return-just-single-component.png)

## Werken met gebeurtenissen

Het is aan te raden om aangepaste code te activeren die is gebaseerd op een gebeurtenis uit de gegevenslaag. Hierna kunt u het registreren en luisteren naar verschillende gebeurtenissen.

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

   De bovenstaande code inspecteert de `event` object en gebruikt het `adobeDataLayer.getState` methode om de huidige status op te halen van het object dat de gebeurtenis heeft geactiveerd. Vervolgens wordt met de hulplijnmethode de `filter` en alleen als de huidige `dataObject` voldoet aan de filtercriteria die worden geretourneerd.

   >[!CAUTION]
   >
   > Het is belangrijk **niet** als u de browser gedurende deze oefening wilt vernieuwen, anders gaat de console-JavaScript verloren.

1. Voer vervolgens een gebeurtenishandler in die wordt aangeroepen wanneer een **Teaser** component wordt weergegeven binnen een **Carousel**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   De `teaserShownHandler` functie roept de functie `getDataObjectHelper` en geeft een filter door van `wknd/components/teaser` als de `@type` om gebeurtenissen uit te filteren die door andere componenten worden teweeggebracht.

1. Duw vervolgens een gebeurtenislistener op de gegevenslaag om te luisteren naar de `cmp:show` gebeurtenis.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   De `cmp:show` De gebeurtenis wordt geactiveerd door veel verschillende componenten, zoals wanneer een nieuwe dia wordt weergegeven in het dialoogvenster **Carousel** of wanneer een nieuw tabblad wordt geselecteerd in het dialoogvenster **Tab** component.

1. Schakel op de pagina de carrouseldia&#39;s in en bekijk de consoleinstructies:

   ![Carousel in-/uitschakelen en gebeurtenislistener bekijken](assets/teaser-console-slides.png)

1. Als u niet meer wilt luisteren naar de `cmp:show` gebeurtenis, de gebeurtenislistener verwijderen uit de gegevenslaag

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. Ga terug naar de pagina en schakel de carrouseldia&#39;s in of uit. Merk op dat er niet meer instructies worden geregistreerd en dat er niet naar de gebeurtenis wordt geluisterd.

1. Maak vervolgens een gebeurtenishandler die wordt aangeroepen wanneer de gebeurtenis page displayed wordt geactiveerd:

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   Bericht dat het middeltype `wknd/components/page` wordt gebruikt om de gebeurtenis te filteren.

1. Duw vervolgens een gebeurtenislistener op de gegevenslaag om te luisteren naar de `cmp:show` gebeurtenis, de `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. U zou onmiddellijk een consoleverklaring moeten zien die met de paginagegevens in brand wordt gestoken:

   ![Gegevens van paginaweergave](assets/page-show-console-data.png)

   De `cmp:show` gebeurtenis voor de pagina wordt geactiveerd bij elke pagina die boven aan de pagina wordt geladen. U zou kunnen vragen, waarom werd de gebeurtenismanager teweeggebracht, wanneer de pagina duidelijk reeds is geladen.

   Één van de unieke eigenschappen van de Laag van de Gegevens van de Cliënt van de Adobe is u gebeurtenisluisteraars kunt registreren **voor** of **na** Als de Laag van Gegevens is geïnitialiseerd, helpt het om de rasvoorwaarden te vermijden.

   De gegevenslaag handhaaft een rijserie van alle gebeurtenissen die in opeenvolging zijn voorgekomen. De Laag van Gegevens door gebrek zal gebeurteniscallbacks voor gebeurtenissen teweegbrengen die in **verleden** en gebeurtenissen in het **toekomst**. Het is mogelijk gebeurtenissen uit het verleden of de toekomst te filteren. [Meer informatie vindt u in de documentatie](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## Volgende stappen

Er zijn twee opties om het leren te houden, eerst om uit te checken [pagina-gegevens verzamelen en naar Adobe Analytics verzenden](../analytics/collect-data-analytics.md) zelfstudie die het gebruik van de laag Gegevens van de Cliënt van de Adobe aantoont. De tweede optie is: leren hoe te [De gegevenslaag van de Cliënt van de Adobe met AEM Componenten aanpassen](./data-layer-customize.md)


## Aanvullende bronnen {#additional-resources}

* [Documentatie over de gegevenslaag van de client Adoben](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Het gebruiken van de Laag van Gegevens van de Cliënt van de Adobe en de Documentatie van de Componenten van de Kern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
