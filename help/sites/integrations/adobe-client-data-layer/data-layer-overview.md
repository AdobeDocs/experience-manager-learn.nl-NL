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
duration: 777
source-git-commit: dc40b8e022477d2b1d8f0ffe3b5e8bcf13be30b3
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# Het gebruiken van de Laag van de Gegevens van de Cliënt van de Adobe met AEM Componenten van de Kern {#overview}

De gegevenslaag van de Cliënt van de Adobe introduceert een standaardmethode om gegevens over de ervaring van een bezoeker op een webpagina te verzamelen en op te slaan en dan het gemakkelijk te maken om tot deze gegevens toegang te hebben. De gegevenslaag van de Cliënt van de Adobe is platform agnostic, maar is volledig geïntegreerd in de Componenten van de Kern voor gebruik met AEM.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> Wilt u de Laag van Gegevens van de Cliënt van de Adobe op uw AEM plaats toelaten? [ zie hier de instructies ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## De gegevenslaag verkennen

U kunt een idee van de ingebouwde functionaliteit van de Laag van Gegevens van de Cliënt van de Adobe enkel krijgen door de ontwikkelaarshulpmiddelen van uw browser en de levende [ WKND verwijzingsplaats ](https://wknd.site/us/en.html) te gebruiken.

>[!NOTE]
>
> Hieronder worden screenshots genomen vanuit de Chrome-browser.

1. Navigeer aan [ https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. Open uw ontwikkelaarshulpmiddelen en ga het volgende bevel in de **Console** in:

   ```js
   window.adobeDataLayer.getState();
   ```

   Om de huidige staat van de gegevenslaag op een AEM plaats te zien inspecteer de reactie. U moet informatie over de pagina en de afzonderlijke componenten bekijken.

   ![ Reactie van de Laag van Gegevens van de Adobe ](assets/data-layer-state-response.png)

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

1. Voer de opdracht `adobeDataLayer.getState()` nogmaals uit en zoek de vermelding voor `training-data` .
1. Voeg vervolgens een padparameter toe om alleen de specifieke status van een component te retourneren:

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![ terugkeer enkel één enkele ingang van componentengegevens ](assets/return-just-single-component.png)

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

   De bovenstaande code inspecteert het object `event` en gebruikt de methode `adobeDataLayer.getState` om de huidige status op te halen van het object dat de gebeurtenis heeft geactiveerd. Vervolgens controleert de hulpmethode de `filter` en alleen als de huidige `dataObject` voldoet aan de filtercriteria die worden geretourneerd.

   >[!CAUTION]
   >
   > Het is belangrijk **niet** browser door deze oefening te verfrissen, anders wordt de console JavaScript verloren.

1. Daarna, ga een gebeurtenismanager in die wordt geroepen wanneer de component van het a **Taser** binnen a **Carrousel** wordt getoond.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/carousel/item"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   De functie `teaserShownHandler` roept de functie `getDataObjectHelper` aan en geeft een filter van `wknd/components/carousel/item` als `@type` door om gebeurtenissen uit te filteren die door andere componenten worden geactiveerd.

1. Vervolgens drukt u een gebeurtenislistener op de gegevenslaag om naar de gebeurtenis `cmp:show` te luisteren.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   De `cmp:show` gebeurtenis wordt teweeggebracht door vele verschillende componenten, als wanneer een nieuwe dia in **Carousel** wordt getoond, of wanneer een nieuw lusje in de **4&rbrace; component van het Lusje &lbrace;wordt geselecteerd.**

1. Schakel op de pagina de carrouseldia&#39;s in en bekijk de consoleinstructies:

   ![ Knevel van de knevel en zie gebeurtenisluisteraar ](assets/teaser-console-slides.png)

1. Als u niet meer wilt luisteren naar de gebeurtenis `cmp:show` , verwijdert u de gebeurtenislistener uit de gegevenslaag

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

   Het middeltype `wknd/components/page` wordt gebruikt om de gebeurtenis te filteren.

1. Vervolgens drukt u een gebeurtenislistener op de gegevenslaag om naar de gebeurtenis `cmp:show` te luisteren en `pageShownHandler` aan te roepen.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. U zou onmiddellijk een consoleverklaring moeten zien die met de paginagegevens in brand wordt gestoken:

   ![ Pagina toont gegevens ](assets/page-show-console-data.png)

   De gebeurtenis `cmp:show` voor de pagina wordt geactiveerd bij elke pagina die boven aan de pagina wordt geladen. U zou kunnen vragen, waarom werd de gebeurtenismanager teweeggebracht, wanneer de pagina duidelijk reeds is geladen.

   Één van de unieke eigenschappen van de Laag van de Gegevens van de Cliënt van de Adobe is u gebeurtenisluisteraars **vóór** of **kunt registreren nadat** de Laag van Gegevens is geïnitialiseerd, helpt het om de rasvoorwaarden te vermijden.

   De gegevenslaag handhaaft een rijserie van alle gebeurtenissen die in opeenvolging zijn voorgekomen. De Laag van Gegevens door gebrek zal gebeurteniscallbacks voor gebeurtenissen teweegbrengen die in **voorbij** en gebeurtenissen in de **toekomst** voorkwamen. Het is mogelijk gebeurtenissen uit het verleden of de toekomst te filteren. [ Meer informatie kan in de documentatie ](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener) worden gevonden.


## Volgende stappen

Er zijn twee opties om het leren te houden, eerste, controle uit [ paginagegegevens verzamelen en het verzenden naar Adobe Analytics ](../analytics/collect-data-analytics.md) leerprogramma dat het gebruik van de laag van de Gegevens van de Cliënt van de Adobe aantoont. De tweede optie is, om te leren hoe te [ de Laag van de Gegevens van de Cliënt van de Adobe met AEM Componenten ](./data-layer-customize.md) aanpassen


## Aanvullende bronnen {#additional-resources}

* [ de Documentatie van de Laag van Gegevens van de Cliënt van de Adobe ](https://github.com/adobe/adobe-client-data-layer/wiki)
* [ Gebruikend de Laag van Gegevens van de Cliënt van de Adobe en de Documentatie van de Componenten van de Kern ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
