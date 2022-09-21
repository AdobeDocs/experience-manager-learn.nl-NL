---
title: Ontwikkelingsoverwegingen
description: Overweeg de invloed op het front-end en back-end ontwikkelingsproces zodra u de front-end pijpleiding toelaat.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 2e3615e9e9305165ca9c3c93b38ac7e9bdcc51fb
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

>[!VIDEO](https://video.tv.adobe.com/v/3409421/)


## Aangepaste ontwikkelingsaanpak

* Voor de lokale ontwikkeling die AEM SDK gebruikt, heeft het back-end dev-team nog steeds clientlib-generatie nodig via `ui.frontend` maar tijdens de implementatie van Cloud Manager in AEM as a Cloud Service omgeving moet u deze overslaan. Dit oppervlakte een uitdaging op hoe te om de project te isoleren config veranderingen die in worden geschetst in [Project bijwerken](update-project.md) hoofdstuk

A __oplossing__ zou kunnen zijn om uw git vertakkend model aan te passen en ervoor te zorgen de AEM project config veranderingen nooit terugstroom naar __lokale ontwikkeling__ vertakken het AEM achterste-eindontwikkelaars gebruiken.


* Als deel van een aan de gang zijnde verhoging aan uw AEM project, als u nieuwe componenten introduceert of een bestaande component bijwerkt die veranderingen in allebei heeft `ui.app` en `ui.frontend` moet u zowel volledige-stapel als front-end pijpleidingen in werking stellen.



