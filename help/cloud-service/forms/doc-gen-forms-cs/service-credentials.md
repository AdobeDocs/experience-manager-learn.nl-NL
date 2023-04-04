---
title: AEM Forms Service Credentials
description: Download servicegegevens van AEM Developer Console.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# AEM Forms Service Credentials

Integraties met AEM as a Cloud Service moeten veilig kunnen worden geverifieerd op AEM. AEM de Console van de Ontwikkelaar produceert de Referenties van de Dienst, die door externe toepassingen, systemen, en de diensten worden gebruikt programmatically met AEM auteur of publiceer de diensten over HTTP in wisselwerking te staan.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Het gedownloade dossier van de dienstgeloofsbrieven wordt opgeslagen als middeldossier genoemd service_token.json in de verstrekte eclipse. De waarden in het service_token dossier worden gebruikt om JWT te produceren en JWT voor een Token van de Toegang te ruilen. De hulpprogrammaklasse GetServiceCredentials wordt gebruikt om de bezitswaarden van het dienst_token.json middeldossier te halen.
