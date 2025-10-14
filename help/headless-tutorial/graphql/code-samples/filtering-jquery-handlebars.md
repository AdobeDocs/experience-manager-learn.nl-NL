---
title: Filteren met jQuery en Handlebars
description: Een JavaScript-implementatie met jQuery en Handlebars die WKND-avonturen filtert op weergave. .
version: Experience Manager as a Cloud Service
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
duration: 26
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Filteren met jQuery en Handlebars

Verken AEM Headless GraphQL APIs capaciteit om gegevens te filtreren gebruikend een app van JavaScript die [&#x200B; jQuery &#x200B;](https://jquery.com/) en [&#x200B; Handlebars &#x200B;](https://handlebarsjs.com/) gebruikt. Deze app maakt een lijst met WKND-avonturen die kunnen worden gefilterd op Type activiteit.

Deze code toont het gebruiken van de Hoofdloze Cliënt van Adobe [&#x200B; AEM voor JavaScript &#x200B;](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) aan om voortgeduurde vragen van GraphQL aan te halen. Deze app gebruikt de `wknd-shared/adventures-all` aanhoudend vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Wanneer een gebruiker een Type van Activiteit selecteert, wordt het geselecteerde type overgegaan tot de `wknd-shared/adventures-by-activity` persisted vraag en wint de avontuurdetails voor slechts die avonturen van het gespecificeerde Type van Activiteit terug.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-activity`
