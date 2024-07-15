---
title: Angular-app filteren
description: Een eenvoudige Angular-app die WKND-avonturen filtert die zijn gemodelleerd met Content Fragments.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
duration: 26
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Angular-app filteren

Onderzoek AEM de capaciteit van GraphQL APIs zonder hoofd om gegevens te filtreren gebruikend app van de a [ Angular ](https://angular.io/). Deze Angular-app maakt een lijst met WKND-avonturen die kunnen worden gefilterd op Type activiteit.

Deze code toont het gebruiken van Adobe [ AEM Koploze Cliënt voor JavaScript ](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) aan om voortgeduurde vragen van GraphQL van Angular aan te halen. Deze app gebruikt de `wknd-shared/adventures-all` aanhoudend vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot de `wknd-shared/adventures-by-activity` persisted vraag en wint de avontuurdetails voor slechts die avonturen van het gespecificeerde Type van Activiteit terug.

Deze code:

+ Maakt verbinding met een AEM Publish-service en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
