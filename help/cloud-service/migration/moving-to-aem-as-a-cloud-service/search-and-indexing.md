---
title: Zoeken en indexeren in AEM as a Cloud Service
description: Leer over AEM as a Cloud Service onderzoeksindexen, hoe te om AEM 6 indexdefinities om te zetten, en hoe te om indexen op te stellen.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Zoeken en indexeren

Leer over AEM as a Cloud Service onderzoeksindexen, hoe te om AEM 6 indexdefinities om te zetten om as a Cloud Service compatibel AEM te zijn, en hoe te om indexen op te stellen om as a Cloud Service te AEM.

>[!VIDEO](https://video.tv.adobe.com/v/336963/?quality=12&learn=on)

## Indexconversie

![Indexconversie](./assets/index-converter.png)

Als deel van het refactoring van uw codebasis, gebruik [Gereedschap Index converteren](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) om aangepaste Eak-indexdefinities om te zetten in AEM as a Cloud Service compatibele indexdefinities.

### Belangrijkste activiteiten

* Gebruik de [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) hulpmiddel om werkstromen voor middelenverwerking te migreren voor het gebruik van de Asset compute microservices.
* Een [plaatselijke ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) en implementeer de aangepaste indexen. Zorg ervoor dat de bijgewerkte indexen up-to-date zijn.
* Implementeer de bijgewerkte codebasis in een AEM as a Cloud Service ontwikkelomgeving en blijf valideren.
* Indien een waarde uit de index van het vak wordt gewijzigd **ALTIJD** kopieer de meest recente indexdefinitie van een AEM as a Cloud Service omgeving die op de meest recente release wordt uitgevoerd. Pas de gekopieerde indexdefinitie aan uw behoeften aan.