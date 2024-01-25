---
title: CRXDE Lite
description: CRXDE Lite is een klassiek, maar krachtig hulpmiddel voor het zuiveren AEM as a Cloud Service milieu's van de Ontwikkelaar. CRXDE Lite verstrekt een reeks van functionaliteit die het zuiveren van het inspecteren van alle middelen en eigenschappen, het manipuleren van de veranderlijke gedeelten van JCR en het onderzoeken van toestemmingen helpt.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 140
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Foutopsporing AEM as a Cloud Service met CRXDE Lite

CRXDE Lite is __ALLEEN__ beschikbaar in AEM as a Cloud Service ontwikkelomgevingen (en de lokale AEM SDK).

## CRXDE Lite benaderen bij AEM auteur

CRXDE Lite is __alleen__ toegankelijk zijn in AEM as a Cloud Service-ontwikkelingsomgevingen en __niet__ beschikbaar in werkgebied- of productieomgevingen.

CRXDE Lite benaderen op AEM auteur:

1. Meld u aan bij de AEM as a Cloud Service AEM Auteur-service.
1. Ga naar Gereedschappen > Algemeen > CRXDE Lite

Hiermee wordt CRXDE Lite geopend met de referenties en machtigingen waarmee u zich aanmeldt bij AEM auteur.

## Fouten opsporen in inhoud

CRXDE Lite biedt directe toegang tot het JCR. De inhoud die zichtbaar is via CRXDE Lite, wordt beperkt door de machtigingen die aan uw gebruiker zijn verleend. Dit houdt in dat u mogelijk niet alles in het JCR kunt zien of wijzigen, afhankelijk van uw toegang.

Let op: `/apps`, `/libs` en `/oak:index` zijn onveranderbaar, wat betekent dat ze niet kunnen worden gewijzigd bij uitvoering door een gebruiker. Deze locaties in het JCR kunnen alleen worden gewijzigd via code-implementaties.

+ U kunt met het linkernavigatievenster navigeren door de JCR-structuur en deze manipuleren
+ Wanneer u een knooppunt in het navigatievenster aan de linkerkant selecteert, wordt de eigenschap node in het onderste deelvenster beschikbaar gemaakt.
   + Eigenschappen kunnen vanuit het deelvenster worden toegevoegd, verwijderd of gewijzigd
+ Als u dubbelklikt op een bestandsknooppunt in de linkernavigatie, wordt de inhoud van het bestand geopend in het rechterbovenvenster
+ Tik op de knop Alles opslaan linksboven om de wijzigingen te handhaven of op de pijl omlaag naast Alles opslaan om alle niet-opgeslagen wijzigingen te herstellen.

![CRXDE Lite - Fouten opsporen in inhoud](./assets/crxde-lite/debugging-content.png)

Het aanbrengen van wijzigingen in veranderbare inhoud tijdens runtime in AEM as a Cloud Service ontwikkelomgeving via CRXDE Lite moet met de nodige voorzichtigheid gebeuren.
Wijzigingen die rechtstreeks via CRXDE Lite aan AEM worden aangebracht, kunnen moeilijk te volgen en te besturen zijn. Zorgt ervoor dat wijzigingen die via CRXDE Lite zijn aangebracht, terugkeren naar de veranderbare inhoudspakketten van het AEM-project (`ui.content`) en geëngageerd aan Git om ervoor te zorgen dat het probleem wordt opgelost. In het ideale geval komen alle wijzigingen in de toepassingsinhoud van de codebasis en gaan deze via implementaties in AEM, in plaats van rechtstreeks wijzigingen aan te brengen in de AEM via CRXDE Lite.

### Toegangsbesturingselementen voor foutopsporing

CRXDE Lite biedt een manier om toegangsbeheer voor een specifiek knooppunt voor een specifieke gebruiker of groep (ook wel principal genoemd) te testen en te evalueren.

Om tot de console van het Toegangsbeheer van de Test in CRXDE Lite toegang te hebben, navigeer aan:

+ CRXDE Lite > Gereedschappen > Toegangsbeheer testen...

![CRXDE Lite - Toegangscontrole testen](./assets/crxde-lite/permissions__test-access-control.png)

1. Selecteer in het veld Pad een JCR-pad dat u wilt evalueren
1. Selecteer in het veld Hoofd de gebruiker of de groep aan de hand waarvan u het pad wilt beoordelen
1. Tik op de knop Testen

De resultaten worden hieronder weergegeven:

+ __Pad__ herhaalt de ingeslagen weg
+ __Opdrachtgever__ herhaalt de gebruiker of groep waarvoor het pad is geëvalueerd
+ __Principes__ maakt een lijst van alle principes het geselecteerde hoofd deel van is.
   + Dit is nuttig om het overgangsgroepslidmaatschap te begrijpen dat toestemmingen via overerving kan verlenen
+ __Rechten op pad__ Hiermee worden alle JCR-machtigingen weergegeven die de geselecteerde principal op het geëvalueerde pad heeft

### Niet-ondersteunde foutopsporingsactiviteiten

Het volgende is het zuiveren activiteiten die kunnen __niet__ worden uitgevoerd in CRXDE Lite.

### Fouten opsporen in OSGi-configuraties

De opgestelde configuraties OSGi kunnen niet via CRXDE Lite worden herzien. De configuraties OSGi worden gehandhaafd in de AEM van het Project `ui.apps` codepakket op `/apps/example/config.xxx`Bij de implementatie in AEM as a Cloud Service omgevingen blijven de OSGi-configuratiebronnen echter niet behouden voor het JCR, zodat deze dus niet zichtbaar zijn via CRXDE Lite.

Gebruik in plaats daarvan de opdracht [Developer Console > Configurations](./developer-console.md#configurations) aan overzicht opgestelde configuraties OSGi.
