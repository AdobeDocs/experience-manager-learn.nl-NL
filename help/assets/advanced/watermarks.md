---
title: Watermerken in AEM Assets
description: AEM als de watermarkeringsmogelijkheden van een Cloud Service, kunnen aangepaste afbeeldingsuitvoeringen met elk PNG-bestand worden gemarkeerd.
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
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
