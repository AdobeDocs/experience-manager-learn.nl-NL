---
title: Hoofdstuk 5 - Pagina's van de Diensten van de Inhoud van het Ontwerp - de Diensten van de Inhoud
description: Hoofdstuk 5 van de AEM zelfstudie zonder koptekst behandelt het maken van de pagina's van de sjablonen die zijn gedefinieerd in hoofdstuk 4. Deze pagina's fungeren als eindpunten voor JSON HTTP.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---

# Hoofdstuk 5 - Pagina&#39;s met inhoudsservices ontwerpen

Hoofdstuk 5 van de AEM zelfstudie zonder koptekst behandelt het maken van de pagina van de sjablonen die zijn gedefinieerd in hoofdstuk 4. De pagina die in dit hoofdstuk wordt gemaakt, fungeert als het eindpunt van JSON HTTP voor de Mobile-app.

>[!NOTE]
>
> De architectuur van de pagina-inhoud van `/content/wknd-mobile/en/api` is vooraf gebouwd. De basispagina&#39;s van `en` en `api` dienen een architectonisch en organisatorisch doel, maar zijn niet strikt vereist. Als API-inhoud kan worden gelokaliseerd, kunt u het beste de gebruikelijke best practices voor taalkopieën en beheer van meerdere pagina&#39;s volgen, aangezien API-pagina&#39;s net als elke AEM Sites-pagina kunnen worden gelokaliseerd.

## De API-pagina voor gebeurtenissen maken

1. Ga naar **[!UICONTROL AEM]> [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Tik op het label op de API-pagina** Tik vervolgens op de knop **Maken** in de bovenste actiebalk en maak een nieuwe API-pagina voor gebeurtenissen onder de API-pagina.
   1. Tikken **Maken** in de bovenste actiebalk
   1. Selecteren **API voor gebeurtenissen** template
   1. In de **Naam** veld enter **gebeurtenissen**
   1. In de **Titel** veld enter **API voor gebeurtenissen**
   1. Tikken **Maken** in de bovenste actiebalk om de pagina te maken
   1. Tikken **Gereed** om terug te keren naar de AEM Sites-beheerder

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## De API-pagina voor gebeurtenissen ontwerpen

>[!NOTE]
>
> Het project bevat CSS om enkele basisstijlen voor de auteur te bieden.

1. Bewerk de **API voor gebeurtenissen** pagina door naar **AEM > Sites > WKND Mobile > English > API**, selecteert u de **API voor gebeurtenissen** pagina, en tikken **Bewerken** in de bovenste actiebalk.
1. Voeg een **logoafbeelding** om in de app weer te geven door deze van de Asset Finder naar de tijdelijke aanduiding voor de afbeeldingscomponent te slepen.
   * Gebruik het opgegeven logo op `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Toevoegen **labellijn** boven de gebeurtenissen weer te geven.
   1. Bewerk de **Tekst** component
   1. Voer de tekst in:
      * `The WKND is here.`

1. Selecteer **gebeurtenissen** weer te geven:
   1. Stel de volgende configuratie in op de knop **Eigenschappen** tab:
      * Model: **Gebeurtenis**
      * Bovenliggend pad: **/content/dam/wknd-mobile/nl/events**
      * Tags: **&lt;leave blank=&quot;&quot;>**
   1. Stel de volgende configuratie in op de knop **Elementen** tab:
      * Verwijder alle vermelde elementen om ervoor te zorgen dat ALLE elementen van de fragmenten voor gebeurtenisinhoud zichtbaar zijn.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## De JSON-uitvoer van de API-pagina controleren

De JSON-uitvoer en de indeling ervan kunnen worden gecontroleerd door de pagina aan te vragen bij de `.model.json` kiezer.

Deze JSON-structuur (of -schema) moet goed worden begrepen door consumenten van deze API. Het is van essentieel belang dat gebruikers weten welke aspecten van de structuur vast zijn (dat wil zeggen: het logo (afbeelding) en de live tag (tekst) van de API voor gebeurtenissen en zijn vloeiend (dat wil zeggen: de gebeurtenissen vermeld onder de component Lijst van het Fragment van de Inhoud).

Als u dit contract niet toepast op een gepubliceerde API, kan dit leiden tot een onjuist gedrag in de betreffende apps.

1. In nieuwe browsertabbladen kunt u de API-pagina&#39;s voor gebeurtenissen aanvragen met de opdracht `.model.json` kiezer, die de JSON Exporter van AEM Content Services aanroept en de pagina en componenten serialiseert in een genormaliseerde, goed gedefinieerde JSON-structuur.

   De JSON-structuur die door deze pagina&#39;s wordt gemaakt, is de structuur waarop apps moeten worden uitgelijnd.

1. Vraag de **API voor gebeurtenissen** pagina als **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Het resultaat moet er ongeveer als volgt uitzien:

![AEM Content Services JSON-uitvoer](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Deze JSON kan worden uitgevoerd in een **gespannen** (geformatteerd) wijze voor menselijke leesbaarheid door te gebruiken `.tidy` kiezer:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Volgende stap

Installeer desgewenst de [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket op AEM-auteur via [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 6 - De inhoud in AEM-publicaties beschikbaar maken als JSON](./chapter-6.md)
