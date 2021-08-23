---
title: Adobe Cloud Manager begrijpen
description: Adobe Cloud Manager biedt een eenvoudige, maar toch robuuste oplossing voor eenvoudig beheer, introspect en zelfbediening van AEM omgevingen.
sub-product: cloudbeheer, stichting
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architectuur
role: Architect
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 1%

---


# Adobe Cloud Manager begrijpen

Adobe Cloud Manager biedt een eenvoudige, maar toch robuuste oplossing voor eenvoudig beheer, introspect en zelfbediening van AEM omgevingen.

## Overzicht van Cloud Manager

In deze videoreeks worden de belangrijkste functies van Cloud Manager besproken voor AEM, waaronder:

* [Programma&#39;s](#programs)
* [Omgevingen](#environments)
* [Rapporten](#reports)
* [Productiepijpleiding CI/CD](#cicd-production-pipeline)
* [Niet-productiepijpleidingen CI/CD](#cicd-non-production-pipeline)
* [Activiteit](#activity)

Voor een volledig overzicht raadpleegt u de [Gebruikershandleiding voor Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html).

## Programma&#39;s {#programs}

[Cloud Manager-](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) programma&#39;s vertegenwoordigen AEM omgevingen die logische reeksen bedrijfsinitiatieven ondersteunen, die doorgaans overeenkomen met een aangeschafte Service Level Agreement (SLA). Bijvoorbeeld, kan één Programma de AEM middelen vertegenwoordigen om de globale openbare Websites te steunen, terwijl een ander Programma een interne Centrale DAM vertegenwoordigt.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Omgevingen {#environments}

[Cloud Manager-](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) omgevingen zijn samengesteld uit instanties van AEM-auteur, AEM-publicatie en Dispatcher. De verschillende milieu&#39;s steunen rollen en kunnen worden betrokken gebruikend verschillende (hieronder beschreven) pijpleidingen CI/CD. Cloud Manager-omgevingen hebben doorgaans één productieomgeving en één werkgebiedomgeving.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Rapporten {#reports}

[De ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) Rapporten van de Manager van de wolk verstrekken een mening in de Milieu en AEM van het Programma door een reeks grafieken die op een verscheidenheid van metriek voor elke AEM melden en volgen.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## Productiepijpleiding CI/CD {#cicd-production-pipeline}

*[Met de CI/CD-pijpleiding in de ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Managervideoreeks van Adobe Cloud kunt u diep doordringen in de uitvoering van de productiepijpleiding, inclusief de verkenning van mislukte en geslaagde implementaties.*

>[!NOTE]
>
> In deze video&#39;s zijn de ontwikkelings-, test- en implementatietijden verkort om de tijd van de video te verkorten. Een volledige pijpleidingsuitvoering neemt typisch 45 minuten of meer (met inbegrip van de verplichte 30 minieme prestatietests), afhankelijk van de projectgrootte, het aantal AEM instanties en de processen van UAT.

### Configuratie

De configuratie [CI/CD Production Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) bepaalt de trekker die de pijpleiding zal in werking stellen, parameters die de productie plaatsing en de parameters van de prestatietest controleren.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Uitvoering pijpleiding

De [CI/CD Productiepijpleiding](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) wordt gebruikt om code door Stadium aan het milieu van de Productie te bouwen en op te stellen, die tijd aan waarde verminderen.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Niet-productiepijpleidingen CI/CD {#cicd-non-production-pipeline}

[CI/CD niet-productie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) pijpleidingen zijn in twee categorieën verdeeld, de Kwaliteitspijpleidingen van de Code, en de pijpleidingen van de Plaatsing. Codekwaliteitpijplijnen zorgen ervoor dat alle code van een Git-vertakking wordt samengesteld en wordt geëvalueerd op basis van de codescanfunctie van Cloud Manager. Implementatiepijpleidingen ondersteunen de geautomatiseerde implementatie van code van de Git-opslagplaats naar elke niet-productieomgeving, wat betekent dat een voorziening AEM omgeving die geen werkgebied of productie is.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Activiteit {#activity}

Cloud Manager biedt een geconsolideerde weergave van de activiteit van een programma, waarin alle uitvoeringen van CI/CD Pipeline, zowel productie als niet-productie, worden vermeld, zodat de zichtbaarheid in het verleden en de huidige activiteit mogelijk is, en de details van elke activiteit kunnen worden herzien.

Cloud Manager kan ook worden geïntegreerd op gebruikersniveau met [Adobe Experience Cloud-meldingen](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/notifications.html), zodat u een alomtegenwoordige weergave krijgt voor gebeurtenissen en acties van belang.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
