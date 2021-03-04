---
title: Hoofdstuk 4 - Templates voor inhoudsservices definiëren - Inhoudsservices
seo-title: Aan de slag met AEM zonder kop - Hoofdstuk 4 - Templates voor inhoudsservices definiëren
description: Hoofdstuk 4 van de AEM zelfstudie zonder titel behandelt de rol van AEM bewerkbare sjablonen in de context van AEM Content Services. Bewerkbare sjablonen worden gebruikt om de JSON-inhoudsstructuur te definiëren AEM Content Services uiteindelijk zichtbaar wordt.
seo-description: Hoofdstuk 4 van de AEM zelfstudie zonder titel behandelt de rol van AEM bewerkbare sjablonen in de context van AEM Content Services. Bewerkbare sjablonen worden gebruikt om de JSON-inhoudsstructuur te definiëren AEM Content Services uiteindelijk zichtbaar wordt.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---


# Hoofdstuk 4 - Templates voor inhoudsservices definiëren

Hoofdstuk 4 van de AEM zelfstudie zonder titel behandelt de rol van AEM bewerkbare sjablonen in de context van AEM Content Services. Bewerkbare sjablonen worden gebruikt om de JSON-inhoudsstructuur te definiëren AEM Content Services wordt aangeboden aan klanten via de compositie van Content Services die is ingeschakeld AEM Components.

## De rol van sjablonen in AEM Content Services begrijpen

AEM Bewerkbare sjablonen worden gebruikt om de HTTP-eindpunten te definiëren die worden benaderd om de Event-inhoud beschikbaar te maken als JSON.

Traditioneel AEM Bewerkbare Malplaatjes worden gebruikt om Web-pagina&#39;s te bepalen, nochtans is dit gebruik eenvoudig conventie. Bewerkbare sjablonen kunnen worden gebruikt om **elke**-set met inhoud samen te stellen; hoe die inhoud wordt benaderd: als HTML in een browser, zoals JSON wordt verbruikt door JavaScript (AEM SPA Editor) of een mobiele toepassing, is een functie van de manier waarop die pagina wordt opgevraagd.

In AEM Content Services worden bewerkbare sjablonen gebruikt om te definiëren hoe de JSON-gegevens worden weergegeven.

Voor [!DNL WKND Mobile] App, zullen wij één enkel Bewerkbaar Malplaatje creëren dat zal worden gebruikt om één enkel API eindpunt te drijven. Hoewel dit voorbeeld eenvoudig is om de concepten AEM Headless te illustreren, kunt u meerdere Pagina&#39;s (of Eindpunten) maken die elk verschillende sets inhoud blootstellen om een complexere, en beter georganiseerde API te maken.

## Het API-eindpunt

Om te begrijpen hoe te om ons API eindpunt samen te stellen, en te begrijpen welke inhoud aan onze [!DNL WKND Mobile] App zou moeten worden blootgesteld, laten wij het ontwerp terugkeren.

![API voor gebeurtenissen Decomposition](./assets/chapter-4/design-to-component-mapping.png)

Zoals we kunnen zien, hebben we drie logische sets met inhoud die aan de mobiele app moeten worden geleverd.

1. Het **logo**
2. De **Taglijn**
3. De lijst met **Gebeurtenissen**

Om dit te doen, kunnen wij deze vereisten aan AEM Componenten (en in ons geval, AEM de Componenten van de Kern WCM) in kaart brengen om de vereiste inhoud als JSON bloot te stellen.

1. Het **Logo** wordt omringd via een **Image-component**
2. De **Taglijn** wordt omringd via een **Tekstcomponent**
3. De lijst van **Gebeurtenissen** zal via een **component van de Lijst van het Fragment van de Inhoud** worden bedekt die beurtelings, verwijzingen een reeks Fragmenten van de Inhoud van de Gebeurtenis.

