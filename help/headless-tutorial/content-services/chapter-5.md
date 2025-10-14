---
title: Hoofdstuk 5 - Pagina's van de Diensten van de Inhoud van het Ontwerp - de Diensten van de Inhoud
description: Hoofdstuk 5 van de AEM zelfstudie zonder koptekst behandelt het maken van de pagina's van de sjablonen die zijn gedefinieerd in hoofdstuk 4. Deze pagina's fungeren als eindpunten voor JSON HTTP.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Hoofdstuk 5 - Pagina&#39;s met inhoudsservices ontwerpen

Hoofdstuk 5 van de AEM zelfstudie zonder koptekst behandelt het maken van de pagina van de sjablonen die zijn gedefinieerd in hoofdstuk 4. De pagina die in dit hoofdstuk wordt gemaakt, fungeert als het eindpunt van JSON HTTP voor de Mobile-app.

>[!NOTE]
>
> De architectuur van de pagina-inhoud van `/content/wknd-mobile/en/api` is vooraf samengesteld. De basispagina&#39;s van `en` en `api` hebben een architecturaal en organisatorisch doel, maar zijn niet strikt vereist. Als API-inhoud kan worden gelokaliseerd, kunt u het beste de gebruikelijke best practices voor taalkopieën en beheer van meerdere pagina&#39;s volgen, aangezien API-pagina&#39;s net als elke AEM Sites-pagina kunnen worden gelokaliseerd.

## De API-pagina voor gebeurtenissen maken

1. Navigeer naar **[!UICONTROL AEM]> [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]** .
1. **Tik het etiket van de API pagina**, dan de **creeer** knoop in de hoogste actiebar en creeer een nieuwe Gebeurtenissen API pagina onder de API pagina.
   1. Tik **creeer** in de hoogste actiebar
   1. Selecteer **Gebeurtenissen API** malplaatje
   1. Op het **gebied van de Naam** **gaat gebeurtenissen** in
   1. Op het **gebied van de Titel** ga **Gebeurtenissen API** in
   1. Tik **creeer** in de hoogste actiebar om de pagina te creëren
   1. Tik **Gedaan** om aan AEM Sites admin terug te keren

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## De API-pagina voor gebeurtenissen ontwerpen

>[!NOTE]
>
> Het project bevat CSS om enkele basisstijlen voor de auteur te bieden.

1. Bewerk de **Gebeurtenissen API** pagina door aan **AEM > Plaatsen > Mobiele WKND > Engels > API** te navigeren, de **Gebeurtenissen API** pagina te selecteren, en het tikken **geeft** in de hoogste actiebar uit.
1. Voeg het beeld van het a **embleem** aan vertoning in app toe door het van de Vinder van Activa op placeholder van de component van het Beeld te slepen en te laten vallen.
   * Gebruik het meegeleverde logo dat u kunt vinden op `/content/dam/wknd-mobile/images/wknd-logo.png` .

1. Voeg **markeringslijn** toe om boven de gebeurtenissen te tonen.
   1. Bewerk de **component van de Tekst**
   1. Voer de tekst in:
      * `The WKND is here.`

1. Selecteer de **gebeurtenissen** aan vertoning:
   1. Plaats de volgende configuratie op het **Eigenschappen** lusje:
      * Model: **Gebeurtenis**
      * Bovenliggend pad: **/content/dam/wknd-mobile/nl/events**
      * Tags: **&lt;Leave blank>**
   1. Plaats de volgende configuratie op het **Elementen** lusje:
      * Verwijder alle vermelde elementen om ervoor te zorgen dat ALLE elementen van de fragmenten voor gebeurtenisinhoud zichtbaar zijn.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## De JSON-uitvoer van de API-pagina controleren

De JSON-uitvoer en de indeling ervan kunnen worden gecontroleerd door de pagina aan te vragen bij de kiezer van `.model.json` .

Deze JSON-structuur (of -schema) moet goed worden begrepen door consumenten van deze API. Het is van essentieel belang dat gebruikers weten welke aspecten van de structuur vast zijn (dat wil zeggen: het logo (afbeelding) en de live tag (tekst) van de API voor gebeurtenissen en zijn vloeiend (dat wil zeggen: de gebeurtenissen vermeld onder de component Lijst van het Fragment van de Inhoud).

Als u dit contract niet toepast op een gepubliceerde API, kan dit leiden tot een onjuist gedrag in de betreffende apps.

1. In nieuwe browsertabbladen vraagt u de API-pagina&#39;s voor gebeurtenissen aan met de `.model.json` -kiezer, die de JSON Exporter van Content Services aanroept AEM en de pagina en componenten serialiseert in een genormaliseerde, goed gedefinieerde JSON-structuur.

   De JSON-structuur die door deze pagina&#39;s wordt gemaakt, is de structuur waarop apps moeten worden uitgelijnd.

1. Vraag de **gebeurtenissen API** pagina als **JSON** aan.

   * [&#x200B; http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Het resultaat moet er ongeveer als volgt uitzien:

![&#x200B; AEM de Uitvoer van de Diensten JSON van de Inhoud &#x200B;](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Dit JSON kan output in a **tidy** (geformatteerde) wijze voor mens-leesbaarheid door de `.tidy` selecteur te gebruiken zijn:
> * [&#x200B; http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## Volgende stap

Naar keuze, installeer [&#x200B; com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip &#x200B;](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket op AEM Auteur via [&#x200B; AEM de Manager van het Pakket &#x200B;](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 6 - De inhoud op AEM Publish beschikbaar maken als JSON](./chapter-6.md)
