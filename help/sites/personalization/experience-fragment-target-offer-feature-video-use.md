---
title: Werken met AEM Experience Fragment-aanbiedingen in Adobe Target
seo-title: Werken met AEM Experience Fragment-aanbiedingen in Adobe Target
description: In Adobe Experience Manager 6.4 wordt de personalisatieworkflow tussen AEM en Target vernieuwd. Ervaringen die in AEM zijn gemaakt, kunnen nu rechtstreeks aan Adobe Target worden geleverd als HTML-aanbiedingen. Hiermee kunnen marketers inhoud op verschillende kanalen naadloos testen en aanpassen.
seo-description: In Adobe Experience Manager 6.4 wordt de personalisatieworkflow tussen AEM en Target vernieuwd. Ervaringen die in AEM zijn gemaakt, kunnen nu rechtstreeks aan Adobe Target worden geleverd als HTML-aanbiedingen. Hiermee kunnen marketers inhoud op verschillende kanalen naadloos testen en aanpassen.
sub-product: content-services
feature: Ervaringsfragmenten
topics: integrations, personalization
audience: all
doc-type: feature video
activity: setup
version: 6.4, 6.5
uuid: 7b91f65d-5a35-419a-8cf7-be850165dd33
discoiquuid: 45fc8d83-73fb-42e5-9c92-ce588c085ed4
topic: Personalisatie
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 1%

---


# Geniet van fragmentatieaanbiedingen in Adobe Target{#using-experience-fragment-offers-within-adobe-target}

In Adobe Experience Manager 6.4 wordt de personalisatieworkflow tussen AEM en Target vernieuwd. Ervaringen die in AEM zijn gemaakt, kunnen nu rechtstreeks aan Adobe Target worden geleverd als HTML-aanbiedingen. Hiermee kunnen marketers inhoud op verschillende kanalen naadloos testen en aanpassen.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>Aanbevolen wordt om de at.js-clientbibliotheek te gebruiken. De beste manier is om oplossingen voor tagbeheer, zoals Launch by Adobe, Adobe DTM of elke andere oplossing voor tagbeheer van derden, te gebruiken om doelbibliotheken aan uw sitepagina&#39;s toe te voegen

>[!NOTE]
>
>AEM Geniet van fragmentatieaanbiedingen in Adobe Target is ook beschikbaar als een functiepakket voor AEM 6.3-gebruikers. Zie de onderstaande sectie voor functiepakketten en afhankelijkheden.


* Adobe Experience Manager maakt eenvoudig te gebruiken en krachtige inhoud, samen met Adobe Target Artificial Intelligence (AI) en Machine Learning, en helpt inhoudsauteurs om inhoud te maken en te beheren voor alle kanalen op een gecentraliseerde locatie. Met de mogelijkheid om Experience Fragments naar Adobe Target te exporteren als HTML-aanbiedingen, hebben marketers nu meer flexibiliteit om een meer persoonlijke ervaring te creëren met deze aanbiedingen en kunnen ze nu elke ervaring die ze maken testen en schalen.
* Het belangrijkste verschil tussen de aanbiedingen van HTML en de aanbiedingen van het Fragment van de Ervaring is dat het uitgeven voor later slechts in AEM kan en dan met Adobe Target synchroniseren
* De de dienstconfiguratie van de Wolk van het doel die op de omslag van het Fragment van de Ervaring wordt toegepast erft aan alle die Fragments van de Ervaring direct onder de ouderomslag worden gecreeerd. De omslag van het kind erft niet de configuratie van de ouderwolkendiensten.
* Om een gepersonaliseerde aanbieding te maken, kunnen we nu gemakkelijk gebruikmaken van inhoud die is opgeslagen in AEM.
* U kunt typen doelactiviteiten maken, zoals Sensei-activiteiten zoals Automatisch toewijzen, Automatisch doel en Automated Personalization

## AEM 6.3 Functiepacks en -afhankelijkheden {#aem-feature-packs-and-dependencies}

| AEM 6.3 Functiepakket | Afhankelijkheden |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [CQ-6.3.0-FEATUREPACK-18961](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-18961) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 |
| [CQ-6.3.0-FEATUREPACK-24442](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24442) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumativefixpack:aem-6.3.2-cfp:1.0 |
| [CQ-6.3.0-FEATUREPACK-24640](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-24640) | adobe/cq630/servicepack:aem-service-pkg:6.3.2 adobe/cq630/cumativefixpack:aem-6.3.2-cfp:2.0 |

## Aanvullende bronnen {#additional-resources}

* [Documentatie over fragmenten voor ervaring](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [Ervaringsfragmenten gebruiken](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
