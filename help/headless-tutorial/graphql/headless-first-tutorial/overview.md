---
title: AEM Headless, eerste zelfstudie
description: Leer hoe u voor het eerst een AEM Headless-toepassing kunt zijn.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 89
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 1%

---

# AEM Headless, eerste zelfstudie

Welkom bij de zelfstudie over het ontwikkelen van een webervaring met React, volledig aangedreven door AEM Headless API&#39;s en GraphQL. In deze zelfstudie begeleiden we u door het proces van het maken van een dynamische en interactieve webtoepassing door de kracht van React, Adobe Experience Manager (AEM) Headless API&#39;s en GraphQL te combineren.

React is een populaire JavaScript-bibliotheek voor het bouwen van gebruikersinterfaces, die bekend staat om de eenvoud, herbruikbaarheid en componentarchitectuur. AEM biedt robuuste mogelijkheden voor inhoudsbeheer en stelt Headless API&#39;s beschikbaar waarmee ontwikkelaars via verschillende kanalen en toepassingen toegang hebben tot inhoud en gegevens die in AEM zijn opgeslagen.

Door gebruik te maken van AEM Headless API&#39;s kunt u inhoud, middelen en gegevens ophalen van uw AEM-instantie en deze gebruiken om uw React-toepassing aan te sturen. GraphQL, een flexibele querytaal voor API&#39;s, biedt een efficiënte en nauwkeurige manier om specifieke gegevens aan te vragen bij uw AEM-instantie, waardoor een naadloze integratie tussen React en AEM mogelijk wordt.

![&#x200B; Hoofdloze Eerste zelfstudie van AEM &#x200B;](./assets/overview/overview.png)

In deze zelfstudie doorlopen we u stap voor stap een webervaring opbouwen met React en AEM Headless API&#39;s met GraphQL. U leert hoe u uw ontwikkelomgeving instelt, een verbinding tot stand brengt tussen React en AEM, inhoud ophaalt met behulp van GraphQL-query&#39;s en deze dynamisch rendert in uw webtoepassing.

Wij zullen onderwerpen zoals het vormen van uw project van de Reactie, het vestigen van authentificatie met AEM, het vragen van inhoud van AEM gebruikend GraphQL, het behandelen van gegevens in uw componenten van de Reactie, en het optimaliseren van prestaties door caching en paginering te gebruiken behandelen.

Aan het einde van deze zelfstudie hebt u een goed inzicht in hoe u React, AEM Headless API&#39;s en GraphQL kunt gebruiken om een krachtige en boeiende webervaring op te bouwen. Dus, laten we induiken en beginnen met het bouwen van uw volgende webtoepassing!

## Vereisten

### Vaardigheden

+ Bevoegdheid in reactie
+ Kennis in GraphQL
+ Basiskennis van AEM as a Cloud Service

### AEM as a Cloud Service

Deze zelfstudie vereist beheerderstoegang tot een AEM as a Cloud Service-omgeving.

### Software

+ [&#x200B; Node.js v16+ &#x200B;](https://nodejs.org/en/)
   + Controleer uw knooppuntversie door `node -v` van de bevellijn in werking te stellen
+ [&#x200B; npm 6+ &#x200B;](https://www.npmjs.com/)
   + Controleer uw npm-versie door `npm -v` vanaf de opdrachtregel uit te voeren
+ [&#x200B; Git &#x200B;](https://git-scm.com/)
   + Controleer uw it-versie door `git -v` uit te voeren vanaf de opdrachtregel

De manager van de knoopversie van het gebruik [&#x200B; (nvm) &#x200B;](https://github.com/nvm-sh/nvm) om het hebben van veelvoudige versies van node.js op de zelfde machine te richten.

Zorg ervoor dat u rechten hebt om software wereldwijd op uw computer te installeren.

## Volgende stap

Nu uw milieu opstelling is, gaan op de volgende stap: [&#x200B; Opstelling en auteursinhoud in AEM as a Cloud Service &#x200B;](./1-content-modeling.md)
