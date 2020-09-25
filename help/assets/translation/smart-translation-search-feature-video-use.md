---
title: Smart Translation Search gebruiken met AEM Assets
seo-title: Smart Translation Search gebruiken met AEM Assets
description: Met Smart Translation Search kunt u automatisch zoeken en zoeken in verschillende talen via AEM inhoud, zowel Middelen als Pagina's, waardoor meer dan 50 talen worden ondersteund en de behoefte aan handmatig vertalen van inhoud wordt verminderd.
seo-description: Met Smart Translation Search kunt u automatisch zoeken en zoeken in verschillende talen via AEM inhoud, zowel Middelen als Pagina's, waardoor meer dan 50 talen worden ondersteund en de behoefte aan handmatig vertalen van inhoud wordt verminderd.
uuid: daa6f20f-a4d3-402d-83b9-57d852062a89
discoiquuid: eb2e484a-0068-458f-acff-42dd95a40aab
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Smart Translation Search gebruiken met AEM Assets{#using-smart-translation-search-with-aem-assets}

Met Smart Translation Search kunt u automatisch zoeken en zoeken in verschillende talen via AEM inhoud, zowel Middelen als Pagina&#39;s, waardoor meer dan 50 talen worden ondersteund en de behoefte aan handmatig vertalen van inhoud wordt verminderd.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

Met AEM Smart Translation Search kunnen gebruikers zoeken naar inhoud in AEM met niet-Engelse termen, zodat deze overeenkomen met de elementen in AEM met vergelijkbare Engelse termen.

Smart Translation Search is een perfecte aanvulling op AEM Slimme tags die worden toegepast op middelen in het Engels.

In deze video wordt ervan uitgegaan dat [AEM Smart Translation Search](smart-translation-search-technical-video-setup.md) is ingesteld.

## Hoe Smart Translation Search werkt {#how-smart-translation-search-works}

![Slim vertaalzoeken in stroomdiagram](assets/smart-translation-search-flow.png)

1. AEM gebruiker voert een full-text onderzoek uit, die een gelokaliseerde onderzoekstermijn (bijv. verstrekt. de Spaanse term &quot;man&quot;, &quot;hombre&quot;).
2. De Smart Translation Search, die wordt geleverd door de Apache Oak Machine Translation OSGi-bundel, is betrokken en evalueert of de aangeboden zoektermen kunnen worden vertaald met de geregistreerde taalpakketten.
3. Alle vertaalde termijnen van Stap #2 worden verzameld, en de vraag wordt intern uitgebreid om hen als onderzoekstermijnen te omvatten. Deze uitgebreide reeks zoektermen wordt standaard geëvalueerd aan de hand van AEM zoekindexen die relevante overeenkomsten zoeken.
4. De zoekresultaten die overeenkomen met de oorspronkelijke term (&#39;hombre&#39;) of de vertaalde term (&#39;man&#39;) worden verzameld en geretourneerd als zoekresultaten.

## Aanvullende bronnen{#additional-resources}

* [Smart Translation Search instellen met AEM Assets](smart-translation-search-technical-video-setup.md)
* [Apache Joshua Language Packs](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)