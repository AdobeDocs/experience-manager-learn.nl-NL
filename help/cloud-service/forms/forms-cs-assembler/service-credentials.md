---
title: AEM servicekredieten
description: Download servicegegevens van AEM Developer Console.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 453
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 0%

---

# Servicereferenties

Integraties met AEM as a Cloud Service moeten veilig kunnen worden geverifieerd op AEM. De Developer Console van AEM genereert Service Credentials. Deze worden gebruikt door externe toepassingen, systemen en services om via HTTP programmatisch te communiceren met AEM auteur- of Publish-services.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Het gedownloade dossier van de dienstgeloofsbrieven wordt opgeslagen als middeldossier genoemd service_token.json in de verstrekte eclipse. De waarden in het service_token dossier worden gebruikt om JWT te produceren en JWT voor een Token van de Toegang te ruilen. De hulpprogrammaklasse GetServiceCredentials wordt gebruikt om de bezitswaarden van het dienst_token.json middeldossier te halen.
