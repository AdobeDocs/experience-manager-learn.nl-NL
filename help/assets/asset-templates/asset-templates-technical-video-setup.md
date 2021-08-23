---
title: Asset Templates instellen met AEM Assets en InDesign Server
description: Met Asset Templates kunnen marketers digitale middelen maken, beheren en leveren voor digitale doeleinden en afdrukken. Het maken van marketingbrochures, visitekaartjes, vliegers, advertenties en postkaarten is veel eenvoudiger met Asset Templates wanneer deze zijn geïntegreerd met de InDesign-server. De configuratie van de server van InDesign met AEM wordt behandeld in deze sectie.
version: 6.3, 6.4, 6.5
topic: Inhoudsbeheer
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# Asset Templates instellen met AEM Assets en InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Met Asset Templates kunnen marketers digitale middelen maken, beheren en leveren voor digitale doeleinden en afdrukken. Het maken van marketingbrochures, visitekaartjes, vliegers, advertenties en postkaarten is veel eenvoudiger met Asset Templates wanneer deze zijn geïntegreerd met de InDesign-server. De configuratie van de server van InDesign met AEM wordt behandeld in deze sectie.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **moet** worden aangesloten op een actieve InDesign-server wanneer de INDD-sjabloon wordt geüpload. Voor een deel van de eerste verwerking in het INDD-bestand is een InDesign-server vereist.

## InDesign Server-proefversie downloaden {#download-indesign-server-trial}

Download [InDesign Server proefdownload Website](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## InDesign Server starten {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
