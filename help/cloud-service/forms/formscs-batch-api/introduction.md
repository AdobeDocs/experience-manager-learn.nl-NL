---
title: Documentgeneratie met batch-API in AEM Forms CS
description: Configureer en activeer batchbewerkingen om documenten te genereren.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
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

U wordt aangeraden vertrouwd te raken met de [API-documentatie](https://experienceleague.corp.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) voordat u deze zelfstudie gaat gebruiken.




