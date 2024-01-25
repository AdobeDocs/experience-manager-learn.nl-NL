---
title: Reactie-app filteren
description: Een eenvoudige React-app waarmee WKND-avonturen worden gefilterd die zijn gemodelleerd met Content Fragments.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11132
thumbnail: KT-11132.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 1eb9487e-a82a-4d15-a776-cf004f2e3f01
duration: 28
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Reactie-app filteren

Ontdek AEM GraphQL API&#39;s zonder koppen die gegevens kunnen filteren met een [Reageren](https://reactjs.org/) app. Deze React app leidt tot een lijst van avonturen WKND die door Type van Activiteit kunnen worden gefilterd.

Deze code demonstreert het gebruik van de Adobe [AEM headless-client voor JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) om aanhoudende GraphQL-query&#39;s van React aan te roepen. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot `wknd-shared/adventures-by-activity` persisted query en haalt de avontuurdetails voor alleen die avonturen van het opgegeven Type activiteit op.

Deze code:

+ Verbindt met de AEM publicatieservice, en vereist geen authentificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
