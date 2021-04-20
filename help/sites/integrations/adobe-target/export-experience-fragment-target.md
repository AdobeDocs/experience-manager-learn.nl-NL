---
title: Fragmenten voor ervaring exporteren naar Adobe Target
description: Leer hoe u AEM Experience Fragment publiceert en exporteert als Adobe Target-aanbiedingen.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 3%

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

    ![Interface-fout voor doel-API](assets/error-target-offer.png)

... en de volgende logboekberichten in het `aemerror` logboek:

    ![API-doelconsolefout](assets/target-console-error.png)

#### Resolutie

1. Aanmelden bij [Admin Console](https://adminconsole.adobe.com/) met beheerrechten voor het gebruikte Adobe Target-productprofiel, maar AEM integratie
2. Selecteer __Producten > Adobe Target > Productprofiel__
3. Selecteer onder __Integraties__ tabblad de integratie voor uw AEM als een Cloud Service-omgeving (dezelfde naam als het Adobe I/O-project)
4. Wijs __Editor__ of __Fiatteur__ rol toe

   ![DoelAPI-fout](assets/target-permissions.png)

Als u de juiste machtigingen toevoegt aan de Adobe Target-integratie, wordt deze fout opgelost.

## Ondersteunende koppelingen

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)