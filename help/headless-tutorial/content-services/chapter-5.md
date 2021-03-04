---
title: Hoofdstuk 5 - Pagina's van de Diensten van de Inhoud van het Ontwerp - de Diensten van de Inhoud
description: Hoofdstuk 5 van de AEM zelfstudie zonder koptekst behandelt het maken van de pagina's van de sjablonen die zijn gedefinieerd in hoofdstuk 4. Deze pagina's fungeren als eindpunten voor JSON HTTP.
feature: '"Inhoudsfragmenten, API''s"'
topic: '"Headless, Content Management"'
role: Developer
level: Begin
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 0%

---


# Hoofdstuk 5 - Pagina&#39;s met inhoudsservices ontwerpen

Hoofdstuk 5 van de AEM zelfstudie zonder koptekst behandelt het maken van de pagina van de sjablonen die zijn gedefinieerd in hoofdstuk 4. De pagina die in dit hoofdstuk wordt gemaakt, fungeert als het eindpunt van JSON HTTP voor de Mobile-app.

>[!NOTE]
>
> De architectuur van de pagina-inhoud van `/content/wknd-mobile/en/api` is vooraf samengesteld. De basispagina&#39;s van `en` en `api` dienen een architecturaal en organisatorisch doel, maar zijn niet strikt vereist. Als API-inhoud kan worden gelokaliseerd, kunt u het beste de gebruikelijke best practices voor taalkopieën en beheer van meerdere pagina&#39;s volgen, aangezien API-pagina&#39;s net als elke AEM Sites-pagina kunnen worden gelokaliseerd.

## De API-pagina voor gebeurtenissen maken

1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tik op het label van de API-pagina**, tik vervolgens op de knop  **** Maken in de bovenste actiebalk en maak een nieuwe API-pagina voor gebeurtenissen onder de API-pagina.
   1. Tik **Maken** in de bovenste actiebalk
   1. Selecteer **Gebeurtenis-API**-sjabloon
   1. Typ **events** in het veld **Naam**
   1. Voer in het veld **Titel** **Gebeurtenissen-API** in
   1. Tik **Maken** in de bovenste actiebalk om de pagina te maken
   1. Tik **Done** om terug te keren naar de AEM Sites-beheerder

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## De API-pagina voor gebeurtenissen ontwerpen

>[!NOTE]
>
> Het project bevat CSS om enkele basisstijlen voor de auteur te bieden.

1. Bewerk de pagina **Events API** door naar **AEM > Sites > WKND Mobile > English > API** te navigeren, de pagina **Events API** te selecteren en op **Edit** te tikken op de bovenste actiebalk.
1. Voeg een **logoafbeelding** toe om in de app weer te geven door deze vanuit de Finder van middelen naar de tijdelijke aanduiding voor de afbeeldingscomponent te slepen.
   * Gebruik het meegeleverde logo dat u kunt vinden op `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Voeg **taglijn** toe om boven de gebeurtenissen weer te geven.
   1. De component **Text** bewerken
   1. Voer de tekst in:
      * `The WKND is here.`

1. Selecteer de **gebeurtenissen** om weer te geven:
   1. Stel de volgende configuratie in op het tabblad **Eigenschappen**:
      * Model: **Gebeurtenis**
      * Bovenliggend pad: **/content/dam/wknd-mobile/nl/events**
      * Tags: **&lt;Leeg laten>**
   1. Stel de volgende configuratie in op het tabblad **Elements**:
      * Verwijder alle vermelde elementen om ervoor te zorgen dat ALLE elementen van de fragmenten voor gebeurtenisinhoud zichtbaar zijn.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## De JSON-uitvoer van de API-pagina controleren

De JSON-uitvoer en de indeling ervan kunnen worden gecontroleerd door de pagina aan te vragen met de kiezer `.model.json`.

Deze JSON-structuur (of -schema) moet goed worden begrepen door consumenten van deze API. Het is van essentieel belang dat gebruikers weten welke aspecten van de structuur vast zijn (dat wil zeggen: het logo (afbeelding) en de live tag (tekst) van de API voor gebeurtenissen en zijn vloeiend (dat wil zeggen: de gebeurtenissen vermeld onder de component Lijst van het Fragment van de Inhoud).

Als u dit contract niet toepast op een gepubliceerde API, kan dit leiden tot een onjuist gedrag in de betreffende apps.

1. In nieuwe browser lusjes, verzoek de Pagina&#39;s van de Gebeurtenissen API gebruikend de `.model.json` selecteur, die AEM de Exporteur van JSON van de Diensten van de Inhoud aanhaalt, en de Pagina en Componenten in een genormaliseerde, duidelijk bepaalde structuur van JSON serialiseert.

   De JSON-structuur die door deze pagina&#39;s wordt gemaakt, is de structuur waarop apps moeten worden uitgelijnd.

1. Vraag de **API voor gebeurtenissen**-pagina aan als **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Het resultaat moet er ongeveer als volgt uitzien:

![AEM Content Services JSON-uitvoer](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Deze JSON kan worden uitgevoerd op een **tidy** (geformatteerd) wijze voor menselijke-leesbaarheid door `.tidy` selecteur te gebruiken:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Volgende stap

Installeer desgewenst het inhoudspakket [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) op AEM Author via [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 6 - De inhoud in AEM-publicaties beschikbaar maken als JSON](./chapter-6.md)
