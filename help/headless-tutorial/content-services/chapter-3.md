---
title: Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen - Inhoudsservices
description: Hoofdstuk 3 van de AEM zelfstudie zonder koptekst behandelt het maken en ontwerpen van gebeurtenisinhoudfragmenten van het model voor inhoudsfragmenten dat in hoofdstuk 2 is gemaakt.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 167
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen

Hoofdstuk 3 van het AEM Hoofdloze leerprogramma behandelt het creëren van en het ontwerpen van de Fragmenten van de Inhoud van Gebeurtenissen van het Model van het Fragment van de Inhoud dat in [ Hoofdstuk 2 ](./chapter-2.md) wordt gecreeerd.

## Een gebeurtenisinhoudsfragment ontwerpen

Wanneer een [!DNL Event] Content Fragment Model (Content Fragment Model) is gemaakt en de AEM Configuration for WKND is toegepast op de map `/content/dam/wknd-mobile` Asset (via de eigenschap `cq:conf` ), kan een [!DNL Event] Content Fragment (Content Fragment) worden gemaakt.

Inhoudsfragmenten, een soort middel, moeten net als andere elementen in AEM Assets worden georganiseerd en beheerd.

* Gebruik locale mappen in de Assets-mapstructuur als vertaling vereist is (of kan zijn)
* Inhoudsfragmenten op logische wijze ordenen, zodat u ze gemakkelijk kunt vinden en beheren

Maak in deze stap een nieuwe [!DNL Event] for `Punkrock Fest` in de map met `/content/dam/wknd-mobile/en/events` elementen.

1. Navigeer naar **[!UICONTROL AEM]> [!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] >[!DNL English]** en maak mappen met elementen **[!DNL Events]** .
1. Maak binnen **[!UICONTROL Assets]> [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** een nieuw inhoudfragment van het type **[!DNL Event]** met de titel **[!DNL Punkrock Fest]** .
1. Maak het nieuwe [!DNL Event] inhoudsfragment.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;Ga een paar lijnen van beschrijving in...>**
   * [!DNL Event Date] : **&lt;Select a date in the future>**
   * [!DNL Event Type] : **Muziek**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **het Reptile House**
   * [!DNL Venue City] : **New York**

   Tik op **[!UICONTROL Save]** in de bovenste actiebalk om de wijzigingen op te slaan.

1. Gebruikend [ AEM de Manager van het Pakket ](http://localhost:4502/crx/packmgr/index.jsp), installeer hieronder het pakket op AEM Auteur. Dit pakket bevat een aantal gebeurtenisinhoudsfragmenten.

   [ krijgt Dossier: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## De JCR-structuur van het inhoudsfragment evalueren

*deze sectie is informationeel slechts en bedoeld om de onderliggende die structuur te socialiseren JCR van Inhoudsfragmenten van de Modellen van het Fragment van de Inhoud worden gemaakt.*

1. Open **[CRXDE Lite ](http://localhost:4502/crx/de/index.jsp)** op AEM Auteur.
1. In CRXDE Lite navigeert u in het hiërarchische menu links naar [ /content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content ](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) . Dit is het knooppunt dat het [!DNL Punkrock Fest] [!DNL Event] Content Fragment in de JCR vertegenwoordigt.
1. Breid de [ gegevens ](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) knoop uit.
Het overzicht in de **ruit van Eigenschappen** dat het een bezit `cq:model` heeft dat aan de [!DNL Event] modeldefinitie van het Fragment van de Inhoud richt.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Onder de `data` knoop selecteer de [ hoofd ](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) knoop en herzie de eigenschappen. Dit knooppunt bevat de inhoud die is verzameld tijdens het ontwerpen van een [!DNL Event] Content Fragment Model. De JCR-eigenschapsnamen komen overeen met de naam van de eigenschap van het Content Fragment Model en de waarden komen overeen met de geschreven waarden van het [!DNL Punkrock Fest] [!DNL Event] Content Fragment.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Volgende stap

Het wordt geadviseerd, installeert u [ com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip ](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket op AEM Auteur via [ AEM [!UICONTROL Package Manager] ](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 4 - Templates voor AEM inhoudsdiensten definiëren](./chapter-4.md)
