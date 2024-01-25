---
title: Filteren met jQuery en Handlebars
description: Een JavaScript-implementatie met jQuery en Handlebars die WKND-avonturen filtert om weer te geven. .
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
duration: 30
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Filteren met jQuery en Handlebars

Ontdek AEM GraphQL API&#39;s zonder koppen de mogelijkheid om gegevens te filteren met een JavaScript-app die [jQuery](https://jquery.com/) en [Werkbalken](https://handlebarsjs.com/). Deze app maakt een lijst met WKND-avonturen die kunnen worden gefilterd op Type activiteit.

Deze code demonstreert het gebruik van de Adobe [AEM headless-client voor JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) om voortgezette GraphQL-query&#39;s aan te roepen. Deze app gebruikt de `wknd-shared/adventures-all` Voortdurende vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot `wknd-shared/adventures-by-activity` persisted query en haalt de avontuurdetails voor alleen die avonturen van het opgegeven Type activiteit op.

Deze code:

+ Verbindt met de AEM publicatieservice, en vereist geen authentificatie
+ Gebruikt de hardnekkige vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
