---
title: Asset Templates instellen met AEM Assets en InDesign Server
description: Met Asset Templates kunnen marketers digitale middelen maken, beheren en leveren voor digitale doeleinden en afdrukken. Het maken van marketingbrochures, visitekaartjes, vliegers, advertenties en postkaarten is veel eenvoudiger met Asset Templates wanneer deze zijn geïntegreerd met de InDesign-server. De configuratie van de server van InDesign met AEM wordt behandeld in deze sectie.
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# Asset Templates instellen met AEM Assets en InDesign Server{#set-up-asset-templates-with-aem-assets-and-indesign-server}

Met Asset Templates kunnen marketers digitale middelen maken, beheren en leveren voor digitale doeleinden en afdrukken. Het maken van marketingbrochures, visitekaartjes, vliegers, advertenties en postkaarten is veel eenvoudiger met Asset Templates wanneer deze zijn geïntegreerd met de InDesign-server. De configuratie van de server van InDesign met AEM wordt behandeld in deze sectie.

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **moet** verbinding maken met een actieve InDesign-server wanneer de INDD-sjabloon wordt geüpload. Voor een deel van de eerste verwerking in het INDD-bestand is een InDesign-server vereist.

## InDesign Server-proefversie downloaden {#download-indesign-server-trial}

Downloaden [Website voor downloaden van InDesign Server-proefversie](https://www.adobeprerelease.com/)

## InDesign Server starten {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
