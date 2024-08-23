---
title: Documentgeneratie met batch-API in AEM Forms CS
description: Configureer en activeer batchbewerkingen om documenten te genereren.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
duration: 26
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Inleiding

In een batchaanvraag worden tientallen, honderden of duizenden vergelijkbare documenten tegelijk gegenereerd. Voorbeeld: een financiële onderneming kan creditcardoverzichten genereren om deze naar al haar klanten te sturen.
Batch-API&#39;s (Asynchrone API&#39;s) zijn geschikt voor geplande toepassingen waarbij meerdere documenten met hoge doorvoer worden gegenereerd. Met deze API&#39;s worden documenten batchgewijs gegenereerd. Zo worden telefoonrekeningen, creditcardafschriften en uitkeringsafschriften elke maand gegenereerd.

Als u de AEM Forms CS-batchbewerking-API wilt gebruiken, hebt u de volgende configuraties nodig

1. Azure-opslagaccount configureren
1. Azure-cloudconfiguratie met opslagondersteuning
1. Batchgegevensopslagconfiguratie maken
1. De batch-API uitvoeren

Het wordt geadviseerd u vertrouwd met de [ API documentatie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) alvorens te werk te gaan om dit leerprogramma te gebruiken.
