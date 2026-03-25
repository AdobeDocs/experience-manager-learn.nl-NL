---
title: Basic React-app
description: Een standaard React-app die een lijst met WKND-avonturen en hun gegevens weergeeft
version: Experience Manager as a Cloud Service
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
source-git-commit: 30b98e82e78120bf9fb13c9d41780af4c07665d8
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 0%

---

# Basic React-app

Deze [&#x200B; Reactie &#x200B;](https://reactjs.org/) app toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted query. Deze toepassing geeft filterbaar van avonturen WKND terug, en na het selecteren van een avontuur, toont de avonturen volledige details.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-slug`

Voor een meer diepgaande overzicht van hoe deze app Next.js wordt gebouwd, herzie het [&#x200B; voorbeeld Reageer app documentatie &#x200B;](../example-apps/react-app.md).
