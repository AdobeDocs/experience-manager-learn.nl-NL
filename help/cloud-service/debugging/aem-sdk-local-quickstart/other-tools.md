---
title: Andere tools voor foutopsporing AEM SDK
description: Een verscheidenheid van andere hulpmiddelen kan helpen bij het zuiveren van de lokale snelle start van AEM SDK.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 1%

---


# Andere tools voor foutopsporing AEM SDK

Een verscheidenheid van andere hulpmiddelen kan helpen bij het zuiveren van uw toepassing op de lokale snelle start van AEM SDK.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite is een webinterface voor interactie met het JCR, AEM gegevensopslagruimte. CRXDE Lite biedt volledige zichtbaarheid in het JCR, inclusief knooppunten, eigenschappen, eigenschapswaarden en machtigingen.

CRXDE Lite bevindt zich in:

+ Gereedschappen > Algemeen > CRXDE Lite
+ of rechtstreeks op [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

## Query uitvoeren

![Query uitvoeren](./assets/other-tools/explain-query.png)

Verklaar het Web-based hulpmiddel van de Vraag in AEM lokale QuickStart van SDK, die zeer belangrijke inzichten in verstrekt hoe AEM en vragen interpreteert uitvoert, en een onschatbaar hulpmiddel om vragen te verzekeren op een prestatieswijze door AEM worden uitgevoerd.

Verklaar de Vraag wordt gevestigd bij:

+ Gereedschappen > Diagnose > Query-prestaties > Het tabblad Query uitvoeren
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > Het tabblad Query uitvoeren

## QueryBuilder-foutopsporing

![QueryBuilder-foutopsporing](./assets/other-tools/query-debugger.png)

Foutopsporing van QueryBuilder is web-based hulpmiddel dat u helpt onderzoeksvragen zuiveren en begrijpen gebruikend AEM syntaxis [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) .

Foutopsporing van QueryBuilder bevindt zich op de volgende locatie:

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log Tracer en AEM Chrome plug-in

![Sling Log Tracer en AEM Chrome plug-in](./assets/other-tools/log-tracer.png)

[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html), die wordt geleverd met AEM lokale QuickStart van SDK, maakt het mogelijk om HTTP-verzoeken grondig te traceren, waarbij diepgaande foutopsporingsinformatie per aanvraag wordt weergegeven. De configuratie OSGi van de Traceur van het [Logboek moet worden gevormd](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1) om deze eigenschap toe te laten.

De open-source [AEM Chrome-plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US) voor de [Google Chrome-webbrowser](https://www.google.com/chrome/), wordt geïntegreerd met Log Tracer en geeft de foutopsporingsinformatie rechtstreeks weer in de Dev Tools van Chrome.

_De AEM Chrome-plug-in is een opensource-programma en Adobe biedt hiervoor geen ondersteuning._

