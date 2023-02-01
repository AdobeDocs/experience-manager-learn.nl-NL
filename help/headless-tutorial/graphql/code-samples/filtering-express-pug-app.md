---
title: ExpressJS- en Pug-app filteren
description: Een eenvoudige toepassing ExpressJS/Pug waarmee WKND-avonturen worden gefilterd die zijn gemodelleerd met Inhoudsfragmenten.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---


# ExpressJS- en Pug-app filteren

Ontdek AEM GraphQL API&#39;s zonder koppen die gegevens kunnen filteren met behulp van een [ExpressJS](https://expressjs.com/)/[Pug](https://pugjs.org/) app. Met deze toepassing ExpressJS/Pug wordt een lijst met WKND-avonturen gemaakt, die op Type activiteit kunnen worden gefilterd.

Deze code demonstreert het gebruik van Adobe [AEM Headless-client voor NodeJS](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) om voortgezette GraphQL-query&#39;s aan te roepen met JavaScript op basis van Node.js. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot `wknd-shared/adventures-by-activity` persisted query en haalt de avontuurdetails voor alleen die avonturen van het opgegeven Type activiteit op.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
