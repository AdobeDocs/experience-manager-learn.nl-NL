---
title: AEM servicekredieten
description: Download servicegegevens van AEM Developer Console.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# Servicereferenties

Integraties met AEM as a Cloud Service moeten veilig kunnen worden geverifieerd op AEM. De Console van de Ontwikkelaar van AEM produceert de Referenties van de Dienst, die door externe toepassingen, systemen, en de diensten worden gebruikt om programmatically met de Auteur van AEM of de Publish diensten van HTTP in wisselwerking te staan.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Het gedownloade dossier van de dienstgeloofsbrieven wordt opgeslagen als middeldossier genoemd service_token.json in de verstrekte eclipse. De waarden in het service_token dossier worden gebruikt om JWT te produceren en JWT voor een Token van de Toegang te ruilen. De hulpprogrammaklasse GetServiceCredentials wordt gebruikt om de bezitswaarden van het dienst_token.json middeldossier te halen.