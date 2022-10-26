---
title: Inhoudsfragmenten in AEM leveren
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: Inhoudsfragmenten, onafhankelijk van de lay-out, kunnen direct in AEM Sites met Core Components worden gebruikt of kunnen zonder kop aan downstreamkanalen worden geleverd.
seo-description: Content Fragments, independent of layout, can be used directly in AEM Sites with Core Components or can be delivered in a headless manner to downstream channels.
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: User
level: Beginner
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# Inhoudsfragmenten leveren {#delivering-content-fragments}

Adobe Experience Manager (AEM) Content Fragments zijn tekstgebaseerde redactionele inhoud die bepaalde gestructureerde gegevenselementen kan bevatten die zijn gekoppeld, maar die wordt beschouwd als zuivere inhoud zonder ontwerp- of layoutinformatie. Inhoudsfragmenten worden doorgaans gemaakt als agnostische inhoud voor kanalen, die bestemd is voor gebruik en hergebruik via kanalen, waardoor de inhoud zelf wordt verpakt in een contextspecifieke ervaring.

Inhoudsfragmenten, onafhankelijk van de lay-out, kunnen direct in AEM Sites met Core Components worden gebruikt of kunnen zonder kop aan downstreamkanalen worden geleverd.

Deze videoreeks behandelt de leveringsopties voor het gebruiken van de Fragmenten van de Inhoud. Details over het definiëren en [authoring Content Fragments can be found here](content-fragments-feature-video-use.md).

1. Inhoudsfragmenten op webpagina&#39;s gebruiken
2. Inhoudsfragmenten beschikbaar maken als JSON met AEM Content Services
3. De HTTP-API voor middelen gebruiken

## Inhoudsfragmenten op webpagina&#39;s gebruiken {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

Inhoudsfragmenten kunnen worden gebruikt op AEM Sites-pagina&#39;s of op een vergelijkbare manier op Experience Fragments, met behulp van de AEM WCM Core Components&#39; [Component Inhoudsfragment](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html).

Componenten van inhoudsfragmenten kunnen zo nodig worden opgemaakt met AEM stijlsysteem.

## Inhoudsfragmenten beschikbaar maken als JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services vereenvoudigt het maken van AEM op pagina gebaseerde HTTP-eindpunten waarmee inhoud wordt weergegeven in een genormaliseerde JSON-indeling.

In de bovenstaande video wordt het [Component Inhoudsfragment](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) om afzonderlijke inhoudsfragmenten zichtbaar te maken. De [Component lijst met inhoudsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) is een nieuwe component waarmee een auteur een query kan definiëren waarmee de pagina dynamisch wordt gevuld met een lijst met inhoudsfragmenten. De component Lijst met inhoudsfragmenten heeft de voorkeur wanneer meerdere inhoudsfragmenten moeten worden weergegeven.

*Voorbeeld van JSON-nuttige last voor inhoudsservices:*\
**[atleten.json](assets/athletes.json)**

## De HTTP-API voor middelen gebruiken

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

Voor het eerst geïntroduceerd in AEM 6.5, wordt verbeterde ondersteuning voor Content Fragments met de middelen HTTP API. Op deze manier kunnen ontwikkelaars op eenvoudige wijze de bewerkingen Maken, Lezen, Bijwerken en Verwijderen (CRUD) uitvoeren op Inhoudsfragmenten.

*Voorbeeld POSTMAN-verzoeken:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Welke leveringsmethode moet worden gebruikt

### Webkanaal

De methode voor het leveren van een inhoudsfragment via een webkanaal is eenvoudig met de component Content Fragment in AEM Sites.

### Koploos

Er zijn twee opties om Content Fragment toegankelijk te maken als JSON voor ondersteuning van een kanaal van een derde partij in een hoofdloos geval:

1. Gebruik AEM Content Services en Proxy API-pagina&#39;s (Video #2) wanneer het hoofdgebruik bestaat uit het leveren van Content Fragments voor gebruik (alleen-lezen) door een kanaal van een andere fabrikant. Het Content Services-framework biedt meer flexibiliteit en opties voor de gegevens die worden vrijgegeven. Ontwikkelaars kunnen het Content Services-framework ook uitbreiden om de gegevens te vergroten en/of te verrijken.

2. Gebruik de HTTP-API voor middelen (Video #3) wanneer het kanaal van een derde partij inhoudsfragmenten moet wijzigen en/of bijwerken. Doorgaans wordt inhoud van derden in een AEM-auteursomgeving opgenomen.

## Aanvullende bronnen {#additional-resources}

* [Inhoudsfragmenten ontwerpen](content-fragments-feature-video-use.md)
* [AEM WCM Core-componenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [AEM WCM Core-inhoudsfragmentcomponent](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

U kunt als volgt het pakket hieronder downloaden en installeren op een AEM 6.4+-instantie voor de uiteindelijke status van de videoreeks:\
**[aem_demo_liquid-experienceContent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
