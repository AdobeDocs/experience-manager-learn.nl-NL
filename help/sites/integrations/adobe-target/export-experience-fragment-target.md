---
title: Fragmenten voor ervaring exporteren naar Adobe Target
description: Leer hoe u AEM Experience Fragment publiceert en exporteert als Adobe Target-aanbiedingen.
feature: Experience Fragments
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Exportervaringsfragment naar Adobe Target {#experience-fragment-target}

Leer hoe u AEM Experience Fragment exporteert als Adobe Target-aanbiedingen.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Volgende stappen

+ [Een doelactiviteit maken met behulp van de fragmentatieaanbiedingen van de ervaring](./create-target-activity.md)

## Problemen oplossen

### Exporteren van ervaringsfragmenten naar doel mislukt

#### Fout

Als u het fragment Experience naar Adobe Target exporteert zonder de juiste machtigingen in Adobe Admin Console, treedt de volgende fout op bij de AEM Author-service:

![&#x200B; de Fout van doel API UI &#x200B;](assets/error-target-offer.png)

... en de volgende logberichten in het `aemerror` -logboek:

![&#x200B; Fout van de Console van het Doel API &#x200B;](assets/target-console-error.png)

#### Resolutie

1. Login aan [&#x200B; Admin Console &#x200B;](https://adminconsole.adobe.com/) met administratieve rechten voor het Gebruikte Profiel van het Product van Adobe Target maar de integratie van AEM
2. Selecteer __Producten > Adobe Target > het Profiel van het Product__
3. Onder __Integraties__ lusje, selecteer de integratie voor uw milieu van AEM as a Cloud Service (zelfde naam zoals het project van Adobe Developer)
4. Wijs __Redacteur__ toe of __Approver__ rol

   ![&#x200B; de Fout van doel API &#x200B;](assets/target-permissions.png)

Als u de juiste machtigingen toevoegt aan de Adobe Target-integratie, wordt deze fout opgelost.

## Ondersteunende koppelingen

+ [&#x200B; Foutopsporing van Adobe Experience Cloud - Chrome &#x200B;](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
