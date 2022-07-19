---
title: Aan de slag met AEM headless - GraphQL
description: Leer over de Experience Manager GraphQL APIs en hun mogelijkheden.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 1%

---

# Aan de slag met AEM zonder kop - GraphQL

AEM GraphQL APIs voor de Fragmenten van de Inhoud steunt hoofd-loze scenario&#39;s CMS waar de externe cliënttoepassingen ervaringen teruggeven gebruikend inhoud die in AEM wordt beheerd.

Een moderne API voor het leveren van inhoud is essentieel voor de efficiëntie en prestaties van op JavaScript gebaseerde frontendtoepassingen. Het gebruiken van REST API introduceert uitdagingen:

* Een groot aantal aanvragen om één object tegelijk op te halen
* Vaak &#39;overlevert&#39;-inhoud, wat betekent dat de toepassing meer ontvangt dan nodig is

Om deze uitdagingen te overwinnen verstrekt GraphQL op vraag-gebaseerde API die cliënten toestaat om AEM voor slechts de inhoud te vragen het vereist, en het gebruiken van één enkele API vraag te ontvangen.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

Deze video is een overzicht van de GraphQL API die in AEM wordt geïmplementeerd. De GraphQL-API in AEM is vooral ontworpen om AEM Content Fragment&#39;s aan downstreamtoepassingen te leveren als onderdeel van een headless-implementatie.

## AEM GraphQL-videoreeks zonder kop

Leer over AEM mogelijkheden GraphQL door de diepgaande analyse van Inhoud Fragments en en AEM GraphQL APIs en ontwikkelingshulpmiddelen.

* [AEM GraphQL-videoreeks zonder kop](./video-series/modeling-basics.md)

## Handleiding voor AEM headless GraphQL Hands-on

Ontdek AEM mogelijkheden GraphQL door een React App te ontwikkelen die inhoudsfragmenten via AEM GraphQL APIs verbruikt.

* [Handleiding voor AEM headless GraphQL Hands-on](./multi-step/overview.md)

## AEM GraphQL vs. AEM Content Services

|  | GraphQL API&#39;s AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schema-definitie | Modellen voor structuurinhoudsfragmenten | AEM componenten |
| Inhoud | Contentfragmenten | AEM componenten |
| Inhoud detecteren | Op GraphQL-query | Op AEM pagina |
| Afleveringsformaat | GraphQL JSON | AEM ComponentExporter JSON |
