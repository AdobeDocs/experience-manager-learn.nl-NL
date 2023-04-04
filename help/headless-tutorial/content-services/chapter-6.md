---
title: Hoofdstuk 6 - De inhoud in AEM publiceren beschikbaar maken als JSON - Content Services
description: Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud op AEM-publicatie staan om consumptie vanaf de mobiele app mogelijk te maken.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 1%

---

# Hoofdstuk 6 - De inhoud beschikbaar maken op AEM-publicaties voor levering

Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud op AEM-publicatie staan om consumptie door de mobiele app mogelijk te maken.

## De inhoud voor AEM Content Services publiceren

De configuratie en inhoud die zijn gemaakt om de gebeurtenissen te sturen via AEM Content Services, moeten worden gepubliceerd naar AEM Publish, zodat de Mobile App er toegang toe heeft.

Omdat AEM Content Services is samengesteld uit Configuration (Content Fragment Models, Editable Templates), Assets (Content Fragments, Images) en Pages, genieten al deze onderdelen automatisch van AEM mogelijkheden voor inhoudsbeheer, waaronder:

* Workflow voor revisie en verwerking
* en activering/deactivering voor het doordrukken en ophalen van inhoud uit de eindpunten van de AEM Content Services van AEM Publish

1. Zorg ervoor dat de **[!DNL WKND Mobile]Toepassingspakketten**, vermeld in [Hoofdstuk 1](./chapter-1.md#wknd-mobile-application-packages), worden geïnstalleerd op **AEM-publicatie** gebruiken [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publiceer de **[!DNL WKND Mobile Events API]Bewerkbare sjabloon**
   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Tools] > [!UICONTROL General] > [!UICONTROL Templates] >[!DNL WKND Mobile]**
   1. Selecteer **[!DNL Event API]** template
   1. Tikken **[!UICONTROL Publish]** in de bovenste actiebalk
   1. Publiceer de **template** en **alle verwijzingen** (inhoudsbeleid, inhoudspolitieke toewijzingen en sjablonen)

1. Publiceer de **[!DNL WKND Mobile Events]inhoudsfragmenten**.

   Dit is vereist omdat de API voor gebeurtenissen de component Lijst met inhoudsfragmenten gebruikt, die niet specifiek verwijst naar inhoudsfragmenten.

   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Alle opties selecteren **[!DNL Event]** inhoudsfragmenten
   1. Tik op de knop **[!UICONTROL Manage Publication]** in de bovenste actiebalk
   1. De standaardinstelling behouden **Publiceren** handeling ongewijzigd, tikken **[!UICONTROL Next]** in de bovenste actiebalk
   1. Selecteren **alles** inhoudsfragmenten
   1. Tikken **[!UICONTROL Publish]** in de bovenste actiebalk
      * *De [!DNL Events] Content Fragment Model and references Event Images will automatically be published with the content fragments.*

1. Publiceer de **[!DNL Events API]page**.
   1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selecteer **[!DNL Events]** page
   1. Tik op de knop **[!UICONTROL Manage Publication]** in de bovenste actiebalk
   1. De standaardinstelling behouden **Publiceren** handeling ongewijzigd, tikken **[!UICONTROL Next]** in de bovenste actiebalk
   1. Selecteer **[!DNL Events]** page
   1. Tikken **[!DNL Publish]** in de bovenste actiebalk

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## Publiceren van AEM controleren

1. In een nieuwe browser van het Web, zorg ervoor u uit Publiceren AEM wordt geregistreerd en verzoek om de volgende URLs (substituerend `http://localhost:4503` voor elke host (AEM Publish van poort ingeschakeld).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   Deze verzoeken moeten dezelfde JSON-reactie retourneren als wanneer de overeenkomstige eindpunten van de AEM-auteur werden herzien. Als dat niet het geval is, zorgt u ervoor dat alle publicaties zijn geslaagd (controleer de Replication-wachtrijen). [!DNL WKND Mobile] `ui.apps` is geïnstalleerd op de AEM-publicatie en bekijkt de `error.log` voor AEM-publicatie.

## Volgende stap

Er zijn geen extra pakketten om te installeren. Zorg ervoor dat de inhoud en configuratie die in deze sectie worden beschreven naar publiceren AEM wordt gepubliceerd, anders werken volgende hoofdstukken niet.

* [Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken](./chapter-7.md)
