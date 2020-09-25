---
title: Aan de slag met AEM headless - Hoofdstuk 6 - De inhoud in AEM publiceren beschikbaar maken als JSON
description: Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud op AEM-publicatie staan om consumptie vanaf de mobiele app mogelijk te maken.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---


# Hoofdstuk 6 - De inhoud beschikbaar maken op AEM-publicaties voor levering

Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud op AEM-publicatie staan om consumptie door de mobiele app mogelijk te maken.

## De inhoud voor AEM Content Services publiceren

De configuratie en inhoud die zijn gemaakt om de gebeurtenissen te sturen via AEM Content Services, moeten worden gepubliceerd naar AEM Publish, zodat de Mobile App er toegang toe heeft.

Omdat AEM Content Services is samengesteld uit Configuration (Content Fragment Models, Editable Templates), Assets (Content Fragments, Images) en Pages, genieten al deze onderdelen automatisch van AEM mogelijkheden voor inhoudsbeheer, waaronder:

* Workflow voor revisie en verwerking
* en activering/deactivering voor het doordrukken en ophalen van inhoud uit de eindpunten van de AEM Content Services van AEM Publish

1. Zorg ervoor dat de **[!DNL WKND Mobile]toepassingspakketten**, die in [hoofdstuk 1](./chapter-1.md#wknd-mobile-application-packages)worden vermeld, zijn geïnstalleerd op de **AEM-publicatie** met behulp van [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. De **[!DNL WKND Mobile Events API]bewerkbare sjabloon publiceren**
   1. Ga naar **[!UICONTROL AEM]>[!UICONTROL Tools]>[!UICONTROL General]>[!UICONTROL Templates]>[!DNL WKND Mobile]**
   1. Select the **[!DNL Event API]** template
   1. Tik **[!UICONTROL Publish]** in de bovenste actiebalk
   1. Publiceer het **malplaatje** en **alle verwijzingen** (inhoudsbeleid, inhoudspolitieke afbeeldingen, en malplaatjes)

1. Publiceer de **[!DNL WKND Mobile Events]inhoudsfragmenten**.

Dit is vereist omdat de API voor gebeurtenissen de component Lijst met inhoudsfragmenten gebruikt, die niet specifiek verwijst naar inhoudsfragmenten.
1. Ga naar **[!UICONTROL AEM]>[!UICONTROL Assets]>[!UICONTROL Files]>[!DNL WKND Mobile]>[!DNL English]>[!DNL Events]** 1. Selecteer alle **[!DNL Event]** inhoudsfragmenten1. Tik op de knop **[!UICONTROL Manage Publication]** in de bovenste actiebalk1. Als u de standaardactie **Publiceren** ongewijzigd laat, tikt u op **[!UICONTROL Next]** de bovenste actiebalk1. Selecteer **alle** inhoudsfragmenten1. Tik **[!UICONTROL Publish]** op de bovenste actiebalk* *Het[!DNL Events]inhoudsfragmentmodel en de verwijzingen naar gebeurtenisafbeeldingen worden automatisch samen met de inhoudsfragmenten gepubliceerd.*

1. Publiceer de **[!DNL Events API]pagina**.
   1. Ga naar **[!UICONTROL AEM]>[!UICONTROL Sites]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**
   1. Selecteer de **[!DNL Events]** pagina
   1. Tik op de knop **[!UICONTROL Manage Publication]** in de bovenste actiebalk
   1. Tik op de bovenste actiebalk terwijl u de standaardactie **Publiceren** **[!UICONTROL Next]** ongewijzigd laat
   1. Selecteer de **[!DNL Events]** pagina
   1. Tik **[!DNL Publish]** in de bovenste actiebalk

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## Publiceren van AEM controleren

1. In nieuwe browser van het Web, zorg ervoor u van AEM wordt het programma geopend publiceer en verzoek volgende URLs (substituerend `http://localhost:4503` voor wat gastheer:havenAEM publiceert loopt).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Deze verzoeken moeten dezelfde JSON-reactie retourneren als wanneer de overeenkomstige eindpunten van de AEM-auteur werden herzien. Als dat niet het geval is, zorgt u ervoor dat alle publicaties zijn geslaagd (controleer de Replication-wachtrijen), wordt het [!DNL WKND Mobile] pakket geïnstalleerd in AEM Publish en controleert u het `ui.apps` `error.log` voor AEM Publish.

## Volgende stap

Er zijn geen extra pakketten om te installeren. Zorg ervoor dat de inhoud en configuratie die in deze sectie worden beschreven naar publiceren AEM wordt gepubliceerd, anders werken volgende hoofdstukken niet.

* [Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken](./chapter-7.md)
