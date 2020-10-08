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
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 0%

---


# Een doelaanroep laden en in werking stellen {#load-fire-target}

Leer hoe te om, parameters tot paginaverzoek over te brengen, en een vraag van het Doel van uw plaatspagina in werking te stellen gebruikend een Regel van de Lancering. De informatie van de pagina wordt teruggewonnen en overgegaan als parameters gebruikend de Laag van Gegevens van de Cliënt van de Adobe die u gegevens over de ervaring van bezoekers op een webpagina kunt verzamelen en opslaan en dan het gemakkelijk maken om tot deze gegevens toegang te hebben.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regel bij laden van pagina

De gegevenslaag van de Cliënt van Adobe is een gebeurtenis gedreven gegevenslaag. Wanneer de gegevenslaag AEM pagina wordt geladen, wordt een gebeurtenis geactiveerd `cmp:show` . In de video wordt de `Launch Library Loaded` regel aangeroepen met behulp van een aangepaste gebeurtenis. Hieronder vindt u de codefragmenten die worden gebruikt in de video voor de aangepaste gebeurtenis en voor de gegevenselementen.

### Aangepaste gebeurtenis

Het onderstaande codefragment voegt een gebeurtenislistener toe door een functie naar de gegevenslaag te duwen. Wanneer de `cmp:show` gebeurtenis wordt geactiveerd, wordt de `pageShownEventHandler` functie aangeroepen. In deze functie worden enkele controles van de hygiëne toegevoegd en wordt een nieuwe laag samengesteld met de meest recente status van de gegevenslaag voor de component die de gebeurtenis heeft geactiveerd. `dataObject`

Daarna `trigger(dataObject)` wordt het genoemd. `trigger()` is een gereserveerde naam in Launch en activeert &quot;de Launch-regel&quot;. We geven het gebeurtenisobject door als een parameter die vervolgens weer wordt vrijgegeven door een andere gereserveerde naam in de gebeurtenis met de naam Launch. Data Elements in Launch kan nu verwijzen naar verschillende eigenschappen, zoals: `event.component['someKey']`.

```javascript
var pageShownEventHandler = function(evt) {
// defensive coding to avoid a null pointer exception
if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
   //trigger Launch Rule and pass event
   console.debug("cmp:show event: " + evt.eventInfo.path);
   var event = {
      //include the id of the component that triggered the event
      id: evt.eventInfo.path,
      //get the state of the component that triggered the event
      component: window.adobeDataLayer.getState(evt.eventInfo.path)
   };

      //Trigger the Launch Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
      // i.e `event.component['someKey']`
      trigger(event);
   }
}

//set the namespace to avoid a potential race condition
window.adobeDataLayer = window.adobeDataLayer || [];
//push the event listener for cmp:show into the data layer
window.adobeDataLayer.push(function (dl) {
   //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
   dl.addEventListener("cmp:show", pageShownEventHandler);
});
```

### Pagina-id gegevenslaag

```
if(event && event.id) {
    return event.id;
}
```

![Pagina-id](assets/pageid.png)

### Pad naar pagina

```
if(event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

![Paginapad](assets/pagepath.png)

### Paginatitel

```
if(event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

![Paginatitel](assets/pagetitle.png)

### Vaak voorkomende problemen

#### Waarom worden mijn dozen niet op mijn webpagina&#39;s geactiveerd?

**Foutbericht wanneer cookie niet is ingesteld**

![Fout doelcookie](assets/target-cookie-error.png)

**Oplossing**

Doelklanten gebruiken soms cloudgebaseerde instanties met Target voor testdoeleinden of eenvoudige concepttest. Deze domeinen, en vele anderen, maken deel uit van de Openbare Lijst van het Achtervoegsel.
In moderne browsers worden cookies niet opgeslagen als u deze domeinen gebruikt, tenzij u de `cookieDomain` instelling aanpast met `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain 
};
```

## Volgende stappen

1. [Exportervaringsfragment naar Adobe Target](./export-experience-fragment-target.md)

## Ondersteunende koppelingen

* [Documentatie gegevenslaag Adobe-client](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
* [Het gebruiken van de Laag van Gegevens van de Cliënt van Adobe en de Documentatie van de Componenten van de Kern](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
* [Inleiding tot de Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)