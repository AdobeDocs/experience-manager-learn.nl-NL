---
title: Hoofdstuk 4 - Templates voor inhoudsservices definiëren - Inhoudsservices
description: Hoofdstuk 4 van de AEM zelfstudie zonder titel behandelt de rol van AEM bewerkbare sjablonen in de context van AEM Content Services. Bewerkbare sjablonen worden gebruikt om de JSON-inhoudsstructuur te definiëren AEM Content Services uiteindelijk beschikbaar maken.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 301
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# Hoofdstuk 4 - Templates voor inhoudsservices definiëren

Hoofdstuk 4 van de AEM zelfstudie zonder titel behandelt de rol van AEM bewerkbare sjablonen in de context van AEM Content Services. Bewerkbare sjablonen worden gebruikt om de JSON-inhoudsstructuur te definiëren AEM Content Services wordt aangeboden aan klanten via de compositie van Content Services die is ingeschakeld AEM Components.

## De rol van sjablonen in AEM Content Services begrijpen

AEM Bewerkbare sjablonen worden gebruikt om de HTTP-eindpunten te definiëren die worden benaderd om de Event-inhoud beschikbaar te maken als JSON.

Traditioneel worden AEM bewerkbare sjablonen gebruikt om webpagina&#39;s te definiëren, maar dit gebruik is gewoon een conventie. Bewerkbare sjablonen kunnen worden gebruikt om samen te stellen **alle** set met inhoud; hoe die inhoud wordt benaderd: als HTML in een browser, zoals JSON gebruikt door JavaScript (AEM SPA Editor) of een Mobile-toepassing, is een functie van de manier waarop die pagina wordt opgevraagd.

In AEM Content Services worden bewerkbare sjablonen gebruikt om te definiëren hoe de JSON-gegevens worden weergegeven.

Voor de [!DNL WKND Mobile] App, zullen wij één enkel Bewerkbaar Malplaatje creëren dat wordt gebruikt om één enkel API eindpunt te drijven. Hoewel dit voorbeeld eenvoudig is om de concepten AEM Headless te illustreren, kunt u meerdere Pagina&#39;s (of Eindpunten) maken die elk verschillende sets inhoud blootstellen om een complexere, en beter georganiseerde API te maken.

## Het API-eindpunt

Om te begrijpen hoe te om ons API eindpunt samen te stellen, en te begrijpen welke inhoud aan ons zou moeten worden blootgesteld [!DNL WKND Mobile] App, laten we het ontwerp opnieuw bekijken.

![API voor gebeurtenissen Decomposition](./assets/chapter-4/design-to-component-mapping.png)

Zoals we kunnen zien, hebben we drie logische sets met inhoud die aan de mobiele app moeten worden geleverd.

1. De **Logo**
2. De **Lijn met tag**
3. De lijst van **Gebeurtenissen**

Om dit te doen, kunnen wij deze vereisten aan AEM Componenten (en in ons geval, AEM de Componenten van de Kern WCM) in kaart brengen om de vereiste inhoud als JSON bloot te stellen.

1. De **Logo** is via een **Afbeeldingscomponent**
2. De **Lijn met tag** is via een **Tekstcomponent**
3. De lijst van **Gebeurtenissen** is via een **component Lijst met inhoudsfragmenten** dat op zijn beurt verwijst naar een set gebeurtenisinhoudfragmenten.

>[!NOTE]
>
>Om de JSON-export van pagina&#39;s en componenten van AEM Content Service te ondersteunen, moeten de pagina&#39;s en componenten **afgeleid van AEM WCM Core Components**.
>
>[AEM WCM Core-componenten](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) beschikken over ingebouwde functionaliteit ter ondersteuning van een genormaliseerd JSON-schema van geëxporteerde pagina&#39;s en componenten. Alle mobiele WKND-componenten die in deze zelfstudie worden gebruikt (pagina, afbeelding, tekst en lijst met inhoudsfragmenten), zijn afgeleid van AEM WCM Core-componenten.

## De API-sjabloon voor gebeurtenissen definiëren

1. Navigeren naar **[!UICONTROL Tools]> [!UICONTROL General] > [!UICONTROL Templates] >[!DNL WKND Mobile]**.

1. Maak de **[!DNL Events API]** sjabloon:

   1. Tikken **[!UICONTROL Create]** in de bovenste actiebalk
   1. Selecteer de **[!DNL WKND Mobile - Empty Page]** template
   1. Tikken **[!UICONTROL Next]** in de bovenste actiebalk
   1. Enter **[!DNL Events API]** in de [!UICONTROL Template Title] field
   1. Tikken **[!UICONTROL Create]** in de bovenste actiebalk
   1. Tikken **[!UICONTROL Open]** de nieuwe sjabloon openen voor bewerken

1. Ten eerste staan we de drie geïdentificeerde AEM componenten toe die we nodig hebben om de inhoud te modelleren door de [!UICONTROL Policy] van de hoofdmap [!UICONTROL Layout Container]. Zorg ervoor dat **[!UICONTROL Structure]** is actief, selecteert u de **[!DNL Layout Container \[Root\]]** en tik op de knop **[!UICONTROL Policy]** knop.
1. Onder **[!UICONTROL Properties]>[!UICONTROL Allowed Components]** zoeken naar **[!DNL WKND Mobile]**. De volgende componenten toestaan vanuit de [!DNL WKND Mobile] componentgroep zodat deze op de [!DNL Events] API-pagina.

   * **[!DNL WKND Mobile > Image]**

      * Het logo voor de app

   * **[!DNL WKND Mobile > Text]**

      * De inleidende tekst van de app

   * **[!DNL WKND Mobile > Content Fragment List]**

      * De lijst met gebeurteniscategorieën die beschikbaar zijn voor weergave in de app

1. Tik op de knop **[!UICONTROL Done]** vinkje in de rechterbovenhoek wanneer dit is voltooid.
1. **Vernieuwen** het browservenster om het nieuwe venster te zien [!UICONTROL Allowed Components] lijst in de linkerrail.
1. Sleep vanuit de Finder Componenten in de linkerspoorstaaf in de volgende AEM Componenten:
   1. **[!DNL Image]** voor het logo
   2. **[!DNL Text]** voor de taglijn
   3. **[!DNL Content Fragment List]** voor de gebeurtenissen
1. **Voor elk van de bovenstaande onderdelen**, selecteert u deze en drukt u op **ontgrendelen** knop.
1. De **lay-outcontainer** is **vergrendeld** om te voorkomen dat andere componenten worden toegevoegd of dat deze drie componenten worden verwijderd.
1. Tikken **[!UICONTROL Page Information]>[!UICONTROL View in Admin]** om terug te keren naar de [!DNL WKND Mobile] aanbieding van sjablonen. Selecteer de zojuist gemaakte **[!DNL Events API]** sjabloon en tikken **[!UICONTROL Enable]** in de bovenste actiebalk.

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> De componenten die worden gebruikt om de inhoud te bedekken, worden aan de sjabloon zelf toegevoegd en vergrendeld. Op deze manier kunnen auteurs de vooraf gedefinieerde componenten bewerken, maar niet willekeurig componenten toevoegen of verwijderen, omdat het wijzigen van de API zelf de veronderstellingen rond de JSON-structuur kan onderbreken en de gebruikte apps kan onderbreken. Alle API&#39;s moeten stabiel zijn.

## Volgende stappen

Installeer desgewenst de [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) inhoudspakket op AEM auteur via [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 5 - Pagina&#39;s met inhoudsservices ontwerpen](./chapter-5.md)
