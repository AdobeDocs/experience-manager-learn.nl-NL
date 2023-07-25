---
title: Fragmenten voor ervaring exporteren naar Adobe Target
description: Leer hoe u AEM Experience Fragment publiceert en exporteert als Adobe Target-aanbiedingen.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 1%

---

# Exportervaringsfragment naar Adobe Target {#experience-fragment-target}

Leer hoe u AEM Ervaring Fragment als Adobe Target-aanbiedingen exporteert.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Volgende stappen

+ [Een doelactiviteit maken met behulp van de fragmentatieaanbiedingen van de ervaring](./create-target-activity.md)

## Problemen oplossen

### Exporteren van ervaringsfragmenten naar doel mislukt

#### Fout

Als u het fragment Experience naar Adobe Target exporteert zonder de juiste machtigingen in Adobe Admin Console, treedt de volgende fout op bij de AEM Author-service:

    ![UI-fout doel-API](assets/error-target-offer.png)

... en de volgende logberichten in de `aemerror` logboek:

    ![Fout doelAPI-console](assets/target-console-error.png)

#### Resolutie

1. Aanmelden bij [Admin Console](https://adminconsole.adobe.com/) met beheerrechten voor het Adobe Target-productprofiel dat wordt gebruikt, maar AEM integratie
2. Selecteren __Producten > Adobe Target > Productprofiel__
3. Onder __Integraties__ tab, selecteer de integratie voor uw AEM as a Cloud Service omgeving (dezelfde naam als het Adobe I/O-project)
4. Toewijzen __Editor__ of __Fiatteur__ rol

   ![DoelAPI-fout](assets/target-permissions.png)

Als u de juiste machtigingen toevoegt aan de Adobe Target-integratie, wordt deze fout opgelost.

## Ondersteunende koppelingen

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
