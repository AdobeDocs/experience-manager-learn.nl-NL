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

Onderzoek AEM de capaciteit van GraphQL APIs zonder hoofd om gegevens te filtreren gebruikend a [ Uitdrukkelijke ](https://expressjs.com/) en [ Pug ](https://pugjs.org/) app. Met deze Express-app maakt u een lijst met WKND-avonturen die kunnen worden gefilterd op Type activiteit.

Deze code toont het gebruiken van Adobe [ AEM Koploze Cliënt voor NodeJS ](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) aan om voortgeduurde vragen van GraphQL aan te halen gebruikend op Node.js-Gebaseerde JavaScript. Deze app gebruikt de `wknd-shared/adventures-all` aanhoudend vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot de `wknd-shared/adventures-by-activity` persisted vraag en wint de avontuurdetails voor slechts die avonturen van het gespecificeerde Type van Activiteit terug. De details van het avontuur worden teruggewonnen van AEM via `wknd-shared/adventures-by-slug` persisted vraag.

Deze code:

+ Maakt verbinding met een AEM Publish-service en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, en `wknd-shared/adventures-by-slug`
