---
title: Formulierbijlagen verzenden in een e-mail
description: Verzend en extraheer verzonden formulierbijlagen in een e-mail met behulp van de geautomatiseerde workflow
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: aea43a705b3959f8be26238d32b816b3953e153e
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Formulierbijlagen uitnemen van verzonden formuliergegevens

Extraheer formulierbijlagen en verzend de bijlagen in een geautomatiseerde workflow voor e-mail.
In de volgende video worden de stappen uitgelegd die nodig zijn om bijlagen van de verzonden gegevens te vormen.
>[!VIDEO](https://video.tv.adobe.com/v/3409017/?quality=12&learn=on)

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