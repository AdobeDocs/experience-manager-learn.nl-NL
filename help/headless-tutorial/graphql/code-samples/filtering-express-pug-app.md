---
title: Uitdrukkelijke app filteren
description: Een eenvoudige Express-app waarmee WKND-avonturen worden gefilterd die zijn gemodelleerd met Content Fragments.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
duration: 29
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Uitdrukkelijke app filteren

Ontdek AEM GraphQL API&#39;s zonder koppen die gegevens kunnen filteren met een [Express](https://expressjs.com/) en [Pug](https://pugjs.org/) app. Met deze Express-app maakt u een lijst met WKND-avonturen die kunnen worden gefilterd op Type activiteit.

Deze code demonstreert het gebruik van de Adobe [AEM Headless-client voor NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) om voortgezette GraphQL-query&#39;s aan te roepen met JavaScript op basis van Node.js. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot `wknd-shared/adventures-by-activity` persisted query en haalt de avontuurdetails voor alleen die avonturen van het opgegeven Type activiteit op. De details van het avontuur worden teruggewonnen van AEM via `wknd-shared/adventures-by-slug` voortgezette query.

Deze code:

+ Verbindt met de AEM publicatieservice, en vereist geen authentificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, en `wknd-shared/adventures-by-slug`
