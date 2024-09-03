---
title: Aanklikbare afbeeldingscomponent maken
description: Aanklikbare afbeeldingscomponent maken in AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Inleiding

Door klikbare afbeeldingen in Forms te gebruiken, kunt u een boeiender, intuïtiever en visueel aantrekkelijke gebruikerservaring creëren. In het kader van dit artikel hebben we SVG gebruikt voor klikbare afbeeldingen, omdat dit verschillende voordelen biedt, met name op het gebied van ontwerpflexibiliteit, prestaties en gebruikerservaring.
SVG kan worden gemaakt met Adobe Illustrator of een van de gratis onlinegereedschappen. Ik heb de [ Kaart van de V.S. van ](https://simplemaps.com/resources/svg-us) vereenvoudigingen gebruikt om het gebruiksgeval aan te tonen.

## Hoofdlettergebruik gebruiken voor het gebruik van een aanklikbare VS-kaart

Op de aanklikbare kaart van de VS kunnen gebruikers de verzending van specifieke formulieren bekijken. Wanneer een gebruiker op een status klikt, worden de verzendingen vanuit die status weergegeven, met de optie om een specifieke verzending te openen.
