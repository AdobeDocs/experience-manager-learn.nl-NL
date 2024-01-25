---
title: Smart Translation Search gebruiken met AEM Assets
description: Met Smart Translation Search kunt u automatisch zoeken en zoeken in verschillende talen via AEM inhoud, zowel Middelen als Pagina's, waardoor meer dan 50 talen worden ondersteund en de behoefte aan handmatig vertalen van inhoud wordt verminderd.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
doc-type: Feature Video
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
duration: 195
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Smart Translation Search gebruiken met AEM Assets{#using-smart-translation-search-with-aem-assets}

Met Smart Translation Search kunt u automatisch zoeken en zoeken in verschillende talen via AEM inhoud, zowel Middelen als Pagina&#39;s, waardoor meer dan 50 talen worden ondersteund en de behoefte aan handmatig vertalen van inhoud wordt verminderd.

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

Met AEM Smart Translation Search kunnen gebruikers zoeken naar inhoud in AEM met niet-Engelse termen, zodat deze overeenkomen met de elementen in AEM met vergelijkbare Engelse termen.

Smart Translation Search is een perfecte aanvulling op AEM Slimme tags die worden toegepast op middelen in het Engels.

Deze video gaat ervan uit [AEM zoeken naar slimme vertaling](smart-translation-search-technical-video-setup.md) is ingesteld.

## Hoe Smart Translation Search werkt {#how-smart-translation-search-works}

![Slim vertaalzoeken in stroomdiagram](assets/smart-translation-search-flow.png)

1. AEM gebruiker voert een full-text onderzoek uit, die een gelokaliseerde onderzoekstermijn (bijv. verstrekt. de Spaanse term &quot;man&quot;, &quot;hombre&quot;).
2. De Smart Translation Search, die wordt geleverd door de Apache Oak Machine Translation OSGi-bundel, is betrokken en evalueert of de aangeboden zoektermen kunnen worden vertaald met behulp van de geregistreerde taalpakketten.
3. Alle vertaalde termijnen van Stap #2 worden verzameld, en de vraag wordt intern uitgebreid om hen als onderzoekstermijnen te omvatten. Deze uitgebreide reeks zoektermen wordt standaard geëvalueerd aan de hand van AEM zoekindexen die relevante overeenkomsten zoeken.
4. De zoekresultaten die overeenkomen met de oorspronkelijke term (&#39;hombre&#39;) of de vertaalde term (&#39;man&#39;) worden verzameld en geretourneerd als zoekresultaten.

## Aanvullende bronnen{#additional-resources}

* [Smart Translation Search instellen met AEM Assets](smart-translation-search-technical-video-setup.md)
* [Apache Joshua Language Packs](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
