---
title: Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen
seo-title: Aan de slag met AEM Content Services - Hoofdstuk 3 - Fragmenten voor gebeurtenisinhoud ontwerpen
description: Hoofdstuk 3 van de AEM zelfstudie zonder titel heeft betrekking op het maken en ontwerpen van gebeurtenisinhoudfragmenten van het model voor inhoudsfragmenten dat in hoofdstuk 2 is gemaakt.
seo-description: Hoofdstuk 3 van de AEM zelfstudie zonder titel heeft betrekking op het maken en ontwerpen van gebeurtenisinhoudfragmenten van het model voor inhoudsfragmenten dat in hoofdstuk 2 is gemaakt.
translation-type: tm+mt
source-git-commit: d7258f8acf6df680795ce61cc8383e60b5b7d722
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 0%

---


# Hoofdstuk 3 - Inhoudsfragmenten voor gebeurtenissen ontwerpen

Hoofdstuk 3 van de AEM zelfstudie zonder titel heeft betrekking op het maken en ontwerpen van Events Content Fragments van het Content Fragment Model dat is gemaakt in [Hoofdstuk 2](./chapter-2.md).

## Een gebeurtenisinhoudsfragment ontwerpen

Wanneer een [!DNL Event] inhoudsfragmentmodel is gemaakt en de AEM Configuratie voor WKND is toegepast op de map `/content/dam/wknd-mobile` Asset (via de `cq:conf` eigenschap), kan een [!DNL Event] inhoudsfragment worden gemaakt.

Inhoudsfragmenten, een soort middel, moeten net als andere elementen in AEM Assets worden georganiseerd en beheerd.

* Gebruik locale mappen in de mappenstructuur van Middelen als vertaling vereist is (of kan zijn)
* Inhoudsfragmenten op logische wijze ordenen, zodat u ze gemakkelijk kunt vinden en beheren

Maak in deze stap een nieuwe versie [!DNL Event] voor `Punkrock Fest` in de map `/content/dam/wknd-mobile/en/events` assets.

1. Navigeer naar **[!UICONTROL AEM]>[!UICONTROL Assets]>[!UICONTROL Files]>[!DNL WKND Mobile]>[!DNL English]** en maak middelenmappen **[!DNL Events]**.
1. Maak in **[!UICONTROL Assets]>[!UICONTROL Files]>[!DNL WKND Mobile]>[!DNL English]>[!DNL Events]** een nieuw inhoudsfragment van het type **[!DNL Event]** met de titel **[!DNL Punkrock Fest]**.
1. Maak het nieuwe [!DNL Event] inhoudsfragment.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;Voer een paar regels beschrijving in...>**
   * [!DNL Event Date] : **&lt;Selecteer een datum in de toekomst>**
   * [!DNL Event Type] : **Muziek**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **Reptile House**
   * [!DNL Venue City] : **New York**

   Tik **[!UICONTROL Save]** in de bovenste actiebalk om de wijzigingen op te slaan.

1. Installeer het onderstaande pakket met [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)op AEM Author. Dit pakket bevat een aantal gebeurtenisinhoudsfragmenten.

   [Bestand ophalen: GitHub > Middelen > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## De JCR-structuur van het inhoudsfragment evalueren

*Dit gedeelte is alleen ter informatie en is bedoeld om de onderliggende JCR-structuur van inhoudsfragmenten die zijn gemaakt op basis van modellen van inhoudsfragmenten te socialiseren.*

1. Open **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** op AEM-auteur.
1. Navigeer in CRXDE Lite op het linkerhiërarchiemenu naar [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) , het knooppunt dat het [!DNL Punkrock Fest] [!DNL Event] inhoudsfragment in het JCR vertegenwoordigt.
1. Vouw het [gegevensknooppunt](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) uit.
Controleer in het deelvenster **** Eigenschappen of de eigenschap verwijst naar de definitie van het `cq:model` [!DNL Event] inhoudsfragmentmodel.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Onder het `data` knooppunt selecteert u het [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) knooppunt en controleert u de eigenschappen. Dit knooppunt bevat de inhoud die is verzameld tijdens het ontwerpen van een [!DNL Event] inhoudsfragmentmodel. De JCR-eigenschapsnamen komen overeen met de namen van de eigenschappen van het Content Fragment Model en de waarden komen overeen met de geschreven waarden van het &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] inhoudsfragment.

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Volgende stap

U wordt aangeraden het inhoudspakket [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) te installeren op AEM Author via [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 4 - Templates voor AEM inhoudsdiensten definiëren](./chapter-4.md)
