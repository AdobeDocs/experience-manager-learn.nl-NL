---
title: CRXDE Lite
description: CRXDE Lite is een klassiek, maar toch krachtig hulpmiddel voor het opsporen van fouten in AEM as a Cloud Service Developer-omgevingen. CRXDE Lite biedt een reeks functies die foutopsporing mogelijk maakt door alle bronnen en eigenschappen te inspecteren, de veranderbare delen van het JCR te bewerken en de machtigingen te onderzoeken.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 125
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Fouten in AEM as a Cloud Service opsporen met CRXDE Lite

CRXDE Lite is __SLECHTS__ beschikbaar op de milieu&#39;s van de Ontwikkeling van AEM as a Cloud Service (evenals lokale AEM SDK).

## CRXDE Lite openen op AEM-auteur

CRXDE Lite is __slechts__ toegankelijk op de milieu&#39;s van de Ontwikkeling van AEM as a Cloud Service, en is __niet__ beschikbaar op de milieu&#39;s van het Stadium of van de Productie.

CRXDE Lite openen op AEM-auteur:

1. Meld u aan bij de AEM as a Cloud Service AEM Author-service.
1. Ga naar Gereedschappen > Algemeen > CRXDE Lite

Hiermee wordt CRXDE Lite geopend met de referenties en machtigingen waarmee u zich aanmeldt bij de AEM Author.

## Fouten opsporen in inhoud

CRXDE Lite biedt directe toegang tot het JCR. De inhoud die via CRXDE Lite zichtbaar is, wordt beperkt door de machtigingen die aan uw gebruiker zijn verleend. Dit houdt in dat u mogelijk niet alles in het JCR kunt zien of wijzigen, afhankelijk van uw toegang.

Houd er rekening mee dat `/apps` , `/libs` en `/oak:index` onveranderbaar zijn. Dit betekent dat deze niet tijdens de runtime door een gebruiker kunnen worden gewijzigd. Deze locaties in het JCR kunnen alleen worden gewijzigd via code-implementaties.

+ U kunt met het linkernavigatievenster navigeren door de JCR-structuur en deze manipuleren
+ Wanneer u een knooppunt in het navigatievenster aan de linkerkant selecteert, wordt de eigenschap node in het onderste deelvenster beschikbaar gemaakt.
   + Eigenschappen kunnen vanuit het deelvenster worden toegevoegd, verwijderd of gewijzigd
+ Als u dubbelklikt op een bestandsknooppunt in de linkernavigatie, wordt de inhoud van het bestand geopend in het rechterbovenvenster
+ Tik op de knop Alles opslaan linksboven om de wijzigingen te handhaven of op de pijl omlaag naast Alles opslaan om alle niet-opgeslagen wijzigingen te herstellen.

![ CRXDE Lite - het Zuiveren Inhoud ](./assets/crxde-lite/debugging-content.png)

Het aanbrengen van wijzigingen in veranderbare inhoud tijdens runtime in AEM as a Cloud Service-ontwikkelomgevingen via CRXDE Lite moet met de nodige voorzichtigheid gebeuren.
Wijzigingen die rechtstreeks via CRXDE Lite in AEM worden aangebracht, kunnen moeilijk te volgen en te besturen zijn. Indien van toepassing, zorg ervoor dat de veranderingen die via CRXDE Lite worden aangebracht hun weg naar de veranderbare inhoudspakketten van het project van AEM (`ui.content`) en geëngageerd aan Git maken, om de kwestie te verzekeren wordt opgelost. In het ideale geval komen alle wijzigingen in de toepassingsinhoud van de basis van de code en gaan deze via implementaties naar AEM, in plaats van direct wijzigingen aan te brengen in de AEM via CRXDE Lite.

### Toegangsbesturingselementen voor foutopsporing

CRXDE Lite biedt een manier om toegangsbeheer voor een specifiek knooppunt voor een specifieke gebruiker of groep (ook wel principal genoemd) te testen en te evalueren.

Ga naar:

+ CRXDE Lite > Extra > Toegangsbeheer testen...

![ CRXDE Lite - de Controle van de Toegang van de Test ](./assets/crxde-lite/permissions__test-access-control.png)

1. Selecteer in het veld Pad een JCR-pad dat u wilt evalueren
1. Selecteer in het veld Hoofd de gebruiker of de groep aan de hand waarvan u het pad wilt beoordelen
1. Tik op de knop Testen

De resultaten worden hieronder weergegeven:

+ __Weg__ herhaalt de weg die werd geëvalueerd
+ __Belangrijk__ herhaalt de gebruiker of de groep waarvoor de weg werd geëvalueerd
+ __Belangrijkste__ maakt een lijst van alle hoofden het geselecteerde hoofd deel van is.
   + Dit is nuttig om het overgangsgroepslidmaatschap te begrijpen dat toestemmingen via overerving kan verlenen
+ __Bevoegdheden bij Weg__ maakt een lijst van alle JCR toestemmingen het geselecteerde hoofd op de geëvalueerde weg heeft

### Niet-ondersteunde foutopsporingsactiviteiten

Het volgende is het zuiveren activiteiten die __niet__ in CRXDE Lite kunnen worden uitgevoerd.

### Fouten opsporen in OSGi-configuraties

De opgestelde configuraties OSGi kunnen niet via CRXDE Lite worden herzien. OSGi-configuraties worden onderhouden in het codepakket `ui.apps` van het AEM Project op `/apps/example/config.xxx` . Bij implementatie in AEM as a Cloud Service-omgevingen worden de OSGi-configuratiebronnen echter niet gecontinueerd naar het JCR, zodat deze dus niet zichtbaar zijn via CRXDE Lite.

In plaats daarvan, gebruik [ Developer Console > Configuraties ](./developer-console.md#configurations) om opgestelde configuraties te herzien OSGi.
