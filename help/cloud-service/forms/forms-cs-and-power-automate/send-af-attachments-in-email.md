---
title: Formulierbijlagen verzenden in een e-mail
description: Verzend en extraheer verzonden formulierbijlagen in een e-mail met behulp van de geautomatiseerde workflow
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 0%

---

# Formulierbijlagen uitnemen van verzonden formuliergegevens

Extraheer formulierbijlagen en verzend de bijlagen in een geautomatiseerde workflow voor e-mail.
In de volgende video worden de stappen uitgelegd die nodig zijn om bijlagen van de verzonden gegevens te vormen.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

Het volgende is het schema van het gehechtheidsvoorwerp dat u in de het schemastap van Parse JSON moet gebruiken

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
