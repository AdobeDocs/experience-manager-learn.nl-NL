---
title: AEM Assets Microservices en overstappen op AEM as a Cloud Service
description: Leer hoe u met de asset compute-microservices van AEM Assets as a Cloud Service automatisch en efficiënt elke uitvoering voor uw middelen kunt genereren en deze rol van de traditionele AEM-workflow kunt vervangen.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 1%

---

# AEM Assets Microservices - Verplaatsen naar AEM as a Cloud Service

Leer hoe u met de asset compute-microservices van AEM Assets as a Cloud Service automatisch en efficiënt elke uitvoering voor uw middelen kunt genereren en deze rol van de traditionele AEM-workflow kunt vervangen.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Workflow-migratiehulpprogramma

![De tool Asset Workflow Migration](./assets/asset-workflow-migration.png)

Als deel van het refactoring van uw codebasis, gebruik [Hulpprogramma voor workflowmigratie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) bestaande workflows migreren om de Asset compute microservices in AEM as a Cloud Service te gebruiken.

### Belangrijkste activiteiten

* Gebruik de [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) hulpmiddel om werkstromen voor middelenverwerking te migreren voor het gebruik van de Asset compute microservices.
* Een [plaatselijke ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) en implementeer de bijgewerkte workflows. Mogelijk is een handmatige aanpassing nodig voor complexe workflows.
* Doorgaan met herhalen in een lokale ontwikkelomgeving met de AEM SDK totdat de bijgewerkte workflow overeenkomt met de pariteit van de functie.
* Implementeer de bijgewerkte codebasis in een AEM as a Cloud Service ontwikkelomgeving en blijf valideren.

