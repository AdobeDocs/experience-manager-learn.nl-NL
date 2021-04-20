---
title: Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen - Inhoudsservices
seo-title: Aan de slag met AEM Content Services - Hoofdstuk 3 - Fragmenten voor gebeurtenisinhoud ontwerpen
description: Hoofdstuk 3 van de AEM zelfstudie zonder koptekst behandelt het maken en ontwerpen van gebeurtenisinhoudfragmenten van het model voor inhoudsfragmenten dat in hoofdstuk 2 is gemaakt.
seo-description: Hoofdstuk 3 van de AEM zelfstudie zonder koptekst behandelt het maken en ontwerpen van gebeurtenisinhoudfragmenten van het model voor inhoudsfragmenten dat in hoofdstuk 2 is gemaakt.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---


# Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen

Hoofdstuk 3 van de AEM zelfstudie zonder titel heeft betrekking op het maken en ontwerpen van Events Content Fragments van het Content Fragment Model dat is gemaakt in [Hoofdstuk 2](./chapter-2.md).

## Een gebeurtenisinhoudsfragment ontwerpen

Als een [!DNL Event] Model van het Fragment van de Inhoud wordt gecreeerd en de Configuratie van de AEM voor WKND wordt toegepast op `/content/dam/wknd-mobile` omslag van Activa (via het `cq:conf` bezit), kan een [!DNL Event] Inhoudsfragment worden gecreeerd.

Inhoudsfragmenten, een soort middel, moeten net als andere elementen in AEM Assets worden georganiseerd en beheerd.

* Gebruik locale mappen in de mappenstructuur van Middelen als vertaling vereist is (of kan zijn)
* Inhoudsfragmenten op logische wijze ordenen, zodat u ze gemakkelijk kunt vinden en beheren

Maak in deze stap een nieuwe [!DNL Event] voor `Punkrock Fest` in de map `/content/dam/wknd-mobile/en/events` assets.

1. Navigeer naar **[!UICONTROL AEM]> [!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] >[!DNL English]** en maak Asset folders **[!DNL Events]**.
1. Maak binnen **[!UICONTROL Assets]> [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** een nieuw inhoudsfragment van het type **[!DNL Event]** met de titel **[!DNL Punkrock Fest]**.
1. Maak het nieuwe [!DNL Event]-inhoudsfragment.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] :  **Muziek**
   * [!DNL Ticket Price] :  **10**
   * [!DNL Event Image] :  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] :  **Reptile House**
   * [!DNL Venue City] :  **New York**

   Tik op **[!UICONTROL Save]** in de bovenste actiebalk om de wijzigingen op te slaan.

1. Installeer het onderstaande pakket met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp) op AEM Author. Dit pakket bevat een aantal gebeurtenisinhoudsfragmenten.

   [Bestand ophalen: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## De JCR-structuur van het inhoudsfragment evalueren

*Dit gedeelte is alleen ter informatie en is bedoeld om de onderliggende JCR-structuur van inhoudsfragmenten die zijn gemaakt op basis van modellen van inhoudsfragmenten te socialiseren.*

1. Open **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** op AEM-auteur.
1. Navigeer in CRXDE Lite op het linkerhiërarchiemenu naar [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content). Dit is het knooppunt dat het [!DNL Punkrock Fest] [!DNL Event] Content Fragment in de JCR vertegenwoordigt.
1. Vouw het knooppunt [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) uit.
Controleer in het **deelvenster Eigenschappen** of er een eigenschap `cq:model` is die naar de [!DNL Event]-definitie van het inhoudsfragmentmodel verwijst.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Onder de `data` knoop selecteer [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) knoop en herzie de eigenschappen. Dit knooppunt bevat de inhoud die is verzameld tijdens het ontwerpen van een [!DNL Event]-inhoudsfragmentmodel. De JCR-eigenschapnamen komen overeen met de naam van de eigenschap van het inhoudsfragmentmodel en de waarden komen overeen met de geschreven waarden van het &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Inhoudsfragment.

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Volgende stap

U wordt aangeraden het inhoudspakket [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) op AEM Author te installeren via [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 4 - Templates voor AEM inhoudsdiensten definiëren](./chapter-4.md)
