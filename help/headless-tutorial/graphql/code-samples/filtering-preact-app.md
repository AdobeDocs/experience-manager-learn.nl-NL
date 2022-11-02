---
title: Preact-app filteren
description: Een eenvoudige Preact-app die WKND-avonturen filtert die zijn gemodelleerd met Content Fragments.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11389
thumbnail: KT-11389.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: a21b78456354c18ad137e69a5d18258d652169b1
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# Preact-app filteren

Ontdek de mogelijkheid AEM Headless GraphQL API&#39;s om gegevens te filteren met behulp van een [Precies](https://preactjs.com/) app. Deze Preact app leidt tot een lijst van avonturen WKND die door Type van Activiteit kunnen worden gefilterd.

Deze code demonstreert het gebruik van Adobe [AEM headless-client voor JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) om voortgeduurde vragen GraphQL van React aan te halen. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot `wknd-shared/adventures-by-activity` persisted query en haalt de avontuurdetails voor alleen die avonturen van het opgegeven Type activiteit op.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
