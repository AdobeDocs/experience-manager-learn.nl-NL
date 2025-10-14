---
title: Hoofdstuk 4 - Templates voor inhoudsservices definiëren - Inhoudsservices
description: Hoofdstuk 4 van de AEM zelfstudie zonder titel behandelt de rol van AEM bewerkbare sjablonen in de context van AEM Content Services. Bewerkbare sjablonen worden gebruikt om de JSON-inhoudsstructuur te definiëren AEM Content Services uiteindelijk beschikbaar maken.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 245
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# Hoofdstuk 4 - Templates voor inhoudsservices definiëren

Hoofdstuk 4 van de AEM zelfstudie zonder titel behandelt de rol van AEM bewerkbare sjablonen in de context van AEM Content Services. Bewerkbare sjablonen worden gebruikt om de JSON-inhoudsstructuur te definiëren AEM Content Services wordt aangeboden aan klanten via de compositie van Content Services die is ingeschakeld AEM Components.

## De rol van sjablonen in AEM Content Services begrijpen

AEM Bewerkbare sjablonen worden gebruikt om de HTTP-eindpunten te definiëren die worden benaderd om de Event-inhoud beschikbaar te maken als JSON.

Traditioneel worden AEM bewerkbare sjablonen gebruikt om webpagina&#39;s te definiëren, maar dit gebruik is gewoon een conventie. Bewerkbare Malplaatjes kunnen worden gebruikt om **om het even welke** reeks inhoud samen te stellen; hoe die inhoud wordt betreden: als HTML in browser, zoals JSON die door JavaScript (AEM Redacteur SPA) wordt verbruikt of een Mobiele App is een functie van hoe die pagina wordt gevraagd.

In AEM Content Services worden bewerkbare sjablonen gebruikt om te definiëren hoe de JSON-gegevens worden weergegeven.

Voor de [!DNL WKND Mobile] -toepassing maken we één bewerkbare sjabloon die wordt gebruikt om één API-eindpunt te maken. Hoewel dit voorbeeld eenvoudig is om de concepten AEM Headless te illustreren, kunt u meerdere Pagina&#39;s (of Eindpunten) maken die elk verschillende sets inhoud blootstellen om een complexere, en beter georganiseerde API te maken.

## Het API-eindpunt

Als u wilt weten hoe u ons API-eindpunt kunt samenstellen en begrijpen welke inhoud beschikbaar moet worden gesteld voor onze [!DNL WKND Mobile] -app, kunt u het ontwerp opnieuw bekijken.

