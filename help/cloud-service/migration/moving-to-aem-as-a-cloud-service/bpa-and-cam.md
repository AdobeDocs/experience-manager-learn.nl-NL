---
title: Opstelling uw project BPA en CAM
description: Leer hoe de Analysator van de Beste praktijken en de Manager van de Versnelling van de Wolk een aangepaste gids voor het migreren aan AEM as a Cloud Service verstrekt.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Best Practice Analyzer en Cloud Acceleration Manager

Leer hoe de Analysator van Beste praktijken (BPA) en de Manager van de Versnelling van de Wolk (CAM) een aangepaste gids voor het migreren aan AEM as a Cloud Service verstrekt. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Gereedheid evalueren

![BPA- en CAM-diagram op hoog niveau](assets/bpa-cam-diagram.png)

Het BPA-pakket moet worden geïnstalleerd op een kloon van de AEM 6.x-omgeving. De BPA zal een rapport produceren dat dan in CAM kan worden geupload, dat begeleiding in de belangrijkste activiteiten zal verstrekken die moeten plaatsvinden om zich naar AEM as a Cloud Service te bewegen.

### Belangrijkste activiteiten

* Maak een kloon van uw productie 6.x milieu. Wanneer u inhoud en refactorcode migreert, is het van belang een kloon van een productieomgeving te hebben om verschillende gereedschappen en wijzigingen te testen.
* Download het nieuwste BPA-programma van de [Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) en installeer op uw AEM 6.x gekloonde omgeving.
* Gebruik het hulpmiddel BPA om een rapport te produceren dat aan de Manager van de Versnelling van de Wolk (CAM) kan worden geupload. CAM is toegankelijk via [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* Gebruik CAM om richtlijnen te geven voor de updates die moeten worden uitgevoerd in de huidige codebasis en -omgeving om naar AEM as a Cloud Service te gaan.
