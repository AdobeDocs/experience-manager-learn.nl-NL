---
title: Basic Next.js, app
description: Een standaard-app Next.js die een lijst met WKND-avonturen en hun details weergeeft
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: e3fb145e7a9f33dd010f6c40e42573d41e54b302
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---


# Basic Next.js, app

Dit [Next.js](https://nextjs.org/) app laat zien hoe u query&#39;s uitvoert op inhoud met AEM GraphQL API&#39;s met behulp van doorlopende query&#39;s. Deze toepassing geeft filterbaar van avonturen WKND terug, en na het selecteren van een avontuur, toont de avonturen volledige details.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-slug`

Voor een diepgaande beoordeling van de manier waarop deze Next.js-app is gemaakt, raadpleegt u de [voorbeeld documentatie bij de toepassing Next.js](../example-apps/next-js.md).

>[!IMPORTANT]
>
> Codesandbox.io biedt geen ondersteuning voor het bewerken van de toepassing Next.js in de ingesloten IDE. U bewerkt dit codevoorbeeld als volgt: [Open de toepassing Next.js rechtstreeks op codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-3n6zdv).