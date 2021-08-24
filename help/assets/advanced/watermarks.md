---
title: Watermerken in AEM Assets
description: AEM als de watermarkeringsmogelijkheden van een Cloud Service, kunnen aangepaste afbeeldingsuitvoeringen met elk PNG-bestand worden gemarkeerd.
feature: asset compute microservices
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: Inhoudsbeheer
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%

---


# Watermerken

AEM als de watermarkeringsmogelijkheden van een Cloud Service, kunnen aangepaste afbeeldingsuitvoeringen met elk PNG-bestand worden gemarkeerd.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi-configuratie

De volgende OSGi configuratiestub kan aan `ui.config` subproject van uw AEM worden bijgewerkt en worden toegevoegd.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
