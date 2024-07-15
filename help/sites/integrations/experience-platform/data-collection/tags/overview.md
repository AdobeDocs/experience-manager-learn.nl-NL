---
title: Tags integreren in Adobe Experience Platform en AEM
description: De markeringen in de Inzameling van de Gegevens van het Experience Platform zijn de volgende-generatieoplossing van het markeringsbeheer van de Adobe en de beste manier om Adobe Analytics, Doel, Audience Manager, en vele meer oplossingen op te stellen. Bekijk een overzicht van tags in Adobe Experience Platform en de aanbevolen integratie met Adobe Experience Manager.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Tags en AEM voor gegevensverzameling van Experience Platforms integreren {#overview}

Leer hoe u de tags in Adobe Experience Platform kunt integreren met Adobe Experience Manager.

Tags zijn Adobe Experience Platform van de volgende generatie technologie voor tagbeheer. Tags bieden de eenvoudigste manier om Adobe Analytics, Target, Audience Manager en nog veel meer oplossingen te implementeren. Bekijk een overzicht van Tags en de aanbevolen integratie met Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## Vereisten

Het volgende wordt vereist wanneer het integreren van de Markeringen van de Inzameling van de Gegevens van het Experience Platform.

+ Toegang AEM beheerder tot AEM as a Cloud Service-omgeving
+ Een verwijzingsplaats als [ WKND ](https://github.com/adobe/aem-guides-wknd) op het wordt opgesteld dat.
+ Toegang tot Adobe Experience Platform-oplossing voor gegevensverzameling
+ De toegang van de Beheerder van het systeem tot [ Adobe Developer Console ](https://developer.adobe.com/developer-console/)


## Stappen op hoog niveau

+ In de Inzameling van Gegevens van Adobe Experience Platform, creeer een bezit van de Markering en geef het uit aan _voeg Regel_ toe. Dan _voeg Bibliotheek_ toe, selecteer de onlangs toegevoegde regel, keur, en publiceer het goed.
+ AEM en tags verbinden met behulp van bestaande (of nieuwe) IMS-configuratie
+ In AEM maakt u een cloudserviceconfiguratie voor tags, past u deze vervolgens toe op een bestaande site en controleert u ten slotte of de eigenschap Tags en de bibliotheken ervan op de site Published of Auteur zijn geladen.

## Volgende stappen

[Een eigenschap voor een tag maken](create-tag-property.md)

## Aanvullende bronnen {#additional-resources}

+ [ Integraties van het Experience Platform met de Toepassingen van het Experience Cloud ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [ Overzicht van Markeringen ](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [ Uitvoerend het Experience Cloud in Websites met Markeringen ](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
