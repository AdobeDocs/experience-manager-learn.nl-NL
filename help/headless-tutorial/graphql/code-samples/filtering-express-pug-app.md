---
title: Uitdrukkelijke app filteren
description: Een eenvoudige Express-app waarmee WKND-avonturen worden gefilterd die zijn gemodelleerd met Content Fragments.
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Uitdrukkelijke app filteren

Verken AEM Headless GraphQL APIs capaciteit om gegevens te filtreren gebruikend a [&#x200B; Uitdrukkelijke &#x200B;](https://expressjs.com/) en [&#x200B; Pug &#x200B;](https://pugjs.org/) app. Met deze Express-app maakt u een lijst met WKND-avonturen die kunnen worden gefilterd op Type activiteit.

Deze code toont het gebruiken van de Hoofdloze Cliënt van Adobe [&#x200B; AEM voor NodeJS &#x200B;](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) aan om voortgeduurde vragen van GraphQL aan te halen gebruikend op Node.js-Gebaseerde JavaScript. Deze app gebruikt de `wknd-shared/adventures-all` aanhoudend vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot de `wknd-shared/adventures-by-activity` persisted vraag en wint de avontuurdetails voor slechts die avonturen van het gespecificeerde Type van Activiteit terug. Adventure-details worden opgehaald uit AEM via de `wknd-shared/adventures-by-slug` persisted query.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all`, `wknd-shared/adventures-by-activity`, en `wknd-shared/adventures-by-slug`
