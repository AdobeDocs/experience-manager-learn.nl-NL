---
title: Tags voor gegevensverzameling van Experience Platforms integreren (Starten) en AEM
description: De markeringen in de Inzameling van de Gegevens van het Experience Platform zijn de volgende-generatieoplossing van het markeringsbeheer van de Adobe en de beste manier om Adobe Analytics, Doel, Audience Manager, en vele meer oplossingen op te stellen. Bekijk een overzicht van de labels (voorheen Launch genoemd) en de aanbevolen integratie met Adobe Experience Manager.
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
duration: 247
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---

# Tags en AEM voor gegevensverzameling van Experience Platforms integreren {#overview}

Leer hoe u het Experience Platform kunt integreren _Labels voor gegevensverzameling_ (voorheen bekend als Launch) met Adobe Experience Manager.

>[!NOTE]
>
>Adobe Experience Platform Launch is omgedoopt tot een reeks technologieën voor gegevensverzameling in Adobe Experience Platform. Diverse terminologische wijzigingen zijn als gevolg hiervan in de productdocumentatie doorgevoerd. Raadpleeg het volgende: [document](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) voor een geconsolideerde referentie van de terminologische wijzigingen.


Tags zijn Adobe Experience Platform van de volgende generatie technologie voor tagbeheer. Tags bieden de eenvoudigste manier om Adobe Analytics, Target, Audience Manager en nog veel meer oplossingen te implementeren. Bekijk een overzicht van Tags en de aanbevolen integratie met Adobe Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## Vereisten

Het volgende wordt vereist wanneer het integreren van de Markeringen van de Inzameling van de Gegevens van het Experience Platform.

+ Toegang AEM tot AEM as a Cloud Service omgeving
+ Een referentiesite als [WKND](https://github.com/adobe/aem-guides-wknd) op het worden ingezet.
+ Toegang tot Adobe Experience Platform-oplossing voor gegevensverzameling
+ Toegang van systeembeheerders tot [Adobe Developer Console](https://developer.adobe.com/developer-console/)


## Stappen op hoog niveau

+ Maak in Adobe Experience Platform Data Collection een Tag-eigenschap en bewerk deze naar _Regel toevoegen_. Vervolgens _Bibliotheek toevoegen_ selecteert u de zojuist toegevoegde regel, keurt u deze goed en publiceert u deze.
+ AEM en tags verbinden met behulp van bestaande (of nieuwe) IMS-configuratie
+ In AEM maakt u een configuratie van de cloudservices voor Starten, past u deze vervolgens toe op een bestaande site en controleert u ten slotte of de eigenschap Codes en de bijbehorende bibliotheken op de site Published of Auteur zijn geladen.

## Volgende stappen

[Een eigenschap voor een tag maken](create-tag-property.md)

## Aanvullende bronnen {#additional-resources}

+ [Integratie van Experience Platforms met Experience Cloud-toepassingen](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Overzicht van codes](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Het Experience Cloud implementeren in websites met tags](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
