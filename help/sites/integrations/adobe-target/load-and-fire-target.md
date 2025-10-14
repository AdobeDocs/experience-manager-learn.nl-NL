---
title: Een doelaanroep laden en in werking stellen
description: Leer hoe te om te laden, parameters tot paginaverzoek over te gaan, en een vraag van het Doel van uw plaatspagina in werking te stellen gebruikend een etikettenRegel.
feature: Core Components, Adobe Client Data Layer
version: Experience Manager as a Cloud Service
jira: KT-6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
duration: 588
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---

# Een doelaanroep laden en in werking stellen {#load-fire-target}

Leer hoe te om te laden, parameters tot paginaverzoek over te gaan, en een vraag van het Doel van uw plaatspagina in werking te stellen gebruikend een etikettenRegel. De informatie van de Web-pagina wordt teruggewonnen en overgegaan als parameters gebruikend de Laag van Gegevens van de Cliënt van Adobe die u gegevens over de ervaring van bezoekers op een webpagina laat verzamelen en opslaan en dan het gemakkelijk maken om tot deze gegevens toegang te hebben.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## Regel bij laden van pagina

De gegevenslaag van de Adobe-client is een gebeurtenisgestuurde gegevenslaag. Wanneer de gegevenslaag AEM Page is geladen, wordt een gebeurtenis `cmp:show` geactiveerd. In de video wordt de regel `tags Library Loaded` aangeroepen met behulp van een aangepaste gebeurtenis. Hieronder vindt u de codefragmenten die worden gebruikt in de video voor de aangepaste gebeurtenis en voor de gegevenselementen.

### Aangepaste weergegeven pagina-gebeurtenis{#page-event}

![&#x200B; Pagina getoonde gebeurtenisconfiguratie en douanecode &#x200B;](assets/load-and-fire-target-call.png)

In het markeringsbezit, voeg een nieuwe **Gebeurtenis** aan de **Regel** toe

+ __Uitbreiding:__ Kern
+ __Type van Gebeurtenis:__ de Code van de Douane
+ __Naam:__ de Handler van de Gebeurtenis van de Show van de Pagina (of iets beschrijvend)

Tik de __Open knoop van de Redacteur__ en deeg in het volgende codefragment. Deze code __moet__ aan de __Configuratie van de Gebeurtenis__ en een verdere __Actie__ worden toegevoegd.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the tags Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        // Trigger the tags Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other tags data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Data Collection, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

Een douanefunctie bepaalt `pageShownEventHandler`, en luistert naar gebeurtenissen die door de Componenten van de Kern van AEM worden uitgegeven, leidt de relevante informatie tot de Component van de Kern af, verpakt het in een gebeurtenisvoorwerp, en activeert de etikettenGebeurtenis met de afgeleide gebeurtenisinfo bij zijn lading.

De markeringsregel wordt teweeggebracht gebruikend de functie van de markeringen `trigger(...)` die __slechts__ beschikbaar van binnen de de codefragmentdefinitie van de Code van de Gebeurtenis van een Regel is.

De functie `trigger(...)` neemt een gebeurtenisobject als een parameter die op zijn beurt weer wordt weergegeven in de labels Data Elements, door een andere gereserveerde naam in de tags `event` . Data Elements in tags kunnen nu met behulp van syntaxis als `event.component['someKey']` verwijzen naar gegevens van dit gebeurtenisobject van het `event` -object.

Als `trigger(...)` buiten de context van het gebeurtenistype van de Code van de Douane van een Gebeurtenis (bijvoorbeeld, in een Actie) wordt gebruikt, wordt de fout van JavaScript `trigger is undefined` geworpen op de Website die met het markeringsbezit wordt geïntegreerd.


### Gegevenselementen

![&#x200B; Elementen van Gegevens &#x200B;](assets/data-elements.png)

De Elementen van de Gegevens van markeringen brengen de gegevens van het gebeurtenisvoorwerp [&#x200B; in de douanePagina in kaart getoonde gebeurtenis &#x200B;](#page-event) aan variabelen beschikbaar in Adobe Target, via het Type van het Element van de Gegevens van de Code van de uitbreiding van de Kern.

#### Pagina-ID-gegevenselement

```
if (event && event.id) {
    return event.id;
}
```

Deze code retourneert de unieke id van de Core Component genereren.

![&#x200B; identiteitskaart van de Pagina &#x200B;](assets/pageid.png)

### Gegevenselement paginapad

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

Deze code retourneert het pad van de AEM-pagina.

![&#x200B; Pad van de Pagina &#x200B;](assets/pagepath.png)

### Gegevenselement paginatitel

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

Deze code retourneert de titel van de AEM-pagina.

![&#x200B; Titel van de Pagina &#x200B;](assets/pagetitle.png)

## Problemen oplossen

### Waarom worden mijn dozen niet op mijn webpagina&#39;s geactiveerd?

#### Foutbericht wanneer cookie niet is ingesteld

![&#x200B; Fout van het Domein van de Koekjestaal van het Doel &#x200B;](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### Oplossing

Doelklanten gebruiken soms cloudgebaseerde instanties met Target voor testdoeleinden of eenvoudige concepttest. Deze domeinen, en vele anderen, maken deel uit van de Openbare Lijst van het Achtervoegsel.
In moderne browsers worden cookies niet opgeslagen als u deze domeinen gebruikt, tenzij u de instelling `cookieDomain` aanpast met `targetGlobalSettings()` .

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## Volgende stappen

+ [Exportervaringsfragment naar Adobe Target](./export-experience-fragment-target.md)

## Ondersteunende koppelingen

+ [&#x200B; de Documentatie van de Laag van Gegevens van de Cliënt van Adobe &#x200B;](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [&#x200B; Foutopsporing van Adobe Experience Cloud - Chrome &#x200B;](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [&#x200B; Gebruikend de Laag van Gegevens van de Cliënt van Adobe en de Documentatie van de Componenten van de Kern &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=nl-NL)
+ [&#x200B; Inleiding aan Adobe Experience Platform Debugger &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=nl-NL)
