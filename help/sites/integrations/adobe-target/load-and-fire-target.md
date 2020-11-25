---
title: Een doelaanroep laden en in werking stellen
description: Leer hoe te om, parameters tot paginaverzoek over te brengen, en een vraag van het Doel van uw plaatspagina in werking te stellen gebruikend een Regel van de Lancering. De informatie van de pagina wordt teruggewonnen en overgegaan als parameters gebruikend de Laag van Gegevens van de Cliënt van de Adobe die u gegevens over de ervaring van bezoekers op een webpagina kunt verzamelen en opslaan en dan het gemakkelijk maken om tot deze gegevens toegang te hebben.
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# Een doelaanroep laden en in werking stellen {#load-fire-target}

Leer hoe te om, parameters tot paginaverzoek over te brengen, en een vraag van het Doel van uw plaatspagina in werking te stellen gebruikend een Regel van de Lancering. De informatie van de Web-pagina wordt teruggewonnen en overgegaan als parameters gebruikend de Laag van Gegevens van de Cliënt van de Adobe die u gegevens over de ervaring van bezoekers op een webpagina laat verzamelen en opslaan en dan het gemakkelijk maken om tot deze gegevens toegang te hebben.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regel bij laden van pagina

De gegevenslaag van de Cliënt van Adobe is een gebeurtenis gedreven gegevenslaag. Wanneer de gegevenslaag AEM pagina wordt geladen, wordt een gebeurtenis geactiveerd `cmp:show` . In de video wordt de `Launch Library Loaded` regel aangeroepen met behulp van een aangepaste gebeurtenis. Hieronder vindt u de codefragmenten die worden gebruikt in de video voor de aangepaste gebeurtenis en voor de gegevenselementen.

### Aangepaste weergegeven pagina-gebeurtenis{#page-event}

![Pagina weergegeven gebeurtenisconfiguratie en aangepaste code](assets/load-and-fire-target-call.png)

In het bezit van de Lancering, voeg een nieuwe **Gebeurtenis** aan de **Regel toe**

+ __Extensie:__ Kern
+ __Type gebeurtenis:__ Aangepaste code
+ __Naam:__ Pagina weergeven, gebeurtenishandler (of iets beschrijends)

Tik op de knop __Editor__ openen en plak in het volgende codefragment. Deze code __moet__ worden toegevoegd aan de Configuratie __van de__ Gebeurtenis en een verdere __Actie__.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Een douanefunctie bepaalt `pageShownEventHandler`, en luistert naar gebeurtenissen die door AEM Componenten van de Kern worden uitgegeven, leidt de relevante informatie tot de Component van de Kern af, verpakt het in een gebeurtenisvoorwerp, en brengt de Gebeurtenis van de Lancering met afgeleide gebeurtenisinfo bij zijn lading teweeg.

De regel van de Lancering wordt teweeggebracht gebruikend de `trigger(...)` functie van de Lancering die __slechts__ van binnen de de codefragmentdefinitie van de Code van de Gebeurtenis van de Regel van de Douane beschikbaar is.

De `trigger(...)` functie neemt een gebeurtenisvoorwerp als parameter die beurtelings in de Elementen van Gegevens van de Lancering, door een andere gereserveerde naam in Launch genoemd wordt blootgesteld `event`. Data Elements in Launch kan nu met dezelfde syntaxis verwijzen naar gegevens van dit gebeurtenisobject van het `event` object `event.component['someKey']`.

Als `trigger(...)` buiten de context van het gebeurtenistype van de Code van de Douane van een Gebeurtenis (bijvoorbeeld, in een Actie) wordt gebruikt, `trigger is undefined` wordt de fout JavaScript geworpen op de Website die met het bezit van de Lancering wordt geïntegreerd.


### Gegevenselementen

![Gegevenselementen](assets/data-elements.png)

Adobe De Elementen van Gegevens van de Lancering van de brengen de gegevens van het gebeurtenisvoorwerp in [teweeggebracht in de douane Pagina getoonde gebeurtenis](#page-event) aan variabelen beschikbaar in Adobe Target, via het Type van het Element van de Gegevens van de Code van de uitbreiding van de Kern in kaart.

#### Pagina-ID-gegevenselement

```
if (event && event.id) {
    return event.id;
}
```

Deze code keert de genereert unieke identiteitskaart van de Component van de Kern terug.

![Pagina-id](assets/pageid.png)

### Gegevenselement paginapad

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Deze code retourneert het pad van de AEM pagina.

![Paginapad](assets/pagepath.png)

### Gegevenselement paginatitel

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Deze code retourneert de titel van de AEM pagina.

![Paginatitel](assets/pagetitle.png)

## Problemen oplossen

### Waarom worden mijn dozen niet op mijn webpagina&#39;s geactiveerd?

#### Foutbericht wanneer mboxDisable cookie niet is ingesteld**

![Fout doelcookie](assets/target-cookie-error.png)

#### Oplossing

Doelklanten gebruiken soms cloudgebaseerde instanties met Target voor testdoeleinden of eenvoudige concepttest. Deze domeinen, en vele anderen, maken deel uit van de Openbare Lijst van het Achtervoegsel.
In moderne browsers worden cookies niet opgeslagen als u deze domeinen gebruikt, tenzij u de `cookieDomain` instelling aanpast met `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Volgende stappen

+ [Exportervaringsfragment naar Adobe Target](./export-experience-fragment-target.md)

## Ondersteunende koppelingen

+ [Documentatie gegevenslaag Adobe-client](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Het gebruiken van de Laag van Gegevens van de Cliënt van Adobe en de Documentatie van de Componenten van de Kern](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Inleiding tot de Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)