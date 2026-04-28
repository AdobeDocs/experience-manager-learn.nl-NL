---
title: Simple SvelteKit-app
description: Een eenvoudige SvelteKit-app waarmee WKND-avonturen worden weergegeven die zijn gemodelleerd met Content Fragments.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
duration: 22
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 0%

---

# De SvelteKit-app filteren

Verken AEM Headless GraphQL APIs capaciteit om van gegevens een lijst te maken gebruikend a [ SvelteKit ](https://kit.svelte.dev/) app. Deze SvelteKit-app maakt een lijst met WKND-avonturen die kunnen worden geselecteerd om de details van het avontuur weer te geven.

Deze code toont het gebruiken van de Hoofdloze Cliënt van Adobe [ AEM voor JavaScript ](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) aan om voortgeduurde vragen van GraphQL van SvelteKit aan te halen. Deze app gebruikt de `wknd-shared/adventures-all` aanhoudend vraag om alle avonturen te verzamelen, en een lijst van beschikbare Types van Activiteit af te leiden. Adventure-details worden aangevraagd via de `wknd-shared/adventures-by-slug` persisted query.

Deze code:

+ Maakt verbinding met een AEM-publicatieservice en vereist geen verificatie
+ Gebruikt de aanhoudend vragen van WKND: `wknd-shared/adventures-all` en `wknd-shared/adventures-by-slug`
