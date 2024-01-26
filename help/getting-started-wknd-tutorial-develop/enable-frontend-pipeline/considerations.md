---
title: Ontwikkelingsoverwegingen
description: Overweeg de invloed op het front-end en back-end ontwikkelingsproces zodra u de front-end pijpleiding toelaat.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: a3b27d5b-b167-4c60-af49-8f2e8d814c86
duration: 89
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Ontwikkelingsoverwegingen

Na het toelaten van de front-end pijpleiding om de front-end middelen in AEM as a Cloud Service milieu slechts op te stellen, is er één of andere invloed op de lokale AEM ontwikkeling en u moet het git vertakkende model aanpassen.

## Doelstelling

* Hoe te om een frictionless front-end en achterste ontwikkelingsstroom te hebben
* Herzie de gebiedsdelen tussen de full-stack en front-end pijpleiding


## Overwegingen voor lokale ontwikkeling

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Aangepaste ontwikkelingsaanpak

* Voor de lokale ontwikkeling die AEM SDK gebruikt, heeft het back-end dev-team nog steeds clientlib genereren via `ui.frontend` maar tijdens de implementatie van Cloud Manager in AEM as a Cloud Service omgeving moet u deze overslaan. Dit oppervlakte een uitdaging op hoe te om de project te isoleren config veranderingen die in worden geschetst in [Project bijwerken](update-project.md) hoofdstuk.

A __oplossing__ zou kunnen zijn om uw git vertakkend model aan te passen en ervoor te zorgen de AEM project config veranderingen nooit terugstroom naar __lokale ontwikkeling__ vertakken het AEM achterste-eindontwikkelaars gebruiken.


* Als deel van een aan de gang zijnde verhoging aan uw AEM project, als u nieuwe componenten introduceert of een bestaande component bijwerkt die veranderingen in allebei heeft `ui.app` en `ui.frontend` moet u zowel volledige-stapel als front-end pijpleidingen in werking stellen.
