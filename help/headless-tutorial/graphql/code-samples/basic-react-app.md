---
title: Basic React-app
description: Een standaard React-app die een lijst met WKND-avonturen en hun gegevens weergeeft
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11134
thumbnail: KT-11134.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 870be37f-68bb-4b0f-9918-e68b09be830e
duration: 21
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# Basic React-app

Dit [Reageren](https://reactjs.org/) toont hoe u inhoud kunt zoeken met behulp van AEM GraphQL API&#39;s met behulp van doorlopende query&#39;s. Deze toepassing geeft filterbaar van avonturen WKND terug, en na het selecteren van een avontuur, toont de avonturen volledige details.

Deze code:

+ Verbindt met de AEM publicatieservice, en vereist geen authentificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-slug`

Voor een diepgaande beoordeling van de manier waarop deze Next.js-app is gemaakt, raadpleegt u de [voorbeeld Documentatie voor toepassing Reageren](../example-apps/react-app.md).
