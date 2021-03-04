---
title: Zelfstudies zonder koppen AEM
description: Een verzameling zelfstudies voor het gebruik van Adobe Experience Manager als een CMS zonder koptekst.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%

---


# Zelfstudies zonder koppen AEM

Adobe Experience Manager heeft meerdere opties om eindpunten zonder kop te definiëren en de inhoud ervan als JSON te leveren. Gebruik praktische zelfstudies om te verkennen hoe u de verschillende opties kunt gebruiken en te kiezen wat bij u past.

## Zelfstudie AEM GraphQL API&#39;s

>[!CAUTION]
>
> De AEM GraphQL API voor de Levering van Inhoudsfragmenten is op verzoek beschikbaar.
> Neem contact op met de Adobe Support om de API voor uw AEM in te schakelen als een Cloud Service-programma.

GrafiekQL APIs voor Inhoudsfragmenten AEM
ondersteunt CMS-scenario&#39;s zonder kop waarbij externe clienttoepassingen ervaringen renderen met inhoud die in AEM wordt beheerd.

Een moderne API voor het leveren van inhoud is essentieel voor de efficiëntie en prestaties van op JavaScript gebaseerde frontendtoepassingen. Het gebruiken van REST API introduceert uitdagingen:

* Een groot aantal aanvragen om één object tegelijk op te halen
* Vaak &#39;overlevert&#39;-inhoud, wat betekent dat de toepassing meer ontvangt dan nodig is

Om deze uitdagingen te overwinnen verstrekt GraphQL op vraag-gebaseerde API die cliënten toestaat om AEM voor slechts de inhoud te vragen het vereist, en het gebruiken van één enkele API vraag te ontvangen.

* Leer hoe te om AEM APIs te gebruiken GraphQL in [Aan de slag met AEM APIs GraphQL leerprogramma](./graphql/overview.md)

## Zelfstudie over verificatie op basis van token

AEM stelt een verscheidenheid van eindpunten van HTTP bloot die met op een headless manier, van GraphQL, AEM de Diensten van de Inhoud aan Activa HTTP API kunnen worden in wisselwerking staan. Vaak moeten deze gebruikers zonder kop zich op AEM verifiëren om toegang te krijgen tot beveiligde inhoud of handelingen. Om dit te vergemakkelijken, steunt AEM symbolisch-gebaseerde authentificatie van HTTP- verzoeken van externe toepassingen, diensten of systemen.

* Leer hoe te om over HTTP voor authentiek te verklaren gebruikend toegangstokens in [voor authentiek te verklaren om als Cloud Service van een externe toepassingsleerprogramma](./authentication/overview.md) te AEM

## Zelfstudie AEM Content Services

AEM Content Services maakt gebruik van traditionele AEM Pagina&#39;s om eindpunten van REST API zonder kop samen te stellen, en AEM Components definiëren (verwijzen naar) de inhoud die op deze eindpunten moet worden weergegeven.

AEM Content Services biedt dezelfde inhoudsabstracties die worden gebruikt voor het ontwerpen van webpagina&#39;s in AEM Sites, om de inhoud en schema&#39;s van deze HTTP API&#39;s te definiëren. Door het gebruik van AEM Pagina&#39;s en AEM Componenten kunnen marketers snel flexibele JSON API&#39;s samenstellen en bijwerken die elke toepassing van stroom kunnen voorzien.

* Leer hoe te om AEM de Diensten van de Inhoud in [Aan de slag met de zelfstudie van de Diensten van de Inhoud van de AEM te gebruiken](./content-services/overview.md)

## AEM GraphQL vs. AEM Content Services

|  | GraphQL API&#39;s AEM | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| Schema-definitie | Modellen voor structuurinhoudsfragmenten | AEM componenten |
| Inhoud | Contentfragmenten | AEM componenten |
| Inhoud detecteren | Op GraphQL-query | Op AEM pagina |
| Afleveringsformaat | GraphQL JSON | AEM ComponentExporter JSON |

## Andere nuttige zelfstudies

Andere AEM die op hoofdloze concepten betrekking hebben omvatten:

* [Aan de slag met de AEM SPA Editor en Angular](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [Aan de slag met de AEM SPA Editor en Reageren](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)