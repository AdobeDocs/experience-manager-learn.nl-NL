---
title: Inhoudsfragmenten leveren in AEM
description: Inhoudsfragmenten, onafhankelijk van de lay-out, kunnen direct in AEM Sites met Core Components worden gebruikt of kunnen zonder kop aan downstreamkanalen worden geleverd.
feature: Content Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 878
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 0%

---

# Inhoudsfragmenten leveren {#delivering-content-fragments}

Adobe Experience Manager (AEM) Content Fragments zijn tekstgebaseerde redactionele inhoud die bepaalde gestructureerde gegevenselementen kan bevatten die zijn gekoppeld, maar die wordt beschouwd als zuivere inhoud zonder ontwerp- of layoutinformatie. Inhoudsfragmenten worden doorgaans gemaakt als agnostische inhoud voor kanalen, die bestemd is voor gebruik en hergebruik via kanalen, waardoor de inhoud zelf wordt verpakt in een contextspecifieke ervaring.

Inhoudsfragmenten, onafhankelijk van de lay-out, kunnen direct in AEM Sites met Core Components worden gebruikt of kunnen zonder kop aan downstreamkanalen worden geleverd.

Deze videoreeks behandelt de leveringsopties voor het gebruiken van de Fragmenten van de Inhoud. De details over het bepalen van en [ authoring de Fragments van de Inhoud kunnen hier ](content-fragments-feature-video-use.md) worden gevonden.

1. Inhoudsfragmenten op webpagina&#39;s gebruiken
2. Inhoudsfragmenten beschikbaar maken als JSON met AEM Content Services
3. De Assets HTTP-API gebruiken

## Inhoudsfragmenten op webpagina&#39;s gebruiken {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

De Fragmenten van de inhoud kunnen op de pagina&#39;s van AEM Sites, of op een gelijkaardige manier, de Fragmenten van de Ervaring worden gebruikt, gebruikend de component van het Fragment van de Componenten van de Kern van AEM WCM [ Inhoud ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html).

Componenten van inhoudsfragmenten kunnen zo nodig worden opgemaakt met het AEM-stijlsysteem om de inhoud weer te geven.

## Inhoudsfragmenten beschikbaar maken als JSON {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services vereenvoudigt het maken van op AEM gebaseerde HTTP-eindpunten waarmee inhoud wordt weergegeven in een genormaliseerde JSON-indeling.

De bovenstaande video gebruikt de [ Component van het Fragment van de Inhoud ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) om individuele Fragmenten van de Inhoud bloot te stellen. De [ Component van de Lijst van het Fragment van de Inhoud ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) is een nieuwe component die een auteur toestaat om een vraag te bepalen die dynamisch de pagina met een lijst van de Fragmenten van de Inhoud zal bevolken. De component Lijst met inhoudsfragmenten heeft de voorkeur wanneer meerdere inhoudsfragmenten moeten worden weergegeven.

*Eind-punt JSON nuttige load van de Diensten van de Inhoud van het Voorbeeld:*\
**[atletes.json](assets/athletes.json)**

## De Assets HTTP-API gebruiken

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

Voor het eerst geïntroduceerd in AEM 6.5 wordt de ondersteuning voor Content Fragments uitgebreid met de Assets HTTP API. Op deze manier kunnen ontwikkelaars op eenvoudige wijze de bewerkingen Maken, Lezen, Bijwerken en Verwijderen (CRUD) uitvoeren op Content Fragments.

*Verzoeken van POSTMAN van het Voorbeeld:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## Welke leveringsmethode moet worden gebruikt

### Webkanaal

De methode voor het leveren van een inhoudsfragment via een webkanaal is eenvoudig met de component Content Fragment in AEM Sites.

### Koploos

Er zijn twee opties om Content Fragment toegankelijk te maken als JSON voor ondersteuning van een kanaal van een derde partij in een hoofdloos geval:

1. Gebruik AEM Content Services en Proxy API-pagina&#39;s (Video #2) wanneer het hoofdgebruik bestaat uit het leveren van Content Fragments voor gebruik (alleen-lezen) door een kanaal van een andere fabrikant. Het Content Services-framework biedt meer flexibiliteit en opties voor de gegevens die worden vrijgegeven. Ontwikkelaars kunnen het Content Services-framework ook uitbreiden om de gegevens te vergroten en/of te verrijken.

2. Gebruik de Assets HTTP API (Video #3) wanneer het kanaal van de derde partij inhoudsfragmenten moet wijzigen en/of bijwerken. Doorgaans wordt inhoud van derden in een AEM-ontwerpomgeving opgenomen als gebruiksgeval.

## Aanvullende bronnen {#additional-resources}

* [Inhoudsfragmenten ontwerpen](content-fragments-feature-video-use.md)
* [ AEM WCM de Componenten van de Kern ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [ AEM WCM de Component van het Fragment van de Inhoud van de Kern ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

U kunt als volgt het pakket downloaden en installeren op een AEM 6.4+-instantie voor de uiteindelijke status van de videoreeks:\
**[aem_demo_liquid-ExperienceContent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