>[!NOTE]
>
>Om de JSON-export van Pagina&#39;s en Componenten van AEM Content Service te ondersteunen, moeten de pagina&#39;s en componenten zijn afgeleid van AEM WCM Core Components **.**
>
>[AEM ingebouwde functionaliteit van de ](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) Componenten van de Kern WCM om een genormaliseerd JSON schema van uitgevoerde Pagina&#39;s en Componenten te steunen. Alle mobiele WKND-componenten die in deze zelfstudie worden gebruikt (pagina, afbeelding, tekst en lijst met inhoudsfragmenten), zijn afgeleid van AEM WCM Core-componenten.

## De API-sjabloon voor gebeurtenissen definiëren

1. Ga naar **[!UICONTROL Tools]> [!UICONTROL General] > [!UICONTROL Templates] >[!DNL WKND Mobile]**.

1. Maak de sjabloon **[!DNL Events API]**:

   1. Tik **[!UICONTROL Create]** in de bovenste actiebalk
   1. Selecteer de sjabloon **[!DNL WKND Mobile - Empty Page]**
   1. Tik **[!UICONTROL Next]** in de bovenste actiebalk
   1. **[!DNL Events API]** invoeren in het veld [!UICONTROL Template Title]
   1. Tik **[!UICONTROL Create]** in de bovenste actiebalk
   1. Tik **[!UICONTROL Open]** om de nieuwe sjabloon te openen en te bewerken

1. Eerst, staan wij de drie geïdentificeerde AEM Componenten toe wij de inhoud door [!UICONTROL Policy] van de Wortel [!UICONTROL Layout Container] te bewerken moeten modelleren. Zorg ervoor dat de modus **[!UICONTROL Structure]** actief is, selecteer **[!DNL Layout Container \[Root\]]** en tik op de knop **[!UICONTROL Policy]**.
1. Onder **[!UICONTROL Properties]>[!UICONTROL Allowed Components]** zoek naar **[!DNL WKND Mobile]**. Sta de volgende componenten van de [!DNL WKND Mobile] componentengroep toe zodat kunnen zij op de [!DNL Events] API pagina worden gebruikt.

   * **[!DNL WKND Mobile > Image]**

      * Het logo voor de app
   * **[!DNL WKND Mobile > Text]**

      * De inleidende tekst van de app
   * **[!DNL WKND Mobile > Content Fragment List]**

      * De lijst met gebeurteniscategorieën die beschikbaar zijn voor weergave in de app



1. Tik op het vinkje **[!UICONTROL Done]** in de rechterbovenhoek wanneer dit is voltooid.
1. **Vernieuw** het browservenster om de nieuwe  [!UICONTROL Allowed Components] lijst weer te geven in de linkertrack.
1. Sleep vanuit de Finder Componenten in de linkerspoorstaaf in de volgende AEM Componenten:
   1. **[!DNL Image]** voor het logo
   2. **[!DNL Text]** voor de taglijn
   3. **[!DNL Content Fragment List]** voor de gebeurtenissen
1. **Selecteer de componenten** voor elk van de bovenstaande componenten en druk op  **** unlockbutton.
1. Zorg er echter voor dat de **layout container** **locked** is om te voorkomen dat andere componenten worden toegevoegd of dat deze drie componenten worden verwijderd.
1. Tik op **[!UICONTROL Page Information]>[!UICONTROL View in Admin]** om terug te keren naar de lijst met [!DNL WKND Mobile] sjablonen. Selecteer de nieuwe **[!DNL Events API]**-sjabloon en tik **[!UICONTROL Enable]** in de bovenste actiebalk.

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> De componenten die worden gebruikt om de inhoud te bedekken, worden aan de sjabloon zelf toegevoegd en vergrendeld. Op deze manier kunnen auteurs de vooraf gedefinieerde componenten bewerken, maar niet willekeurig componenten toevoegen of verwijderen, omdat het wijzigen van de API zelf de veronderstellingen rond de JSON-structuur kan onderbreken en de gebruikte apps kan onderbreken. Alle API&#39;s moeten stabiel zijn.

## Volgende stappen

Installeer desgewenst het inhoudspakket [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) op AEM Author via [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Dit pakket bevat de configuraties en inhoud die in dit en vorige hoofdstukken van de zelfstudie worden beschreven.

* [Hoofdstuk 5 - Pagina&#39;s met inhoudsservices ontwerpen](./chapter-5.md)
