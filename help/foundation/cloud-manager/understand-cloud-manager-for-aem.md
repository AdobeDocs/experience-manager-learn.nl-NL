---
title: Adobe Cloud Manager begrijpen
description: Adobe Cloud Manager biedt een eenvoudige, maar toch robuuste oplossing voor eenvoudig beheer, introspects en zelfbediening van AEM omgevingen.
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1026
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 0%

---

# Adobe Cloud Manager begrijpen

Adobe Cloud Manager biedt een eenvoudige, maar toch robuuste oplossing voor eenvoudig beheer, introspects en zelfbediening van AEM omgevingen.

## Overzicht van Cloud Manager

In deze videoreeks worden de belangrijkste functies van Cloud Manager besproken voor AEM, waaronder:

* [Programma&#39;s](#programs)
* [Omgevingen](#environments)
* [Rapporten](#reports)
* [Productiepijpleiding CI/CD](#cicd-production-pipeline)
* [Niet-productiepijpleidingen CI/CD](#cicd-non-production-pipeline)
* [Activiteit](#activity)

Voor een volledig overzicht raadpleegt u de [Gebruikershandleiding voor Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html).

## Programma&#39;s {#programs}

[Cloud Manager-programma&#39;s](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) vertegenwoordigen reeksen AEM milieu&#39;s die logische reeksen bedrijfsinitiatieven steunen, typisch die aan een gekochte Overeenkomst van het Niveau van de Dienst (SLA) beantwoorden. Bijvoorbeeld, kan één Programma de AEM middelen vertegenwoordigen om de globale openbare Websites te steunen, terwijl een ander Programma een interne Centrale DAM vertegenwoordigt.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## Omgevingen {#environments}

[Cloud Manager-omgevingen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) zijn samengesteld uit AEM instanties Auteur, AEM Publiceren en Dispatcher. De verschillende milieu&#39;s steunen rollen en kunnen worden betrokken gebruikend verschillende (hieronder beschreven) pijpleidingen CI/CD. Cloud Manager-omgevingen hebben doorgaans één productieomgeving en één werkgebiedomgeving.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## Rapporten {#reports}

[Cloud Manager-rapporten](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) Geef een overzicht van de milieus en AEM van het programma door middel van een reeks grafieken die verschillende meetgegevens voor elke AEM rapporteren en bijhouden.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## Productiepijpleiding CI/CD {#cicd-production-pipeline}

*[De CI/CD-pijpleiding gebruiken in Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) videoreeks verstrekt een diepe duik in de uitvoering van de Pijpleiding van de Productie, met inbegrip van exploratie van het ontbreken en succesvolle plaatsingen.*

>[!NOTE]
>
> Door deze video&#39;s zijn de ontwikkelings-, test- en implementatietijden verkort om de tijd van de video te verkorten. Een volledige pijpleidingsuitvoering neemt typisch 45 minuten of meer (met inbegrip van de verplichte 30 minieme prestatietests), afhankelijk van de projectgrootte, het aantal AEM instanties en de processen van UAT.

### Configuratie

De [Productiepijpleiding CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) de configuratie bepaalt de trekker die de pijpleiding in werking stelt, en parameters die de de plaatsing van de productie en de parameters van de prestatietest controleren.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### Uitvoering pijpleiding

De [Productiepijpleiding CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) wordt gebruikt om code door Stadium aan het milieu van de Productie te bouwen en op te stellen, die tijd aan waarde verminderen.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## Niet-productiepijpleidingen CI/CD {#cicd-non-production-pipeline}

[Niet-productiepijpleidingen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) zijn verdeeld in twee categorieën, de pijpleidingen van de Kwaliteit van de Code, en de pijpleidingen van de Plaatsing. Codekwaliteitpijplijnen zorgen ervoor dat alle code van een Git-vertakking wordt samengesteld en wordt geëvalueerd op basis van de codescanfunctie van Cloud Manager. Implementatiepijpleidingen ondersteunen de geautomatiseerde implementatie van code van de Git-opslagplaats naar elke niet-productieomgeving, wat betekent dat een voorziening AEM omgeving die geen werkgebied of productie is.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## Activiteit {#activity}

Cloud Manager biedt een geconsolideerde weergave van de activiteit van een programma, waarin alle uitvoeringen van CI/CD Pipeline, zowel productie als niet-productie, worden vermeld, zodat de zichtbaarheid in het verleden en de huidige activiteit mogelijk is, en de details van elke activiteit kunnen worden herzien.

Cloud Manager kan ook per gebruiker worden geïntegreerd met [Adobe Experience Cloud-meldingen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), die een alomtegenwoordig beeld geven van gebeurtenissen en acties van belang.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
