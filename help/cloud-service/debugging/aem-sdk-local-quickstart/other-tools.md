---
title: Andere gereedschappen voor foutopsporing in AEM SDK
description: Een verscheidenheid van andere hulpmiddelen kan helpen bij het zuiveren van de lokale QuickStart van AEM SDK.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
duration: 107
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 0%

---

# Andere gereedschappen voor foutopsporing in AEM SDK

Een verscheidenheid van andere hulpmiddelen kan helpen bij het zuiveren van uw toepassing op AEM SDK lokale quickstart.

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite is een webinterface voor interactie met de gegevensopslagruimte van de JCR, AEM. CRXDE Lite biedt volledige zichtbaarheid in het JCR, inclusief knooppunten, eigenschappen, eigenschapswaarden en machtigingen.

CRXDE Lite bevindt zich in:

+ Gereedschappen > Algemeen > CRXDE Lite
+ of direct bij [ http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### Fouten opsporen in inhoud

CRXDE Lite biedt directe toegang tot het JCR. De inhoud die via CRXDE Lite zichtbaar is, wordt beperkt door de machtigingen die aan uw gebruiker zijn verleend. Dit houdt in dat u mogelijk niet alles in het JCR kunt zien of wijzigen, afhankelijk van uw toegang.

+ U kunt met het linkernavigatievenster navigeren door de JCR-structuur en deze manipuleren
+ Wanneer u een knooppunt in het navigatievenster aan de linkerkant selecteert, wordt de eigenschap node in het onderste deelvenster beschikbaar gemaakt.
   + Eigenschappen kunnen vanuit het deelvenster worden toegevoegd, verwijderd of gewijzigd
+ Als u dubbelklikt op een bestandsknooppunt in de linkernavigatie, wordt de inhoud van het bestand geopend in het rechterbovenvenster
+ Tik op de knop Alles opslaan linksboven om de wijzigingen te handhaven of op de pijl omlaag naast Alles opslaan om alle niet-opgeslagen wijzigingen te herstellen.

![ CRXDE Lite - het Zuiveren Inhoud ](./assets/other-tools/crxde-lite__debugging-content.png)

Wijzigingen die rechtstreeks via CRXDE Lite in AEM SDK worden aangebracht, kunnen moeilijk te volgen en te besturen zijn. Indien nodig zorgt u ervoor dat wijzigingen die via CRXDE Lite zijn aangebracht, terugkeren naar de verwisselbare inhoudspakketten van het AEM-project (`ui.content`) en doorvoeren in Git. In het ideale geval komen alle wijzigingen in de toepassingsinhoud van de basis van de code en gaan deze via implementaties naar AEM SDK, in plaats van rechtstreeks wijzigingen aan te brengen in de AEM SDK via CRXDE Lite.

### Toegangsbesturingselementen voor foutopsporing

CRXDE Lite biedt een manier om toegangsbeheer voor een specifiek knooppunt voor een specifieke gebruiker of groep (ook wel principal genoemd) te testen en te evalueren.

Ga naar:

+ CRXDE Lite > Extra > Toegangsbeheer testen...

![ CRXDE Lite - de Controle van de Toegang van de Test ](./assets/other-tools/crxde-lite__test-access-control.png)

1. Selecteer in het veld Pad een JCR-pad dat u wilt evalueren
1. Selecteer in het veld Hoofd de gebruiker of de groep aan de hand waarvan u het pad wilt beoordelen
1. Tik op de knop Testen

De resultaten worden hieronder weergegeven:

+ __Weg__ herhaalt de weg die werd geëvalueerd
+ __Belangrijk__ herhaalt de gebruiker of de groep waarvoor de weg werd geëvalueerd
+ __Belangrijkste__ maakt een lijst van alle hoofden het geselecteerde hoofd deel van is.
   + Dit is nuttig om het overgangsgroepslidmaatschap te begrijpen dat toestemmingen via overerving kan verlenen
+ __Bevoegdheden bij Weg__ maakt een lijst van alle JCR toestemmingen het geselecteerde hoofd op de geëvalueerde weg heeft

## Query uitvoeren

![ verklaart Vraag ](./assets/other-tools/explain-query.png)

Verklaar het Web-based hulpmiddel van de Vraag in AEM SDK lokale quickstart, die zeer belangrijke inzichten in verstrekt hoe AEM interpreteert en vragen uitvoert, en een onschatbaar hulpmiddel om ervoor te zorgen vragen op een prestatieswijze door AEM worden uitgevoerd.

Verklaar de Vraag wordt gevestigd bij:

+ Gereedschappen > Diagnose > Query-prestaties > Het tabblad Query uitvoeren
+ [ http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) > verklaart het lusje van de Vraag

## QueryBuilder-foutopsporing

![ Foutopsporing QueryBuilder ](./assets/other-tools/query-debugger.png)

Foutopsporing van QueryBuilder is web-based hulpmiddel dat u helpt onderzoeksvragen zuiveren en begrijpen gebruikend de syntaxis van AEM [ QueryBuilder ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

Foutopsporing van QueryBuilder bevindt zich in:

+ [ http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
