---
title: Reactie-app filteren
description: Een eenvoudige React-app waarmee WKND-avonturen worden gefilterd die zijn gemodelleerd met Content Fragments.
version: Experience Manager as a Cloud Service
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
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Reactie-app filteren

Verken AEM Headless GraphQL APIs capaciteit om gegevens te filtreren gebruikend a [&#x200B; Reageer &#x200B;](https://reactjs.org/) app. Deze React app leidt tot een lijst van avonturen WKND die door Type van Activiteit kunnen worden gefilterd.

Deze code toont het gebruiken van de Hoofdloze Cliënt van Adobe [&#x200B; AEM voor JavaScript &#x200B;](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) aan om voortgeduurde vragen van GraphQL van React aan te halen. Deze app gebruikt de `wknd-shared/adventures-all` aanhoudend vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot de `wknd-shared/adventures-by-activity` persisted vraag en wint de avontuurdetails voor slechts die avonturen van het gespecificeerde Type van Activiteit terug.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
