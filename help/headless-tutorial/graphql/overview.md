---
title: Aan de slag met AEM Headless - GraphQL
description: Leer meer over de Experience Manager GraphQL API's en hun mogelijkheden.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# Aan de slag met AEM headless - GraphQL {#getting-started-with-aem-headless}

{{aem-headless-trials-promo}}

GraphQL API&#39;s voor inhoudsfragmenten AEM
ondersteunt CMS-scenario&#39;s zonder kop waarbij externe clienttoepassingen ervaringen renderen met inhoud die in AEM wordt beheerd.

Een moderne API voor het leveren van inhoud is essentieel voor de efficiëntie en prestaties van op JavaScript gebaseerde frontendtoepassingen. Het gebruiken van REST API introduceert uitdagingen:

* Een groot aantal aanvragen om één object tegelijk op te halen
* Vaak &#39;overlevert&#39;-inhoud, wat betekent dat de toepassing meer ontvangt dan nodig is

Om deze uitdagingen te overwinnen verstrekt GraphQL een op vraag-gebaseerde API die cliënten toestaat om AEM voor slechts de inhoud te vragen het vereist, en het gebruiken van één enkele API vraag te ontvangen.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

Deze video is een overzicht van de GraphQL API die in AEM is geïmplementeerd. De GraphQL API in AEM is vooral ontworpen om AEM toepassingen voor inhoudsfragmenten te leveren aan downstreamtoepassingen als onderdeel van een headless-implementatie.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="Aan de slag met AEM headless - GraphQL"
>abstract="Leer hoe u inhoudsfragmenten kunt leveren met GraphQL."
>additional-url="https://video.tv.adobe.com/v/328618" text="Overzicht van GraphQL in AEM"

## AEM GraphQL-videoreeks zonder hoofd

Leer hoe u de GraphQL-mogelijkheden kunt AEM via de diepgaande doorloop van Content Fragments en het AEM van GraphQL API&#39;s en ontwikkelingsprogramma&#39;s.

* [AEM GraphQL-videoreeks zonder hoofd](./video-series/modeling-basics.md)

## Zelfstudie voor GraphQL-handleiding zonder hoofd AEM

Ontdek AEM GraphQL-mogelijkheden door een React-app te ontwikkelen die contentfragmenten gebruikt via AEM GraphQL API&#39;s.

* [Zelfstudie voor GraphQL-handleiding zonder hoofd AEM](./multi-step/overview.md)

## AEM GraphQL versus AEM Content Services

|                                | GraphQL API&#39;s AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schema-definitie | Modellen voor structuurinhoudsfragmenten | AEM componenten |
| Inhoud | Inhoudsfragmenten | AEM componenten |
| Inhoud detecteren | Op GraphQL-query | Op AEM pagina |
| Afleveringsformaat | GraphQL JSON | AEM ComponentExporter JSON |