![&#x200B; Gebeurtenissen API de Decompositie van de Pagina &#x200B;](./assets/chapter-4/design-to-component-mapping.png)

Zoals we kunnen zien, hebben we drie logische sets met inhoud die aan de mobiele app moeten worden geleverd.

1. Het **logo**
2. De **Lijn van de Markering**
3. De lijst van **Gebeurtenissen**

Om dit te doen, kunnen wij deze vereisten aan AEM Componenten (en in ons geval, AEM de Componenten van de Kern WCM) in kaart brengen om de vereiste inhoud als JSON bloot te stellen.

1. Het **Logo** wordt bedekt via een **component van het Beeld**
2. De **Lijn van de Markering** wordt bedekt via de component van de a **Tekst**
3. De lijst van **Gebeurtenissen** wordt bedekt via de component van de Lijst van het Fragment van de a **Inhoud** die beurtelings, verwijzingen een reeks Fragmenten van de Inhoud van de Gebeurtenis.

>[!NOTE]
>
>Om AEM de uitvoer van JSON van de Dienst van de Inhoud van Pagina&#39;s en Componenten te steunen, moeten de Pagina&#39;s en de Componenten **uit AEM WCM de Componenten van de Kern voortkomen**.
>
>[&#x200B; AEM de Componenten van de Kern WCM &#x200B;](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) hebben ingebouwde functionaliteit om een genormaliseerd schema JSON van uitgevoerde Pagina&#39;s en Componenten te steunen. Alle mobiele WKND-componenten die in deze zelfstudie worden gebruikt (pagina, afbeelding, tekst en lijst met inhoudsfragmenten), zijn afgeleid van AEM WCM Core-componenten.

## De API-sjabloon voor gebeurtenissen definiëren

1. Navigeer naar **[!UICONTROL Tools]> [!UICONTROL General] > [!UICONTROL Templates] >[!DNL WKND Mobile]** .

1. Maak de sjabloon **[!DNL Events API]** :

   1. Tik **[!UICONTROL Create]** op de bovenste actiebalk
   1. Selecteer de sjabloon **[!DNL WKND Mobile - Empty Page]**
   1. Tik **[!UICONTROL Next]** op de bovenste actiebalk
   1. Voer **[!DNL Events API]** in het veld [!UICONTROL Template Title] in
   1. Tik **[!UICONTROL Create]** op de bovenste actiebalk
   1. Tik op de nieuwe sjabloon **[!UICONTROL Open]** om deze te bewerken

1. Ten eerste staan we de drie geïdentificeerde AEM componenten toe die we nodig hebben om de inhoud te modelleren door de [!UICONTROL Policy] van de hoofdmap [!UICONTROL Layout Container] te bewerken. Zorg ervoor dat de modus **[!UICONTROL Structure]** actief is, selecteer **[!DNL Layout Container \[Root\]]** en tik op de knop **[!UICONTROL Policy]** .
1. Onder **[!UICONTROL Properties]>[!UICONTROL Allowed Components]** Zoeken naar **[!DNL WKND Mobile]** . De volgende componenten uit de componentgroep [!DNL WKND Mobile] toestaan, zodat ze op de API-pagina van [!DNL Events] kunnen worden gebruikt.

   * **[!DNL WKND Mobile > Image]**

      * Het logo voor de app

   * **[!DNL WKND Mobile > Text]**

      * De inleidende tekst van de app

   * **[!DNL WKND Mobile > Content Fragment List]**

      * De lijst met gebeurteniscategorieën die beschikbaar zijn voor weergave in de app

1. Tik na voltooiing op het vinkje van **[!UICONTROL Done]** in de rechterbovenhoek.
1. **verfrist zich** het browser venster om nieuwe [!UICONTROL Allowed Components] lijst in het linkerspoor te zien.
1. Sleep vanuit de Finder Componenten in de linkerspoorstaaf in de volgende AEM Componenten:
   1. **[!DNL Image]** voor het logo
   2. **[!DNL Text]** voor de taglijn
   3. **[!DNL Content Fragment List]** voor de gebeurtenissen
1. **voor elk van de bovengenoemde componenten**, selecteer hen en druk **ontgrendelen** knoop.
1. Nochtans, verzeker de **lay-outcontainer** **wordt gesloten** om andere componenten te verhinderen worden toegevoegd, of deze drie componenten worden verwijderd.
1. Tik op **[!UICONTROL Page Information]>[!UICONTROL View in Admin]** om terug te keren naar de lijst met [!DNL WKND Mobile] sjablonen. Selecteer de nieuw gemaakte **[!DNL Events API]** sjabloon en tik **[!UICONTROL Enable]** op de bovenste actiebalk.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> De componenten die worden gebruikt om de inhoud te bedekken, worden aan de sjabloon zelf toegevoegd en vergrendeld. Op deze manier kunnen auteurs de vooraf gedefinieerde componenten bewerken, maar niet willekeurig componenten toevoegen of verwijderen, omdat het wijzigen van de API zelf de veronderstellingen rond de JSON-structuur kan onderbreken en de gebruikte apps kan onderbreken. Alle API&#39;s moeten stabiel zijn.

## Volgende stappen

Naar keuze, installeer [&#x200B; com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip &#x200B;](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket op AEM Auteur via [&#x200B; AEM de Manager van het Pakket &#x200B;](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 5 - Pagina&#39;s met inhoudsservices ontwerpen](./chapter-5.md)
