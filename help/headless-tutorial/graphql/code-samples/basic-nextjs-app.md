---
title: Basic Next.js, app
description: Een standaard-app Next.js die een lijst met WKND-avonturen en hun details weergeeft
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 22
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 0%

---

# Basic Next.js, app

Dit [ Next.js ](https://nextjs.org/) app toont aan hoe te om inhoud te vragen gebruikend AEM GraphQL APIs gebruikend persisted query. Deze toepassing geeft filterbaar van avonturen WKND terug, en na het selecteren van een avontuur, toont de avonturen volledige details.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-slug`

>[!IMPORTANT]
>
> Codesandbox.io biedt geen ondersteuning voor het bewerken van de toepassing Next.js in de ingesloten IDE. Om deze codesteekproef uit te geven, [ open direct Next.js app op codesandbox.io ](https://codesandbox.io/s/wknd-next-js-app-u8x5f8).
