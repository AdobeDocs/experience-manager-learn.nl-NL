---
title: Simple SvelteKit-app
description: Een eenvoudige SvelteKit-app waarmee WKND-avonturen worden weergegeven die zijn gemodelleerd met Content Fragments.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
source-git-commit: a0a1c7e5d3dd74454b9b8ab787ce7447e73ee098
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---


# De SvelteKit-app filteren

Ontdek AEM GraphQL API&#39;s zonder koppen die gegevens kunnen weergeven met een [SvelteKit](https://kit.svelte.dev/) app. Deze SvelteKit-app maakt een lijst met WKND-avonturen die kunnen worden geselecteerd om de details van het avontuur weer te geven.

Deze code demonstreert het gebruik van Adobe [AEM headless-client voor JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) om aanhoudende GraphQL-query&#39;s van SvelteKit aan te roepen. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Adventure-details worden aangevraagd via de `wknd-shared/adventures-by-slug` voortgezette query.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-slug`
