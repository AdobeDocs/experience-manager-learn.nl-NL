---
title: Lokale ontwikkelomgeving voor AEM als Cloud Service
description: Adobe Experience Manager (AEM) overzicht van de lokale ontwikkelomgeving.
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---


# Lokale ontwikkelomgeving instellen

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Overzicht"
>abstract="De vestiging lokale ontwikkelomgeving voor AEM als Cloud Service omvat ontwikkelingshulpmiddelen die worden vereist om AEM Projecten te ontwikkelen, te bouwen en te compileren, evenals lokale runtime die ontwikkelaars toestaat om nieuwe eigenschappen plaatselijk snel te bevestigen alvorens hen aan AEM als Cloud Service via de Manager van de Wolk van Adobe te opstellen."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Richtlijnen voor ontwikkeling"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Grondbeginselen van ontwikkeling"

Deze zelfstudie doorloopt het opzetten van een lokale ontwikkelomgeving voor Adobe Experience Manager (AEM) waarbij de AEM als Cloud Service SDK wordt gebruikt. Hieronder vindt u de ontwikkelingstool die nodig is voor het ontwikkelen, bouwen en compileren van AEM projecten, en lokale runtimes waarmee ontwikkelaars snel lokale nieuwe functies kunnen valideren voordat ze deze als Cloud Service via Adobe Cloud Manager implementeren.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM als Cloud Service Local Development Environment Technology Stack](./assets/overview/aem-sdk-technology-stack.png)

De lokale ontwikkelomgeving voor AEM kan worden opgesplitst in drie logische groepen:

+ Het __AEM Project__ bevat de douanecode, de configuratie en de inhoud die de AEM toepassing van de douane is.
+ De __Lokale AEM Runtime__ die een lokale versie van AEM de Auteur en de Publicatie diensten plaatselijk in werking stelt.
+ De __Local Dispatcher Runtime__ die een lokale versie van Apache HTTP Web Server en Dispatcher uitvoert.

Dit leerprogramma analyseert hoe te om de benadrukte punten in het bovengenoemde diagram te installeren en te plaatsen, die een stabiele lokale ontwikkelomgeving voor AEM ontwikkeling verstrekken.

## Bestandssysteemorganisatie

In deze zelfstudie werd de locatie van de AEM als Cloud Service-SDK-artefacten en AEM projectcode als volgt vastgesteld:

+ `~/aem-sdk` is een organisatorische omslag die de diverse hulpmiddelen bevat die door de AEM als Cloud Service SDK worden verstrekt
+ `~/aem-sdk/author` bevat de AEM-auteurservice
+ `~/aem-sdk/publish` bevat de AEM-publicatieservice
+ `~/aem-sdk/dispatcher` bevat de Dispatcher-gereedschappen
+ `~/code/<project name>` bevat de aangepaste AEM-broncode van het project

Merk op dat `~` verkort voor de Folder van de Gebruiker is. In Windows is dit het equivalent van `%HOMEPATH%`;

## Ontwikkelingsinstrumenten voor AEM projecten

Het AEM project is de basis van de douanecode die de code, de configuratie en de inhoud bevat die via de Manager van de Wolk aan AEM als Cloud Service wordt opgesteld. De basislijnprojectstructuur wordt gegenereerd via het [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype).

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ [!DNL Java] installeren
+ [!DNL Node.js] (en npm) installeren
+ [!DNL Maven] installeren
+ [!DNL Git] installeren

[Ontwikkelingshulpmiddelen instellen voor AEM projecten](./development-tools.md)

## Lokale AEM

De AEM als Cloud Service SDK verstrekt [!DNL QuickStart Jar] die een lokale versie van AEM in werking stelt. Met de [!DNL QuickStart Jar] kunt u de AEM-auteurservice of de AEM-publicatieservice lokaal uitvoeren. Hoewel [!DNL QuickStart Jar] een lokale ontwikkelervaring biedt, worden niet alle functies die in AEM als Cloud Service beschikbaar zijn, opgenomen in [!DNL QuickStart Jar].

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ [!DNL Java] installeren
+ De AEM SDK downloaden
+ [!DNL AEM Author Service] uitvoeren
+ [!DNL AEM Publish Service] uitvoeren

[De lokale AEM-runtime instellen](./aem-runtime.md)

## Lokale [!DNL Dispatcher] Runtime

AEM als Dispatcher Tools van een Cloud Service SDK verstrekt alles vereist om lokale [!DNL Dispatcher] runtime te opstelling. [!DNL Dispatcher] De hulpmiddelen zijn  [!DNL Docker]-gebaseerd en verstrekt de hulpmiddelen van de bevellijn om de Server en de  [!DNL Apache HTTP] configuratiedossiers van het  [!DNL Dispatcher] Web in een compatibele formaten te transpileren en hen op te stellen aan  [!DNL Dispatcher] lopend in de  [!DNL Docker] container.

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ De AEM SDK downloaden
+ [!DNL Dispatcher]-gereedschappen installeren
+ De lokale [!DNL Dispatcher]-runtime uitvoeren

[De lokale [!DNL Dispatcher] runtime instellen](./dispatcher-tools.md)
