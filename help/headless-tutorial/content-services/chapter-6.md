---
title: Hoofdstuk 6 - De inhoud in AEM publiceren beschikbaar maken als JSON - Content Services
description: Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud op AEM-publicatie staan om consumptie vanaf de mobiele app mogelijk te maken.
feature: '"Inhoudsfragmenten, API''s"'
topic: '"Headless, Content Management"'
role: Developer
level: Begin
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 1%

---


# Hoofdstuk 6 - De inhoud beschikbaar maken op AEM-publicaties voor levering

Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud op AEM-publicatie staan om consumptie door de mobiele app mogelijk te maken.

## De inhoud voor AEM Content Services publiceren

De configuratie en inhoud die zijn gemaakt om de gebeurtenissen te sturen via AEM Content Services, moeten worden gepubliceerd naar AEM Publish, zodat de Mobile App er toegang toe heeft.

Omdat AEM Content Services is samengesteld uit Configuration (Content Fragment Models, Editable Templates), Assets (Content Fragments, Images) en Pages, genieten al deze onderdelen automatisch van AEM mogelijkheden voor inhoudsbeheer, waaronder:

* Workflow voor revisie en verwerking
* en activering/deactivering voor het doordrukken en ophalen van inhoud uit de eindpunten van de AEM Content Services van AEM Publish

1. Zorg ervoor dat **[!DNL WKND Mobile]Toepassingspakketten**, vermeld in [Hoofdstuk 1](./chapter-1.md#wknd-mobile-application-packages), zijn geïnstalleerd op **AEM-publicatie** met [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. De bewerkbare sjabloon **[!DNL WKND Mobile Events API]publiceren**
   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Tools] > [!UICONTROL General] > [!UICONTROL Templates] >[!DNL WKND Mobile]**
   1. Selecteer de sjabloon **[!DNL Event API]**
   1. Tik **[!UICONTROL Publish]** in de bovenste actiebalk
   1. Publiceer **malplaatje** en **alle verwijzingen** (inhoudsbeleid, inhoudspolitieke afbeeldingen, en malplaatjes)

1. Publiceer de **[!DNL WKND Mobile Events]inhoudsfragmenten**.

Dit is vereist omdat de API voor gebeurtenissen de component Lijst met inhoudsfragmenten gebruikt, die niet specifiek verwijst naar inhoudsfragmenten.
1. Navigeer naar **[!UICONTROL AEM]> [!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1. Alle **[!DNL Event]**-inhoudsfragmenten selecteren
1. Tik op **[!UICONTROL Manage Publication]** in de bovenste actiebalk
1. Als u de standaardactie **Publiceren** ongewijzigd laat, tikt u op **[!UICONTROL Next]** in de bovenste actiebalk
1. **alle** inhoudsfragmenten selecteren
1. Tik **[!UICONTROL Publish]** in de bovenste actiebalk
* *Het [!DNL Events] Content Fragment Model and references Event Images will automatically be publish together with the content fragments.*

1. Publiceer **[!DNL Events API]pagina**.
   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. De pagina **[!DNL Events]** selecteren
   1. Tik op **[!UICONTROL Manage Publication]** in de bovenste actiebalk
   1. Als u de standaardactie **Publiceren** ongewijzigd laat, tikt u op **[!UICONTROL Next]** in de bovenste actiebalk
   1. De pagina **[!DNL Events]** selecteren
   1. Tik **[!DNL Publish]** in de bovenste actiebalk

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Publiceren van AEM controleren

1. In een nieuwe browser van het Web, zorg ervoor u uit AEM wordt geregistreerd publiceer en verzoek volgende URLs (die `http://localhost:4503` voor om het even welke gastheer vervangen:havenAEM publiceert).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Deze verzoeken moeten dezelfde JSON-reactie retourneren als wanneer de overeenkomstige eindpunten van de AEM-auteur werden herzien. Als dat niet het geval is, zorgt u ervoor dat alle publicaties zijn geslaagd (controleer de Replication-wachtrijen), wordt het [!DNL WKND Mobile] `ui.apps`-pakket geïnstalleerd in AEM Publish en controleert u `error.log` voor AEM Publish.

## Volgende stap

Er zijn geen extra pakketten om te installeren. Zorg ervoor dat de inhoud en configuratie die in deze sectie worden beschreven naar publiceren AEM wordt gepubliceerd, anders werken volgende hoofdstukken niet.

* [Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken](./chapter-7.md)
