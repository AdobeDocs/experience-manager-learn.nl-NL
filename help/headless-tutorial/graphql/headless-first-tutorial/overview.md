---
title: Eerste zelfstudie AEM zonder hoofd
description: Leer hoe u een eerste toepassing zonder koptekst AEM.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 1%

---

# Eerste zelfstudie AEM zonder hoofd

{{aem-headless-trials-promo}}

Welkom bij de zelfstudie over het ontwikkelen van een webervaring met React, volledig aangedreven door AEM headless API&#39;s en GraphQL. In deze zelfstudie begeleiden we u door het proces van het maken van een dynamische en interactieve webtoepassing door de kracht van React, Adobe Experience Manager (AEM) Headless API&#39;s en GraphQL te combineren.

React is een populaire bibliotheek JavaScript voor het bouwen van gebruikersinterfaces, die voor zijn eenvoud, herbruikbaarheid, en op component-gebaseerde architectuur wordt gekend. AEM biedt robuuste mogelijkheden voor inhoudsbeheer en stelt Headless API&#39;s beschikbaar waarmee ontwikkelaars toegang hebben tot inhoud en gegevens die in AEM zijn opgeslagen via een groot aantal kanalen en toepassingen.

Door gebruik te maken van AEM headless API&#39;s kunt u inhoud, elementen en gegevens ophalen van uw AEM-instantie en deze gebruiken om uw React-toepassing aan te sturen. GraphQL, een flexibele querytaal voor API&#39;s, biedt een efficiënte en nauwkeurige manier om specifieke gegevens van uw AEM-instantie aan te vragen, waardoor een naadloze integratie tussen React en AEM mogelijk wordt.

![AEM eerste zelfstudie zonder hoofd](./assets/overview/overview.png)

Tijdens deze zelfstudie doorlopen we u stap voor stap een webervaring opbouwen met React en AEM Headless API&#39;s met GraphQL. U leert hoe u uw ontwikkelomgeving kunt instellen, een verbinding tot stand kunt brengen tussen Reageren en AEM, inhoud kunt ophalen met GraphQL-query&#39;s en deze dynamisch kunt renderen in uw webtoepassing.

Wij zullen onderwerpen zoals het vormen van uw project van het Reageren, het vestigen van authentificatie met AEM, het vragen van inhoud van AEM gebruikend GraphQL, het behandelen van gegevens in uw componenten van het Reageren, en het optimaliseren van prestaties door caching en paginering te gebruiken behandelen.

Aan het einde van deze zelfstudie hebt u een goed inzicht in hoe u React, AEM Headless API&#39;s en GraphQL kunt gebruiken om een krachtige en boeiende webervaring op te bouwen. Dus, laten we induiken en beginnen met het bouwen van uw volgende webtoepassing!

## Vereisten

### Vaardigheden

+ Bevoegdheid in reactie
+ Kennis in GraphQL
+ Basiskennis van AEM as a Cloud Service

### AEM as a Cloud Service

Deze zelfstudie vereist beheerderstoegang tot een AEM as a Cloud Service omgeving.

### Software

+ [Node.js v16+](https://nodejs.org/en/)
   + De nodeversie controleren door uit te voeren `node -v` vanaf de opdrachtregel
+ [npm 6+](https://www.npmjs.com/)
   + Controleer uw npm-versie door uit te voeren `npm -v` vanaf de opdrachtregel
+ [Git](https://git-scm.com/)
   + Uw it-versie controleren door uit te voeren `git -v` vanaf de opdrachtregel

Gebruiken [node version Manager (nvm)](https://github.com/nvm-sh/nvm) om het hebben van veelvoudige versies van node.js op de zelfde machine te richten.

Zorg ervoor dat u rechten hebt om software wereldwijd op uw computer te installeren.

## Volgende stap

Nu uw omgeving is ingesteld, gaan we naar de volgende stap: [Inhoud instellen en ontwerpen in AEM as a Cloud Service](./1-content-modeling.md)
