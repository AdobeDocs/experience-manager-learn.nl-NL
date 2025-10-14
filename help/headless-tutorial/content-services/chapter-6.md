---
title: Hoofdstuk 6 - De inhoud op AEM Publish beschikbaar maken als JSON - Content Services
description: Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud zich op AEM Publish bevinden om het gebruik van de mobiele app mogelijk te maken.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 196
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 0%

---

# Hoofdstuk 6 - De inhoud op AEM Publish beschikbaar maken voor levering

Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud zich op AEM Publish bevinden om het gebruik door de mobiele app mogelijk te maken.

## De inhoud voor AEM Content Services publiceren

De configuratie en de inhoud die zijn gemaakt om de gebeurtenissen te sturen via AEM Content Services moeten worden gepubliceerd naar AEM Publish zodat de Mobile App er toegang toe heeft.

Omdat AEM Content Services is samengesteld op basis van Configuratie (Content Fragment Models, Editable Templates), Assets (Content Fragments, Images) en Pagina&#39;s, genieten al deze onderdelen automatisch van AEM mogelijkheden voor inhoudsbeheer, waaronder:

* Workflow voor revisie en verwerking
* en activering/deactivatie voor het duwen en trekken van inhoud van de AEM Publish AEM Content Services eindpunten

1. Verzeker de **[!DNL WKND Mobile]Pakketten van de Toepassing**, die in [&#x200B; Hoofdstuk 1 &#x200B;](./chapter-1.md#wknd-mobile-application-packages) worden vermeld, geïnstalleerd op **AEM Publish** gebruikend [!UICONTROL Package Manager].
   * [&#x200B; http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publish the **[!DNL WKND Mobile Events API]Editable Template**
   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Tools] > [!UICONTROL General] > [!UICONTROL Templates] >[!DNL WKND Mobile]**
   1. Selecteer de sjabloon **[!DNL Event API]**
   1. Tik **[!UICONTROL Publish]** op de bovenste actiebalk
   1. Publish het **malplaatje** en **alle verwijzingen** (inhoudsbeleid, inhoudspolitieke afbeeldingen, en malplaatjes)

1. Publish de **[!DNL WKND Mobile Events]inhoudsfragmenten** .

   Dit is vereist omdat de API voor gebeurtenissen de component Lijst met inhoudsfragmenten gebruikt, die niet specifiek verwijst naar inhoudsfragmenten.

   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Alle **[!DNL Event]** inhoudsfragmenten selecteren
   1. Tik op de **[!UICONTROL Manage Publication]** in de bovenste actiebalk
   1. Verlaat standaard **Publish** actie zoals-is, tikken **[!UICONTROL Next]** in de hoogste actiebar
   1. Selecteer **alle** inhoudsfragmenten
   1. Tik **[!UICONTROL Publish]** op de bovenste actiebalk
      * *het [!DNL Events] Model van het Fragment van de Inhoud en de beelden van de verwijzingenGebeurtenis zullen automatisch samen met de inhoudsfragmenten worden gepubliceerd.*

1. Publish de **[!DNL Events API]pagina**.
   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selecteer de pagina **[!DNL Events]**
   1. Tik op de **[!UICONTROL Manage Publication]** in de bovenste actiebalk
   1. Verlaat standaard **Publish** actie zoals-is, tikken **[!UICONTROL Next]** in de hoogste actiebar
   1. Selecteer de pagina **[!DNL Events]**
   1. Tik **[!DNL Publish]** op de bovenste actiebalk

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## AEM Publish verifiëren

1. In nieuwe browser van het Web, zorg ervoor u uit AEM Publish wordt geregistreerd en verzoek volgende URLs (die `http://localhost:4503` voor om het even welke gastheer vervangt:haven AEM Publish loopt).

   * [&#x200B; http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   Deze verzoeken moeten dezelfde JSON-reactie retourneren als wanneer de overeenkomstige eindpunten van AEM auteur werden herzien. Als dat niet het geval is, zorgt u ervoor dat alle publicaties zijn geslaagd (controleer de Replication-wachtrijen), wordt het [!DNL WKND Mobile] `ui.apps` -pakket geïnstalleerd op AEM Publish en controleert u de `error.log` for AEM Publish.

## Volgende stap

Er zijn geen extra pakketten om te installeren. Zorg ervoor dat de inhoud en configuratie die in deze sectie worden geschetst naar AEM Publish wordt gepubliceerd, anders werken volgende hoofdstukken niet.

* [Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken](./chapter-7.md)
