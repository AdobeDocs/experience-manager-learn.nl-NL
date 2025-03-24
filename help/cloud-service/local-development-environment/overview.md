---
title: Lokale ontwikkelomgeving voor AEM as a Cloud Service
description: Overzicht van de lokale ontwikkelomgeving van Adobe Experience Manager (AEM).
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Lokale ontwikkelomgeving instellen {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Overzicht"
>abstract="Het instellen van een lokale ontwikkelomgeving voor AEM as a Cloud Service omvat ontwikkelingshulpmiddelen die nodig zijn voor het ontwikkelen, bouwen en compileren van AEM-projecten, en lokale runtimes waarmee ontwikkelaars snel lokale nieuwe functies kunnen valideren voordat ze deze via Adobe Cloud Manager naar AEM as a Cloud Service implementeren."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Richtlijnen voor ontwikkeling"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Grondbeginselen van ontwikkeling"

In deze zelfstudie wordt het opzetten van een lokale ontwikkelomgeving voor Adobe Experience Manager (AEM) met AEM as a Cloud Service SDK besproken. Hieronder vindt u de ontwikkelingstool die nodig is voor het ontwikkelen, bouwen en compileren van AEM-projecten, en lokale runtimes waarmee ontwikkelaars nieuwe functies snel lokaal kunnen valideren voordat ze deze via Adobe Cloud Manager naar AEM as a Cloud Service implementeren.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![ AEM as a Cloud Service de Stapel van de Technologie van de Milieu van de Lokale Ontwikkeling ](./assets/overview/aem-sdk-technology-stack.png)

De lokale ontwikkelomgeving voor AEM kan worden opgesplitst in drie logische groepen:

+ Het __Project van AEM__ bevat de douanecode, de configuratie en de inhoud die de toepassing van douaneAEM is.
+ De __Lokale Runtime van AEM__ die een lokale versie van de Auteur van AEM in werking stelt en de diensten plaatselijk publiceert.
+ De __Lokale Runtime van Dispatcher__ die een lokale versie van de Server en Dispatcher van het Web van Apache HTTP in werking stelt.

In deze zelfstudie wordt uitgelegd hoe u de gemarkeerde items in het bovenstaande diagram kunt installeren en instellen en wordt een stabiele lokale ontwikkelomgeving voor AEM-ontwikkeling geboden.

## Bestandssysteemorganisatie

In deze zelfstudie werden de locatie van de AEM as a Cloud Service SDK-artefacten en AEM Project-code als volgt vastgesteld:

+ `~/aem-sdk` is een organisatiemap met de verschillende tools van de AEM as a Cloud Service SDK
+ `~/aem-sdk/author` bevat de AEM Author Service
+ `~/aem-sdk/publish` bevat de AEM Publish Service
+ `~/aem-sdk/dispatcher` bevat de Dispatcher Tools
+ `~/code/<project name>` bevat de aangepaste AEM Project-broncode

`~` is verkort voor de gebruikerslijst. In Windows is dit het equivalent van `%HOMEPATH%` ;

## Ontwikkelingsinstrumenten voor AEM-projecten

Het AEM-project is de basis van de aangepaste code die de code, configuratie en inhoud bevat die via Cloud Manager naar AEM as a Cloud Service wordt geïmplementeerd. De basislijnprojectstructuur wordt geproduceerd via het [ Project van AEM Maven Archetype ](https://github.com/adobe/aem-project-archetype).

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ Installeren [!DNL Java]
+ Installeren [!DNL Node.js] (en npm)
+ Installeren [!DNL Maven]
+ Installeren [!DNL Git]

[Ontwikkelingshulpmiddelen instellen voor AEM-projecten](./development-tools.md)

## Lokale AEM Runtime

De AEM as a Cloud Service SDK biedt een [!DNL QuickStart Jar] die een lokale versie van AEM uitvoert. [!DNL QuickStart Jar] kan worden gebruikt om de AEM Author Service of de AEM Publish Service lokaal uit te voeren. Hoewel de [!DNL QuickStart Jar] een lokale ontwikkelervaring biedt, worden niet alle functies die beschikbaar zijn in AEM as a Cloud Service opgenomen in de [!DNL QuickStart Jar] .

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ Installeren [!DNL Java]
+ Download de AEM SDK
+ Voer de [!DNL AEM Author Service] uit
+ Voer de [!DNL AEM Publish Service] uit

[De lokale AEM-runtime instellen](./aem-runtime.md)

## Lokale [!DNL Dispatcher] runtime

AEM as a Cloud Service SDK Dispatcher Tools biedt alles wat nodig is om de lokale [!DNL Dispatcher] runtime in te stellen. [!DNL Dispatcher] Gereedschappen zijn gebaseerd op [!DNL Docker] en beschikken over opdrachtregelprogramma&#39;s voor het omzetten van [!DNL Apache HTTP] Web Server- en [!DNL Dispatcher] -configuratiebestanden in een compatibele indeling en voor het implementeren van deze bestanden in [!DNL Dispatcher] in de [!DNL Docker] -container.

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ Download de AEM SDK
+ [!DNL Dispatcher] Gereedschappen installeren
+ De lokale [!DNL Dispatcher] runtime uitvoeren

[Opstelling Lokale  [!DNL Dispatcher]  Runtime](./dispatcher-tools.md)
