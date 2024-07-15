---
title: Toepassingen bewerken met Universal Editor
description: Leer hoe u de inhoud van een voorbeeld van een React-app bewerkt met de Universal Editor.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Toepassingen bewerken met Universal Editor

Leer hoe u de inhoud van een voorbeeld van een React-app bewerkt met de Universal Editor. De inhoud wordt opgeslagen in Content Fragments in AEM en wordt opgehaald met GraphQL API&#39;s.

Deze zelfstudie begeleidt u door het proces waarbij u de lokale ontwikkelomgeving instelt, de React-app van instrumenten voorziet om de inhoud ervan te bewerken en de inhoud bewerkt met de Universal Editor.

## Wat u leert

Deze zelfstudie behandelt de volgende onderwerpen:

- Een kort overzicht van de Universal Editor
- Plaatsing van de lokale ontwikkelomgeving
   - **AEM SDK**: het verstrekt de inhoud die binnen de Fragmenten van de Inhoud voor React app gebruikend GraphQL APIs wordt opgeslagen.
   - **Reageer app**: een eenvoudig gebruikersinterface die de inhoud van AEM toont.
   - **Universele Dienst van de Redacteur**: a _lokaal exemplaar van de Universele dienst van de Redacteur_ die Universele Redacteur en AEM SDK bindt.
- De React-app instrumenten om de inhoud te bewerken met de Universal Editor
- De inhoud van de React-app bewerken met Universal Editor


## Overzicht van de Universal Editor

De Universele Redacteur machtigt inhoudsauteurs en ontwikkelaars (front-end en backend), herzien enkele zeer belangrijke voordelen van Universele Redacteur:

- Gebouwd om inhoud zonder kop en zonder kop te bewerken in de WYSIWYG-modus (what-you-see-is-what-you-get).
- Biedt een consistente ervaring voor het bewerken van inhoud op verschillende front-end technologieën, zoals Reageren, Angular, Waarde, enzovoort. De auteurs van de inhoud kunnen de inhoud dus bewerken zonder zich zorgen te maken over de onderliggende front-end technologie.
- Er is zeer weinig instrumentatie vereist om de Universal Editor in de front-end toepassing in te schakelen. Zo maximaliseert u de productiviteit van de ontwikkelaar en maakt u ze vrij om de ervaring op te bouwen.
- Scheiding van zorgen over drie rollen, inhoudauteurs, front-end ontwikkelaars, en backendontwikkelaars, die elke rol toestaan om zich op zijn kernverantwoordelijkheden te concentreren.


## Voorbeeld van React-app

Dit leerprogramma gebruikt [**WKND Teams** ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) als steekproef Reageer app. De **Teams van WKND** Reageer app toont een lijst van teamleden en hun details.

De details van het Team zoals titel, beschrijving, en teamleden worden opgeslagen als _de Fragmenten van de Inhoud van het Team_ in AEM. Eveneens, worden de Persoon details zoals naam, biografie, en profielbeeld opgeslagen als _Persoon_ de Fragmenten van de Inhoud in AEM.

De inhoud voor de React-app wordt geleverd door AEM met GraphQL API&#39;s en de gebruikersinterface is samengesteld met behulp van twee React-componenten, `Teams` en `Person` .

Een overeenkomstige leerprogramma is beschikbaar om te leren hoe te om de **Teams van WKND** Reageren app te bouwen. U kunt het leerprogramma [ hier ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview) vinden.

## Volgende stap

Leer hoe te [ opstelling de lokale ontwikkelomgeving ](./local-development-setup.md).
