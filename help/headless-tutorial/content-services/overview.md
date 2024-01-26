---
title: Aan de slag met AEM headless - Content Services
description: Een end-to-end tutorial waarin wordt geïllustreerd hoe u content kunt samenstellen en beschikbaar maken met AEM Headless.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 333
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 3%

---

# Aan de slag met AEM headless - Content Services

AEM Content Services maakt gebruik van traditionele AEM Pagina&#39;s om eindpunten van REST API zonder kop samen te stellen, en AEM Components definiëren (verwijzen naar) de inhoud die op deze eindpunten moet worden weergegeven.

AEM Content Services biedt dezelfde inhoudsabstracties die worden gebruikt voor het ontwerpen van webpagina&#39;s in AEM Sites, om de inhoud en schema&#39;s van deze HTTP API&#39;s te definiëren. Door het gebruik van AEM Pagina&#39;s en AEM Componenten kunnen marketers snel flexibele JSON API&#39;s samenstellen en bijwerken die elke toepassing van stroom kunnen voorzien.

## Zelfstudie voor inhoudsservices

Een end-to-end zelfstudie waarin wordt geïllustreerd hoe u in een CMS-scenario inhoud kunt ontwikkelen en beschikbaar kunt maken met behulp van AEM en wordt verbruikt door een systeemeigen mobiele app.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

In deze zelfstudie wordt uitgelegd hoe AEM Content Services kan worden gebruikt om de ervaring van een mobiele app die gebeurtenisinformatie weergeeft (muziek, prestaties, illustraties, enz.), te verbeteren. die wordt geleid door het WKND-team.

In deze zelfstudie worden de volgende onderwerpen behandeld:

* Inhoud maken die een gebeurtenis vertegenwoordigt met behulp van inhoudfragmenten
* Definieer eindpunten voor AEM Content Services met behulp van de sjablonen en pagina&#39;s van AEM Sites die de gebeurtenisgegevens weergeven als JSON
* Ontdek hoe AEM WCM Core Components kan worden gebruikt om marketers in staat te stellen JSON-eindpunten te maken
* AEM Content Services JSON vanuit een mobiele app gebruiken
   * Android wordt gebruikt omdat het een platformonafhankelijke emulator heeft die alle gebruikers (Windows, macOS en Linux) van deze zelfstudie kunnen gebruiken om de native app uit te voeren.

## GitHub-project

De broncode en inhoudspakketten zijn beschikbaar op de [AEM - WKND Mobile GitHub Project](https://github.com/adobe/aem-guides-wknd-mobile).

Als u een probleem hebt met de zelfstudie of de code, kunt u een [GitHub-probleem](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## AEM GraphQL versus AEM Content Services

|                                | GraphQL API&#39;s AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schema-definitie | Modellen voor structuurinhoudsfragmenten | AEM componenten |
| Inhoud | Inhoudsfragmenten | AEM componenten |
| Inhoud detecteren | Op GraphQL-query | Op AEM pagina |
| Afleveringsformaat | GraphQL JSON | AEM ComponentExporter JSON |
