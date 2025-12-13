---
title: Aan de slag met AEM Headless - Content Services
description: Een end-to-end tutorial waarin wordt geïllustreerd hoe u content kunt samenstellen en beschikbaar maken met AEM Headless.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 3%

---

# Aan de slag met AEM Headless - Content Services

AEM Content Services maakt gebruik van traditionele AEM Pages om eindpunten van REST API zonder kop samen te stellen, en AEM Components definieert of verwijst naar de inhoud die op deze eindpunten moet worden weergegeven.

Met AEM Content Services kunt u dezelfde inhoudsabstracties gebruiken als voor het ontwerpen van webpagina&#39;s in AEM Sites, om de inhoud en schema&#39;s van deze HTTP API&#39;s te definiëren. Met AEM Pages and AEM Components kunnen marketers snel flexibele JSON API&#39;s samenstellen en bijwerken die elke toepassing kunnen bedienen.

## Zelfstudie voor inhoudsservices

Een end-to-end zelfstudie waarin wordt geïllustreerd hoe u in een CMS-scenario zonder kop inhoud kunt maken en onthullen met AEM en hoe deze inhoud wordt verbruikt door een native mobiele app.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

In deze zelfstudie wordt uitgelegd hoe u AEM Content Services kunt gebruiken om de ervaring van een mobiele app die gebeurtenisinformatie (muziek, prestaties, illustraties, enz.) weergeeft die door het WKND-team wordt beheerd, kracht bij te zetten.

In deze zelfstudie worden de volgende onderwerpen behandeld:

* Inhoud maken die een gebeurtenis vertegenwoordigt met behulp van inhoudfragmenten
* Eindpunten van AEM Content Services definiëren met behulp van AEM Sites&#39; Templates and Pages die de gebeurtenisgegevens als JSON beschikbaar maken
* Ontdek hoe AEM WCM Core Components kan worden gebruikt om marketers in staat te stellen JSON-eindpunten te maken
* AEM Content Services JSON vanuit een mobiele app gebruiken
   * Android wordt gebruikt omdat het een platformonafhankelijke emulator heeft die alle gebruikers (Windows, macOS en Linux) van deze zelfstudie kunnen gebruiken om de native app uit te voeren.

## GitHub-project

De broncode, en inhoudspakketten zijn beschikbaar op [ AEM Guides - het Mobiele Project van GitHub van WKND ](https://github.com/adobe/aem-guides-wknd-mobile).

Als u een kwestie met het leerprogramma of de code vindt, gelieve de kwestie van a [ GitHub ](https://github.com/adobe/aem-guides-wknd-mobile/issues) te verlaten.

## AEM GraphQL versus AEM Content Services

|                                | AEM GraphQL API&#39;s | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schema-definitie | Modellen voor structuurinhoudsfragmenten | AEM-componenten |
| Inhoud | Inhoudsfragmenten | AEM-componenten |
| Inhoud detecteren | Op GraphQL-query | Op AEM-pagina |
| Afleveringsformaat | GraphQL JSON | AEM ComponentExporter JSON |
