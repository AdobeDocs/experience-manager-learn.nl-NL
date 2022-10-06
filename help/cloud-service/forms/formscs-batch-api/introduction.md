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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Inleiding

In een batchaanvraag worden tientallen, honderden of duizenden vergelijkbare documenten tegelijk gegenereerd. Voorbeeld: Een financiële onderneming kan creditcardoverzichten genereren om deze naar al haar klanten te sturen.
Batch-API&#39;s (Asynchrone API&#39;s) zijn geschikt voor geplande toepassingen waarbij meerdere documenten met hoge doorvoer worden gegenereerd. Deze API&#39;s genereren documenten batchgewijs. Zo worden telefoonrekeningen, creditcardafschriften en uitkeringsafschriften elke maand gegenereerd.

Als u de AEM Forms CS-batchbewerking-API wilt gebruiken, hebt u de volgende configuraties nodig

1. Azure-opslagaccount configureren
1. Azure-cloudconfiguratie met opslagondersteuning
1. Batchgegevensopslagconfiguratie maken
1. De batch-API uitvoeren

U wordt aangeraden vertrouwd te raken met de [API-documentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) voordat u deze zelfstudie gaat gebruiken.
