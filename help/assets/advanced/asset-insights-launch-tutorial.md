---
title: Asset Insights instellen met AEM Assets en Adobe Launch
description: In deze videoreeks van 5 delen, lopen wij door de opstelling en de configuratie van de Inzichten van Activa voor Experience Manager die via Launch by Adobe wordt opgesteld.
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---

# Asset Insights instellen met AEM Assets en Adobe Experience Platform Launch

In deze videoreeks van 5 delen, lopen wij door de opstelling en de configuratie van de Inzichten van Activa voor Experience Manager die via de Lancering van de Adobe wordt opgesteld.

## Deel 1: Overzicht van informatie over elementen {#overview}

Overzicht van Asset Insights. Installeer Core Components, Sample Image Component en andere inhoudspakketten om uw omgeving gereed te maken.

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### Architectuurdiagram {#architecture-diagram}

![Architectuurdiagram](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Zorg ervoor dat u de [nieuwste versie van Core Components](https://github.com/adobe/aem-core-wcm-components) voor uw implementatie.

De video gebruikt Core Components v2.2.2 die niet langer de meest recente versie is. Gebruik de nieuwste versie voordat u verdergaat met de volgende sectie.

* Downloaden [Inhoud van voorbeeldafbeelding van asset Insights](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Downloaden [de nieuwste AEM WCM Core-componenten](https://github.com/adobe/aem-core-wcm-components/releases)

## Deel 2: Asset Insights Tracking inschakelen voor Sample Image Component {#sample-image-component-asset-insights}

Verbeteringen aan de componenten van de Kern en het gebruiken van volmachtscomponent (de Component van het Beeld van de Steekproef) voor de Inzichten van Activa. Het sjabloonbeleid voor de inhoudspagina bewerken om de voorbeeldafbeeldingscomponent in te schakelen voor de verwijzingssite.

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>De component Image Core bevat de mogelijkheid om UUID-tracking uit te schakelen door het bijhouden van de UUID van het element uit te schakelen (unieke id-waarde voor een knooppunt dat binnen JCR is gemaakt)

De component Core Image gebruikt ***data-asset-id*** attribuut binnen het bovenliggende element &lt;div> van een afbeeldingstag om deze functie in of uit te schakelen. De Component van de Volmacht treedt de kerncomponent met de volgende veranderingen met voeten.

* Hiermee verwijdert u de ***data-asset-id*** van de bovenliggende div van een  &lt;img> element in image.html
* Toevoegingen ***data-aem-asset-id*** rechtstreeks naar het  &lt;img>  element in image.html
* Toevoegingen ***data-trackable=&#39;true&#39;*** waarde voor het  &lt;img>  element in image.html
* ***data-aem-asset-id*** en ***data-trackable=&#39;true&#39;*** op hetzelfde knooppuntniveau worden gehouden

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* en *data-trackable=&#39;true&#39;* Dit zijn de belangrijkste kenmerken die aanwezig moeten zijn voor Asset Impressions. Voor Asset Click Insights moet de bovenliggende tag, naast de bovenstaande gegevenskenmerken in de  &lt;img> tag, een geldige href-waarde hebben.

## Deel 3: Adobe Analytics — Report Suite maken, gegevensverzameling in real-time en AEM Assets Reporting inschakelen {#adobe-analytics-asset-insights}

De reeks van het rapport met gegevensinzameling in real time wordt gecreeerd voor activa het volgen. AEM Assets Insights-configuratie wordt ingesteld met Adobe Analytics-referenties.

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
Voor de Adobe Analytics Report Suite moet het verzamelen van gegevens in real-time en het AEM van bedrijfsmiddelen zijn ingeschakeld. Het toelaten AEM de variabelen van de reserveanalyse van Activa het Rapporteren van Activa voor het volgen van activainzichten.

Voor de configuratie van AEM Assets Insights hebt u de volgende referenties nodig

* Datacenter
* Naam van analysebedrijf
* Gebruikersnaam voor Analytics
* Gedeeld geheim (kan worden verkregen van *Adobe Analytics > Admin > Company Settings > Web Service*).
* Rapportsuite (zorg dat u de juiste rapportsuite selecteert die wordt gebruikt voor Asset Reporting)

## Deel 4: Adobe Experience Platform Launch gebruiken om Adobe Analytics-extensie toe te voegen {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Adobe Analytics-extensie toevoegen, regels voor het laden van pagina&#39;s maken en AEM integreren met Starten met Adobe IMS-technische account.

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
Zorg ervoor dat u al uw wijzigingen van de auteur naar de publicatie-instantie dupliceert.

### Artikel 1: Paginanummering (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

Paginacontracker implementeert twee callbacks (geregistreerd in asset-embed-code)

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** : Wordt aangeroepen wanneer de gebeurtenis &#39;load&#39; wordt verzonden voor het element asset-DOM.
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** : Wordt aangeroepen wanneer de gebeurtenis &#39;click&#39; wordt verzonden voor het element asset-DOM-is dit alleen relevant wanneer het element asset-DOM-element een ankertag heeft als bovenliggend element met een geldig extern kenmerk &#39;href&#39;

Tot slot implementeert Pagetracker een initialisatiefunctie als.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : aangeroepen om de component Pagetracker te initialiseren. Dit DIENT te worden aangeroepen voordat een van de gebeurtenissen asset-inzichten-events (Impressions and/or Clicks) op de webpagina wordt gegenereerd.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : Accepteert optioneel een object AppMeasurement — indien opgegeven, wordt niet geprobeerd een nieuwe instantie van een object AppMeasurement te maken.

### Regel 2: Afbeeldingsbeheer — Actie 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### Regel 2: Afbeeldingsbeheer — Handeling 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded() : wordt aangeroepen tijdens het laden van de pagina en activeert Asset Impressions voor alle trackable images
* Analysevariabele die de geladen elementenlijst draagt: **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() : wordt aangeroepen wanneer het element element element element element element element element element element een ankertag met geldige href-waarde heeft. Wanneer op een element wordt geklikt, wordt een cookie gemaakt met de waarde van het aangeklikte element-id.**(Naam cookie: a.assets.click)**
* Analysevariabele die de geladen elementenlijst draagt: **contextData[&#39;c.a.assets.clickdid&#39;]**
* Oorsprong: **contextData[&quot;c.a.assets.source&quot;]**

### Foutopsporingsinstructies voor console {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

Twee Google Chrome-browserextensies worden in de video gebruikt als manieren om fouten op te sporen in Analytics. Soortgelijke extensies zijn ook beschikbaar voor andere browsers.

* [Chrome-extensie wijzigen starten](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

Het is ook mogelijk om van DTM op zuiveringswijze met de volgende Uitbreiding van Chrome over te schakelen: [Starten en DTM-switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). Hierdoor is het eenvoudiger om te zien of er fouten zijn met betrekking tot DTM-implementatie. Daarnaast kunt u handmatig van DTM overschakelen op de foutopsporingsmodus via elke browser *tools voor ontwikkelaars -> JS Console* door het volgende fragment toe te voegen:

## Deel 5: Testen van gegevens over Analytische tracking en Synchronisatie{#analytics-tracking-asset-insights}

Rapport AEM Asset Reporting Job Scheduler en Assets Insights-rapporten configureren

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
