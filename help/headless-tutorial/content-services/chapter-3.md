---
title: Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen - Inhoudsservices
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: Hoofdstuk 3 van de AEM zelfstudie zonder koptekst behandelt het maken en ontwerpen van gebeurtenisinhoudfragmenten van het model voor inhoudsfragmenten dat in hoofdstuk 2 is gemaakt.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 0%

---

# Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen

Hoofdstuk 3 van de AEM zelfstudie zonder kop behandelt het maken en ontwerpen van Events Content Fragments van het Content Fragment Model dat is gemaakt in [Hoofdstuk 2](./chapter-2.md).

## Een gebeurtenisinhoudsfragment ontwerpen

Met een [!DNL Event] Het gemaakte model van het Fragment van de inhoud en de Configuratie van de AEM voor WKND toegepast op `/content/dam/wknd-mobile` Map met middelen (via `cq:conf` eigenschap), a [!DNL Event] Inhoudsfragment kan worden gemaakt.

Inhoudsfragmenten, een soort middel, moeten net als andere elementen in AEM Assets worden georganiseerd en beheerd.

* Gebruik locale mappen in de mappenstructuur van Middelen als vertaling vereist is (of kan zijn)
* Inhoudsfragmenten op logische wijze ordenen, zodat u ze gemakkelijk kunt vinden en beheren

Maak in deze stap een nieuwe [!DNL Event] for `Punkrock Fest` in de `/content/dam/wknd-mobile/en/events` map met middelen.

1. Navigeren naar **[!UICONTROL AEM]> [!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] >[!DNL English]** en maak mappen met middelen **[!DNL Events]**.
1. Within **[!UICONTROL Assets]> [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** een nieuw inhoudfragment van type maken **[!DNL Event]** met een titel van **[!DNL Punkrock Fest]**.
1. Auteur van de nieuwe versie [!DNL Event] Inhoudsfragment.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Muziek**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **Reptile House**
   * [!DNL Venue City] : **New York**

   Tikken **[!UICONTROL Save]** in de bovenste actiebalk om wijzigingen op te slaan.

1. Gebruiken [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp), installeert u het onderstaande pakket op AEM Author. Dit pakket bevat een aantal gebeurtenisinhoudsfragmenten.

   [Bestand ophalen: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## De JCR-structuur van het inhoudsfragment evalueren

*Dit gedeelte is alleen ter informatie en is bedoeld om de onderliggende JCR-structuur van inhoudsfragmenten die zijn gemaakt op basis van modellen van inhoudsfragmenten te socialiseren.*

1. Openen **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** op AEM-auteur.
1. Navigeer in CRXDE Lite op het linkerhiërarchiemenu naar [/content/dam/wknd-mobile/nl/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) Dit is het knooppunt dat de [!DNL Punkrock Fest] [!DNL Event] Inhoudsfragment in de JCR.
1. Breid uit [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) knooppunt.
Herziening in de **Venster Eigenschappen** dat het een eigenschap heeft `cq:model` dat de [!DNL Event] Definitie van inhoudsfragmentmodel.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Onder de `data` knoop selecteert [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) en bekijk de eigenschappen. Dit knooppunt bevat de inhoud die tijdens het ontwerpen van een [!DNL Event] Inhoudsfragmentmodel. De JCR-eigenschapsnamen komen overeen met de namen van de eigenschappen van het Content Fragment Model en de waarden komen overeen met de geschreven waarden van de eigenschap &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Inhoudsfragment.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Volgende stap

U wordt aangeraden de [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket op AEM-auteur via [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 4 - Templates voor AEM inhoudsdiensten definiëren](./chapter-4.md)
