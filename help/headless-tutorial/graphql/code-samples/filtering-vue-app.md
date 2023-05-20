---
title: Kleurtoon-app filteren
description: Een eenvoudige Vue-app die WKND-avonturen filtert die zijn gemodelleerd met Content Fragments.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11366
thumbnail: KT-11366.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 8f96093a-4449-4249-9257-028e2ffd979b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# Kleurtoon-app filteren

Ontdek AEM GraphQL API&#39;s zonder koppen die gegevens kunnen filteren met behulp van een [Waarde](https://vuejs.org/) app. Deze React app leidt tot een lijst van avonturen WKND die door Type van Activiteit kunnen worden gefilterd.

Deze code demonstreert het gebruik van Adobe [AEM headless-client voor JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) om voortgeduurde vragen van GraphQL van Vue aan te halen. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot `wknd-shared/adventures-by-activity` persisted query en haalt de avontuurdetails voor alleen die avonturen van het opgegeven Type activiteit op.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
