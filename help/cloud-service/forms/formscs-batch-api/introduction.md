---
title: Documentgeneratie met batch-API in AEM Forms CS
description: Configureer en activeer batchbewerkingen om documenten te genereren.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '134'
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
