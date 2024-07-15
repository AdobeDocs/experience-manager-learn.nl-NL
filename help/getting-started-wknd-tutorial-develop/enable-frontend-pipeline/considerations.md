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
duration: 79
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Ontwikkelingsoverwegingen

Nadat u de front-end pijplijn hebt ingeschakeld om alleen de front-end bronnen in AEM as a Cloud Service-omgeving te implementeren, is er enige invloed op de lokale AEM ontwikkeling en moet u het git vertakkingsmodel aanpassen.

## Doelstelling

* Hoe te om een frictionless front-end en achterste ontwikkelingsstroom te hebben
* Herzie de gebiedsdelen tussen de full-stack en front-end pijpleiding


## Overwegingen voor lokale ontwikkeling

>[!VIDEO](https://video.tv.adobe.com/v/3409421?quality=12&learn=on)


## Aangepaste ontwikkelingsaanpak

* Voor de lokale ontwikkeling die AEM SDK gebruikt, heeft het back-end dev-team nog steeds clientlib-generatie via de module `ui.frontend` nodig, maar tijdens de implementatie van Cloud Manager naar de AEM as a Cloud Service-omgeving moet u dit team overslaan. Dit oppervlakt een uitdaging op hoe te om de project te isoleren config veranderingen die in het [ worden geschetst 1} hoofdstuk van het Project van de Update {worden geschetst.](update-project.md)

A __oplossing__ zou uw git vertakkend model kunnen aanpassen en ervoor zorgen de AEM projectconfig verandert nooit terugstromen naar de __lokale ontwikkeling__ tak het AEM achterste-eindontwikkelaarsgebruik.


* Als onderdeel van een voortdurende verbetering van uw AEM project, als u nieuwe componenten introduceert of een bestaande component bijwerkt die veranderingen in zowel `ui.app` als `ui.frontend` module heeft, moet u zowel volledig-stapel als front-end pijpleidingen in werking stellen.
