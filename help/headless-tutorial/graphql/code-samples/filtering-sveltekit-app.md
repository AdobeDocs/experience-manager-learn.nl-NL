---
title: De SvelteKit-app filteren
description: Een eenvoudige SvelteKit-app waarmee WKND-avonturen worden gefilterd die zijn gemodelleerd met Content Fragments.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
hidefromtoc: true
source-git-commit: c96b8c9761ff9477fda40d641db5021994b32754
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---


# De SvelteKit-app filteren

Ontdek AEM GraphQL API&#39;s zonder koppen die gegevens kunnen filteren met behulp van een [SvelteKit](https://kit.svelte.dev/) app. Deze SvelteKit-app maakt een lijst met WKND-avonturen die kunnen worden gefilterd op Type activiteit.

Deze code demonstreert het gebruik van Adobe [AEM headless-client voor JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) om aanhoudende GraphQL-query&#39;s van SvelteKit aan te roepen. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot `wknd-shared/adventures-by-activity` persisted query en haalt de avontuurdetails voor alleen die avonturen van het opgegeven Type activiteit op.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`