---
title: Adobe Cloud Manager begrijpen
description: Adobe Cloud Manager biedt een eenvoudige, maar toch robuuste oplossing voor eenvoudig beheer, introspects en zelfbediening van AEM-omgevingen.
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Developer
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1011
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 0%

---

# Adobe Cloud Manager begrijpen

Adobe Cloud Manager biedt een eenvoudige, maar toch robuuste oplossing voor eenvoudig beheer, introspects en zelfbediening van AEM-omgevingen.

## Cloud Manager - Overzicht

In deze videoreeks worden de belangrijkste functies van Cloud Manager voor AEM besproken, waaronder:

* [Programma&#39;s](#programs)
* [Omgevingen](#environments)
* [Rapporten](#reports)
* [Productiepijpleiding CI/CD](#cicd-production-pipeline)
* [Niet-productiepijpleidingen CI/CD](#cicd-non-production-pipeline)
* [Activiteit](#activity)

Voor een volledig overzicht, te herzien gelieve de [&#x200B; Gids van de Gebruiker van Cloud Manager &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=nl-NL).

## Programma&#39;s {#programs}

[&#x200B; de Programma&#39;s van Cloud Manager &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html?lang=nl-NL) vertegenwoordigen reeksen milieu&#39;s van AEM ondersteunend logische reeksen bedrijfsinitiatieven, typisch die aan een gekochte Service level agreement (SLA) beantwoorden. Bijvoorbeeld, kan één Programma de middelen van AEM vertegenwoordigen om de globale openbare Websites te steunen, terwijl een ander Programma een interne Centrale DAM vertegenwoordigt.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## Omgevingen {#environments}

[&#x200B; de Milieu&#39;s van Cloud Manager &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html?lang=nl-NL) zijn samengesteld uit de Auteur van AEM, publiceren AEM en de instanties van Dispatcher. De verschillende milieu&#39;s steunen rollen en kunnen worden betrokken gebruikend verschillende (hieronder beschreven) pijpleidingen CI/CD. Cloud Manager-omgevingen hebben doorgaans één productieomgeving en één werkgebiedomgeving.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## Rapporten {#reports}

[&#x200B; de Rapporten van Cloud Manager &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html?lang=nl-NL) verstrekken een mening in de Milieu&#39;s en instanties van AEM van het Programma door een reeks grafieken die op rapporteert en diverse metriek voor elke instantie van AEM volgen.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## Productiepijpleiding CI/CD {#cicd-production-pipeline}

*[gebruik CI/CD de Pijpleiding in Adobe Cloud Manager &#x200B;](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) videoreeks verstrekt een diepe duik in de uitvoering van de Pijpleiding van de Productie, met inbegrip van exploratie van het ontbreken en succesvolle plaatsingen.*

>[!NOTE]
>
> Door deze video&#39;s zijn de ontwikkelings-, test- en implementatietijden verkort om de tijd van de video te verkorten. Een volledige pijpleidingsuitvoering neemt typisch 45 minuten of meer (met inbegrip van de verplichte 30 minieme prestatietests), afhankelijk van de projectgrootte, het aantal instanties van AEM en processen van UAT.

### Configuratie

De [&#x200B; CI/CD configuratie van de Productiepijpleiding van de Productie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=nl-NL) bepaalt de trekker die de pijpleiding, en parameters in werking stelt die de de testparameters van de productieplaatsing en van de prestatietest controleren.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### Uitvoering pijpleiding

[&#x200B; CI/CD de Pijpleiding van de Productie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=nl-NL) wordt gebruikt om code door Stadium aan het milieu van de Productie te bouwen en op te stellen, die tijd aan waarde verminderen.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## Niet-productiepijpleidingen CI/CD {#cicd-non-production-pipeline}

[&#x200B; CI/CD de niet productiepijpleidingen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=nl-NL) zijn gebroken in twee categorieën, de pijpleidingen van de Kwaliteit van de Code, en de pijpleidingen van de Plaatsing. Codekwaliteitspipetten bevatten alle code van een Git-vertakking die moet worden gemaakt en worden geëvalueerd op basis van een Cloud Manager-scan van de codekwaliteit. Implementatiepijpleidingen ondersteunen de geautomatiseerde implementatie van code van de Git-opslagplaats naar elke niet-productieomgeving, wat een AEM-omgeving betekent die geen werkgebied of productie is.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## Activiteit {#activity}

Cloud Manager geeft een geconsolideerd beeld van de activiteiten van een programma, waarin alle uitvoeringen van CI/CD Pipeline, zowel productie als niet-productie, worden vermeld, waardoor de zichtbaarheid in het verleden en de huidige activiteit wordt toegestaan, en de details van elke activiteit kunnen worden herzien.

Cloud Manager integreert ook op een per-gebruikersniveau met [&#x200B; Meldingen van Adobe Experience Cloud &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html?lang=nl-NL), die een alomtegenwoordige mening in gebeurtenissen en acties van belang verstrekken.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
