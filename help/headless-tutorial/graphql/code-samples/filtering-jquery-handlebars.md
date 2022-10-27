---
title: Filteren met jQuery en Handlebars
description: Een JavaScript-implementatie met jQuery en Handlebars die WKND-avonturen filtert om weer te geven. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: 74510a4b075d2dba9b3f27018ba05f15dcad9562
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---


# Filteren met jQuery en Handlebars

Ontdek de mogelijkheid AEM Headless GraphQL API&#39;s om gegevens te filteren met een JavaScript-app die gebruikmaakt van [jQuery](https://jquery.com/) en [Werkbalken](https://handlebarsjs.com/). Deze app maakt een lijst met WKND-avonturen die kunnen worden gefilterd op Type activiteit.

Deze code demonstreert het gebruik van Adobe [AEM headless-client voor JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) om voortgeduurde vragen aan te halen GraphQL. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot `wknd-shared/adventures-by-activity` persisted query en haalt de avontuurdetails voor alleen die avonturen van het opgegeven Type activiteit op.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
