---
title: Lokale ontwikkelomgeving voor AEM als Cloud Service
description: Adobe Experience Manager (AEM) overzicht van de lokale ontwikkelomgeving.
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---


# Lokale ontwikkelomgeving instellen

Deze zelfstudie doorloopt het opzetten van een lokale ontwikkelomgeving voor Adobe Experience Manager (AEM) waarbij de AEM als Cloud Service SDK wordt gebruikt. Hieronder vindt u de ontwikkelingstool die nodig is voor het ontwikkelen, bouwen en compileren van AEM projecten, en lokale runtimes waarmee ontwikkelaars snel lokale nieuwe functies kunnen valideren voordat ze deze als Cloud Service via Adobe Cloud Manager implementeren.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM als Cloud Service Local Development Environment Technology Stack](./assets/overview/aem-sdk-technology-stack.png)

De lokale ontwikkelomgeving voor AEM kan worden opgesplitst in drie logische groepen:

+ Het __AEM Project__ bevat de aangepaste code, configuratie en inhoud die de aangepaste AEM toepassing is.
+ De __lokale AEM Runtime__ die een lokale versie van de auteur en van de Publicatie AEM plaatselijk in werking stelt.
+ De __lokale Dispatcher Runtime__ die een lokale versie van Apache HTTP Web Server en Dispatcher in werking stelt.

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

+ Installeren [!DNL Java]
+ Installeren [!DNL Node.js] (en npm)
+ Installeren [!DNL Maven]
+ Installeren [!DNL Git]

[Ontwikkelingshulpmiddelen instellen voor AEM projecten](./development-tools.md)

## Lokale AEM

De AEM als Cloud Service SDK verstrekt een [!DNL QuickStart Jar] die een lokale versie van AEM in werking stelt. U [!DNL QuickStart Jar] kunt de AEM-auteurservice of de AEM-publicatieservice lokaal uitvoeren. Let op: hoewel de software een lokale ontwikkelervaring [!DNL QuickStart Jar] biedt, worden niet alle functies die in AEM als Cloud Service beschikbaar zijn, opgenomen in de [!DNL QuickStart Jar]code.

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ Installeren [!DNL Java]
+ De AEM SDK downloaden
+ Voer de [!DNL AEM Author Service]
+ Voer de [!DNL AEM Publish Service]

[De lokale AEM-runtime instellen](./aem-runtime.md)

## Lokale [!DNL Dispatcher] runtime

AEM als Dispatcher Tools van een Cloud Service SDK verstrekt alles vereist om lokale [!DNL Dispatcher] runtime te opstelling. [!DNL Dispatcher] De hulpmiddelen zijn [!DNL Docker]-gebaseerd en verstrekt de hulpmiddelen van de bevellijn om de Server van het [!DNL Apache HTTP] Web en de [!DNL Dispatcher] configuratiedossiers in een compatibele formaten te transpileren en hen op te stellen om in de [!DNL Dispatcher] container te [!DNL Docker] lopen.

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ De AEM SDK downloaden
+ Installatieprogramma&#39; [!DNL Dispatcher] s
+ De lokale [!DNL Dispatcher] runtime uitvoeren

[De [!DNL Dispatcher] LocalRuntime instellen](./dispatcher-tools.md)
