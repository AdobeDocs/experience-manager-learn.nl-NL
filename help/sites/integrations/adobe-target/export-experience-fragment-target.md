---
title: Fragmenten voor ervaring exporteren naar Adobe Target
description: Leer hoe u AEM Experience Fragment publiceert en exporteert als Adobe Target-aanbiedingen.
feature: Experience Fragments
version: Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '180'
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

Als u het fragment Experience naar Adobe Target exporteert zonder de juiste machtigingen in Adobe Admin Console, treedt de volgende fout op bij de AEM Auteur-service:

![ de Fout van doel API UI ](assets/error-target-offer.png)

... en de volgende logberichten in het `aemerror` -logboek:

![ Fout van de Console van het Doel API ](assets/target-console-error.png)

#### Resolutie

1. Login aan [ Admin Console ](https://adminconsole.adobe.com/) met administratieve rechten voor het Gebruikte Profiel van het Product van Adobe Target maar de AEM integratie
2. Selecteer __Producten > Adobe Target > het Profiel van het Product__
3. Onder __Integraties__ lusje, selecteer de integratie voor uw milieu van AEM as a Cloud Service (zelfde naam zoals het project van Adobe Developer)
4. Wijs __Redacteur__ toe of __Approver__ rol

   ![ de Fout van doel API ](assets/target-permissions.png)

Als u de juiste machtigingen toevoegt aan de Adobe Target-integratie, wordt deze fout opgelost.

## Ondersteunende koppelingen

+ [ Foutopsporing van Adobe Experience Cloud - Chrome ](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
