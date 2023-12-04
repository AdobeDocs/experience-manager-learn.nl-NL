---
title: Een doelaanroep laden en in werking stellen
description: Leer hoe te om, parameters tot paginaverzoek over te brengen, en een vraag van het Doel van uw plaatspagina in werking te stellen gebruikend een Regel van de Lancering. De informatie van de pagina wordt teruggewonnen en overgegaan als parameters gebruikend de Laag van de Gegevens van de Cliënt van de Adobe die u gegevens over de ervaring van bezoekers op een webpagina kunt verzamelen en opslaan en dan het gemakkelijk maken om tot deze gegevens toegang te hebben.
feature: Core Components, Adobe Client Data Layer
version: Cloud Service
jira: KT-6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
duration: 663
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Een doelaanroep laden en in werking stellen {#load-fire-target}

Leer hoe te om, parameters tot paginaverzoek over te brengen, en een vraag van het Doel van uw plaatspagina in werking te stellen gebruikend een Regel van de Lancering. De informatie van de Web-pagina wordt teruggewonnen en overgegaan als parameters gebruikend de Laag van de Gegevens van de Cliënt van de Adobe die u gegevens over de ervaring van bezoekers op een webpagina laat verzamelen en opslaan en dan het gemakkelijk maken om tot deze gegevens toegang te hebben.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regel bij laden van pagina

De gegevenslaag van de Cliënt van de Adobe is een gebeurtenis-gedreven gegevenslaag. Wanneer de gegevenslaag AEM pagina is geladen, wordt een gebeurtenis geactiveerd `cmp:show` . In de video worden de `Launch Library Loaded` regel wordt aangeroepen via een aangepaste gebeurtenis. Hieronder vindt u de codefragmenten die worden gebruikt in de video voor de aangepaste gebeurtenis en voor de gegevenselementen.

### Aangepaste weergegeven pagina-gebeurtenis{#page-event}

![Pagina weergegeven gebeurtenisconfiguratie en aangepaste code](assets/load-and-fire-target-call.png)

Voeg een nieuwe **Gebeurtenis** aan de **Regel**

+ __Extensie:__ Kern
+ __Type gebeurtenis:__ Aangepaste code
+ __Naam:__ Pagina weergeven, gebeurtenishandler (of iets beschrijends)

Tik op de knop __Editor openen__ en plak in het volgende codefragment. Deze code __moet__ worden toegevoegd aan __Gebeurtenisconfiguratie__ en daarna __Handeling__.

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

Een aangepaste functie definieert de `pageShownEventHandler`en luistert naar gebeurtenissen die door AEM Core Components worden uitgegeven, leidt de relevante informatie tot de Component van de Kern, verpakt het in een gebeurtenisvoorwerp, en brengt de Gebeurtenis van de Lancering met de afgeleide gebeurtenisinfo bij zijn lading teweeg.

De Launch-regel wordt geactiveerd via de Launch-functie `trigger(...)` functie die __alleen__ beschikbaar vanuit de definitie van het codefragment Aangepaste code van een regel.

De `trigger(...)` De functie neemt een gebeurtenisvoorwerp als parameter die beurtelings in de Elementen van Gegevens van de Lancering, door een andere gereserveerde naam in Lancering genoemd wordt blootgesteld `event`. Data Elements in Launch kan nu verwijzen naar gegevens van dit gebeurtenisobject uit het dialoogvenster `event` object gebruiken als syntaxis `event.component['someKey']`.

Indien `trigger(...)` wordt gebruikt buiten de context van het gebeurtenistype Aangepaste code van een gebeurtenis van het type van de Code van de Gebeurtenis (bijvoorbeeld, in een Actie), de fout JavaScript `trigger is undefined` wordt geworpen op de Website die met het bezit van de Lancering wordt geïntegreerd.


### Gegevenselementen

![Gegevenselementen](assets/data-elements.png)

Adobe gegevenselementen starten wijst de gegevens van het gebeurtenisobject toe [geactiveerd in de aangepaste gebeurtenis Page Shown](#page-event) aan variabelen beschikbaar in Adobe Target, via het Type van Gegevens van het Element van de Code van de uitbreiding van de Kern.

#### Pagina-ID-gegevenselement

```
if (event && event.id) {
    return event.id;
}
```

Deze code retourneert de unieke id van de Core Component genereren.

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

#### Foutbericht wanneer cookie niet is ingesteld

![Fout doelcookie](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### Oplossing

Doelklanten gebruiken soms cloudgebaseerde instanties met Target voor testdoeleinden of eenvoudige concepttest. Deze domeinen, en vele anderen, maken deel uit van de Openbare Lijst van het Achtervoegsel.
Moderne browsers slaan cookies niet op als u deze domeinen gebruikt, tenzij u de opties `cookieDomain` instellen met `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Volgende stappen

+ [Exportervaringsfragment naar Adobe Target](./export-experience-fragment-target.md)

## Ondersteunende koppelingen

+ [Documentatie over de gegevenslaag van de client Adoben](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud-foutopsporing - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Het gebruiken van de Laag van Gegevens van de Cliënt van de Adobe en de Documentatie van de Componenten van de Kern](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Inleiding tot het Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
