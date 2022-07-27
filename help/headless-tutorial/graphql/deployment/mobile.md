---
title: Mobiele implementaties zonder koptelefoon AEM
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 0%

---


# Mobiele implementaties zonder koptelefoon AEM

AEM mobiele implementaties zonder koptelefoon zijn systeemeigen mobiele apps voor iOS, Android, enz. die op een krantenloze manier inhoud verbruiken en interageren.

Mobiele implementaties vereisen een minimale configuratie, aangezien HTTP-verbindingen met AEM headless API&#39;s niet in de context van een browser worden gestart.

## Implementatieconfiguraties

| Mobiele app maakt verbinding met | AEM-auteur | AEM-publicatie | Voorvertoning AEM |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [Verzendingsfilters](./dispatcher-fitlers.md) | ✘ | ✔ | ✔ |
| [CORS-configuratie](./cors.md) | ✘ | ✘ | ✘ |
| Hostnaam afbeeldings-URL | ✔ | ✔ | ✔ |

## Voorbeeld van mobiele apps

Adobe biedt voorbeelden van mobiele apps voor iOS en Android.


