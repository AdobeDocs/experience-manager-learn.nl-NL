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
duration: 17
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# Basic React-app

Deze [ Reactie ](https://reactjs.org/) app toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted vragen. Deze toepassing geeft filterbaar van avonturen WKND terug, en na het selecteren van een avontuur, toont de avonturen volledige details.

Deze code:

+ Maakt verbinding met een AEM Publish-service en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-slug`

Voor een meer diepgaande overzicht van hoe deze app Next.js wordt gebouwd, herzie het [ voorbeeld Reageer app documentatie ](../example-apps/react-app.md).
