---
title: Lokale ontwikkelomgeving voor AEM as a Cloud Service
description: Adobe Experience Manager (AEM) overzicht van de lokale ontwikkelomgeving.
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Lokale ontwikkelomgeving instellen {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Overzicht"
>abstract="De vestiging lokale ontwikkelomgeving voor AEM as a Cloud Service omvat ontwikkelingshulpmiddelen die worden vereist om AEM Projecten te ontwikkelen, te bouwen en te compileren, evenals lokale runtime die ontwikkelaars toestaat om nieuwe eigenschappen plaatselijk snel te bevestigen alvorens hen aan AEM as a Cloud Service via Adobe Cloud Manager op te stellen."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="Richtlijnen voor ontwikkeling"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Grondbeginselen van ontwikkeling"

In deze zelfstudie wordt het opzetten van een lokale ontwikkelomgeving voor Adobe Experience Manager (AEM) met de SDK van AEM as a Cloud Service besproken. Hieronder vindt u de ontwikkelingstool die nodig is voor het ontwikkelen, bouwen en compileren van AEM projecten, en lokale runtimes waarmee ontwikkelaars nieuwe functies snel lokaal kunnen valideren voordat ze deze via Adobe Cloud Manager naar AEM as a Cloud Service implementeren.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![ AEM as a Cloud Service de Stapel van de Technologie van de Milieu van de Lokale Ontwikkeling ](./assets/overview/aem-sdk-technology-stack.png)

De lokale ontwikkelomgeving voor AEM kan worden opgesplitst in drie logische groepen:

+ Het __AEM Project__ bevat de douanecode, de configuratie en de inhoud die de AEM toepassing van de douane is.
+ De __Lokale AEM Runtime__ die een lokale versie van AEMAuteur en de diensten van Publish plaatselijk in werking stelt.
+ De __Lokale Runtime van Dispatcher__ die een lokale versie van de Server en Dispatcher van het Web van Apache HTTP in werking stelt.

Dit leerprogramma analyseert hoe te om de benadrukte punten in het bovengenoemde diagram te installeren en te plaatsen, die een stabiele lokale ontwikkelomgeving voor AEM ontwikkeling verstrekken.

## Bestandssysteemorganisatie

In deze zelfstudie werden de locatie van de AEM as a Cloud Service SDK-artefacten en AEM projectcode als volgt vastgesteld:

+ `~/aem-sdk` is een organisatiemap met de verschillende tools van de SDK van AEM as a Cloud Service
+ `~/aem-sdk/author` bevat de AEM Auteur-service
+ `~/aem-sdk/publish` bevat de AEM Publish Service
+ `~/aem-sdk/dispatcher` bevat de Dispatcher Tools
+ `~/code/<project name>` bevat de aangepaste AEM-broncode van het project

`~` is verkort voor de gebruikerslijst. In Windows is dit het equivalent van `%HOMEPATH%` ;

## Ontwikkelingsinstrumenten voor AEM projecten

Het AEM project is de basis van de douanecode die de code, de configuratie en de inhoud bevat die via Cloud Manager aan AEM as a Cloud Service wordt opgesteld. De basislijnprojectstructuur wordt geproduceerd via het [ AEM Project Maven Archetype ](https://github.com/adobe/aem-project-archetype).

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ Installeren [!DNL Java]
+ Installeren [!DNL Node.js] (en npm)
+ Installeren [!DNL Maven]
+ Installeren [!DNL Git]

[Ontwikkelingshulpmiddelen instellen voor AEM projecten](./development-tools.md)

## Lokale AEM Runtime

De AEM as a Cloud Service SDK biedt een [!DNL QuickStart Jar] die een lokale versie van AEM uitvoert. Met [!DNL QuickStart Jar] kunt u de AEM Author Service of AEM Publish Service lokaal uitvoeren. Hoewel de [!DNL QuickStart Jar] een lokale ontwikkelervaring biedt, worden niet alle functies die beschikbaar zijn in AEM as a Cloud Service opgenomen in de [!DNL QuickStart Jar] .

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ Installeren [!DNL Java]
+ De AEM SDK downloaden
+ Voer de [!DNL AEM Author Service] uit
+ Voer de [!DNL AEM Publish Service] uit

[De lokale AEM-runtime instellen](./aem-runtime.md)

## Lokale [!DNL Dispatcher] runtime

Dispatcher Tools van AEM as a Cloud Service SDK biedt alles wat vereist is voor het instellen van de lokale [!DNL Dispatcher] -runtime. [!DNL Dispatcher] Gereedschappen zijn gebaseerd op [!DNL Docker] en beschikken over opdrachtregelprogramma&#39;s voor het omzetten van [!DNL Apache HTTP] Web Server- en [!DNL Dispatcher] -configuratiebestanden in een compatibele indeling en voor het implementeren van deze bestanden in [!DNL Dispatcher] in de [!DNL Docker] -container.

In dit gedeelte van de zelfstudie wordt getoond hoe u:

+ De AEM SDK downloaden
+ [!DNL Dispatcher] Gereedschappen installeren
+ De lokale [!DNL Dispatcher] runtime uitvoeren

[Opstelling Lokale  [!DNL Dispatcher]  Runtime](./dispatcher-tools.md)
