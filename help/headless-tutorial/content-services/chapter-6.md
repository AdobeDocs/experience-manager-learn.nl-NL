---
title: Hoofdstuk 6 - De inhoud voor AEM publiceren beschikbaar maken als JSON - Content Services
description: Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud op AEM Publiceren staan zodat gebruikers van de mobiele app mogelijk zijn.
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

# Hoofdstuk 6 - De inhoud beschikbaar maken bij AEM publiceren voor levering

Hoofdstuk 6 van de AEM zelfstudie zonder koppen omvat de verzekering dat alle benodigde pakketten, configuratie en inhoud op AEM Publiceren staan om consumptie door de mobiele app mogelijk te maken.

## De inhoud voor AEM Content Services publiceren

De configuratie en de inhoud die zijn gemaakt om de gebeurtenissen te sturen via AEM Content Services moeten worden gepubliceerd om te AEM publiceren zodat de Mobile App er toegang toe heeft.

Omdat AEM Content Services is samengesteld uit Configuration (Content Fragment Models, Editable Templates), Assets (Content Fragments, Images) en Pages, genieten al deze onderdelen automatisch van AEM mogelijkheden voor inhoudsbeheer, waaronder:

* Workflow voor revisie en verwerking
* en activering/deactivering voor het duwen en trekken van inhoud van de eindpunten van de AEM Content Services van de AEM publiceren

1. Zorg ervoor dat **[!DNL WKND Mobile]Toepassingspakketten**, vermeld in [Hoofdstuk 1](./chapter-1.md#wknd-mobile-application-packages), worden geïnstalleerd op **AEM publiceren** gebruiken [!UICONTROL Package Manager].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publiceer de **[!DNL WKND Mobile Events API]Bewerkbare sjabloon**
   1. Navigeren naar **[!UICONTROL AEM]> [!UICONTROL Tools] > [!UICONTROL General] > [!UICONTROL Templates] >[!DNL WKND Mobile]**
   1. Selecteer de **[!DNL Event API]** template
   1. Tikken **[!UICONTROL Publish]** in de bovenste actiebalk
   1. Publiceer de **template** en **alle verwijzingen** (inhoudsbeleid, inhoudspolitieke toewijzingen en sjablonen)

1. Publiceer de **[!DNL WKND Mobile Events]inhoudsfragmenten**.

   Dit is vereist omdat de API voor gebeurtenissen de component Lijst met inhoudsfragmenten gebruikt, die niet specifiek verwijst naar inhoudsfragmenten.

   1. Navigeren naar **[!UICONTROL AEM]> [!UICONTROL Assets] > [!UICONTROL Files] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. Alle opties selecteren **[!DNL Event]** inhoudsfragmenten
   1. Tik op de knop **[!UICONTROL Manage Publication]** in de bovenste actiebalk
   1. De standaardinstelling behouden **Publiceren** handeling ongewijzigd, tikken **[!UICONTROL Next]** in de bovenste actiebalk
   1. Selecteren **alles** inhoudsfragmenten
   1. Tikken **[!UICONTROL Publish]** in de bovenste actiebalk
      * *De [!DNL Events] Content Fragment Model and references Event Images will automatically be published with the content fragments.*

1. Publiceer de **[!DNL Events API]page**.
   1. Navigeren naar **[!UICONTROL AEM]> [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. Selecteer de **[!DNL Events]** page
   1. Tik op de knop **[!UICONTROL Manage Publication]** in de bovenste actiebalk
   1. De standaardinstelling behouden **Publiceren** handeling ongewijzigd, tikken **[!UICONTROL Next]** in de bovenste actiebalk
   1. Selecteer de **[!DNL Events]** page
   1. Tikken **[!DNL Publish]** in de bovenste actiebalk

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## AEM publiceren verifiëren

1. In een nieuwe browser van het Web, zorg ervoor u uit AEM wordt geregistreerd publiceren en om de volgende URLs (vervangend `http://localhost:4503` voor elke host (poort AEM Publish wordt uitgevoerd).

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   Deze verzoeken moeten dezelfde JSON-reactie retourneren als wanneer de overeenkomstige eindpunten van AEM auteur werden herzien. Als dat niet het geval is, zorgt u ervoor dat alle publicaties zijn geslaagd (controleer de Replication-wachtrijen). [!DNL WKND Mobile] `ui.apps` is geïnstalleerd op AEM Publiceren en bekijk de `error.log` voor AEM Publiceren.

## Volgende stap

Er zijn geen extra pakketten om te installeren. Zorg ervoor dat de inhoud en configuratie die in deze sectie worden beschreven wordt gepubliceerd om te AEM publiceren, anders zullen de verdere hoofdstukken niet werken.

* [Hoofdstuk 7 - AEM inhoudsservices van een mobiele app gebruiken](./chapter-7.md)
