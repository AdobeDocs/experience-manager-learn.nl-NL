---
title: CRXDE Lite
description: 'CRXDE Lite is een klassiek, maar krachtig hulpmiddel om AEM als milieu''s van de Ontwikkelaar van de Cloud Service te zuiveren. CRXDE Lite verstrekt een reeks van functionaliteit die het zuiveren van het inspecteren van alle middelen en eigenschappen, het manipuleren van de veranderlijke gedeelten van JCR en het onderzoeken van toestemmingen helpt. '
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
translation-type: tm+mt
source-git-commit: 10d0970c1b0debfec7cf94cc106bcae414fb55f6
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# Foutopsporing AEM als Cloud Service met CRXDE Lite

CRXDE Lite is __SLECHTS__ beschikbaar op AEM als milieu&#39;s van de Ontwikkeling van de Cloud Service (evenals lokale AEM SDK).

## CRXDE Lite benaderen op AEM-auteur

CRXDE Lite is __slechts__ toegankelijk op AEM als milieu&#39;s van de Ontwikkeling van de Cloud Service, en is __niet__ beschikbaar op het Stadium of de milieu&#39;s van de Productie.

CRXDE Lite openen op AEM-auteur:

1. Meld u aan bij de AEM als AEM-auteurservice van een Cloud Service.
1. Ga naar Gereedschappen > Algemeen > CRXDE Lite

Hiermee wordt CRXDE Lite geopend met de referenties en machtigingen waarmee u zich aanmeldt bij de AEM-auteur.

## Fouten opsporen in inhoud

CRXDE Lite biedt directe toegang tot het JCR. De inhoud die zichtbaar is via CRXDE Lite, wordt beperkt door de machtigingen die aan uw gebruiker zijn verleend. Dit houdt in dat u mogelijk niet alles in het JCR kunt zien of wijzigen, afhankelijk van uw toegang.

Merk op dat `/apps`, `/libs` en `/oak:index` zijn onveranderlijk, betekenend zij niet bij runtime door enige gebruiker kunnen worden veranderd. Deze locaties in het JCR kunnen alleen worden gewijzigd via code-implementaties.

+ U kunt met het linkernavigatievenster navigeren door de JCR-structuur en deze manipuleren
+ Wanneer u een knooppunt in het navigatievenster aan de linkerkant selecteert, wordt de eigenschap node in het onderste deelvenster beschikbaar gemaakt.
   + Eigenschappen kunnen vanuit het deelvenster worden toegevoegd, verwijderd of gewijzigd
+ Als u dubbelklikt op een bestandsknooppunt in de linkernavigatie, wordt de inhoud van het bestand geopend in het rechterbovenvenster
+ Tik op de knop Alles opslaan linksboven om de wijzigingen te handhaven of op de pijl omlaag naast Alles opslaan om alle niet-opgeslagen wijzigingen te herstellen.

![CRXDE Lite - Fouten opsporen in inhoud](./assets/crxde-lite/debugging-content.png)

Het aanbrengen van veranderingen in veranderbare inhoud bij runtime in AEM als milieu van de Cloud Service via CRXDE Lite moet met zorg worden gedaan.
Wijzigingen die rechtstreeks via CRXDE Lite aan AEM worden aangebracht, kunnen moeilijk te volgen en te besturen zijn. Zorgt ervoor dat wijzigingen die via CRXDE Lite zijn aangebracht, terugkeren naar de veranderbare inhoudspakketten (`ui.content`) van het AEM-project en zich ertoe verbinden aan Git, zodat het probleem wordt opgelost. In het ideale geval komen alle wijzigingen in de toepassingsinhoud van de codebasis en gaan deze via implementaties in AEM, in plaats van rechtstreeks wijzigingen aan te brengen in de AEM via CRXDE Lite.

### Toegangsbesturingselementen voor foutopsporing

CRXDE Lite biedt een manier om toegangsbeheer voor een specifiek knooppunt voor een specifieke gebruiker of groep (ook wel principal genoemd) te testen en te evalueren.

Om tot de console van het Toegangsbeheer van de Test in CRXDE Lite toegang te hebben, navigeer aan:

+ CRXDE Lite > Gereedschappen > Toegangsbeheer testen...

![CRXDE Lite - Toegangscontrole testen](./assets/crxde-lite/permissions__test-access-control.png)

1. Selecteer in het veld Pad een JCR-pad dat u wilt evalueren
1. Selecteer in het veld Hoofd de gebruiker of de groep aan de hand waarvan u het pad wilt beoordelen
1. Tik op de knop Testen

De resultaten worden hieronder weergegeven:

+ __Het weg__ herhaalt de weg die werd geëvalueerd
+ __De principal__ herhaalt de gebruiker of de groep waarvoor de weg werd geëvalueerd
+ __Principals__ maakt een lijst van alle principes het geselecteerde hoofd deel van uitmaakt.
   + Dit is nuttig om de transitieve groepslidmaatschappen te begrijpen die toestemmingen via overerving kunnen verstrekken
+ __De voorrechten bij Weg__ maakt een lijst van alle JCR toestemmingen het geselecteerde hoofd op de geëvalueerde weg heeft

### Niet-ondersteunde foutopsporingsactiviteiten

Het volgende is het zuiveren activiteiten die __niet__ in CRXDE Lite kunnen worden uitgevoerd.

### Fouten opsporen in OSGi-configuraties

De opgestelde configuraties OSGi kunnen niet via CRXDE Lite worden herzien. De configuraties OSGi worden gehandhaafd in het `ui.apps` codepakket van het AEM Project bij `/apps/example/config.xxx`, echter op plaatsing aan AEM als Cloud Service milieu&#39;s, worden de OSGi configuratiemiddelen niet voortgeduurd aan JCR, daarom niet zichtbaar via CRXDE Lite.

Gebruik in plaats daarvan de [Developer Console > Configurations](./developer-console.md#configurations) om geïmplementeerde OSGi-configuraties te controleren.
