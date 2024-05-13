---
title: Toepassingen bewerken met Universal Editor
description: Leer hoe u de inhoud van een voorbeeld van een React-app bewerkt met de Universal Editor.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
source-git-commit: 14767141348d3d56c154704cc21d39722bb67aec
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
   - **AEM SDK**: De inhoud wordt geleverd in Content Fragments voor de React-app met GraphQL API&#39;s.
   - **Toepassingen Reageren**: een eenvoudige gebruikersinterface die de inhoud van AEM weergeeft.
   - **Universal Editor-service**: a _lokale kopie van de Universal Editor-service_ die de Universal Editor en de AEM SDK bindt.
- De React-app instrumenten om de inhoud te bewerken met de Universal Editor
- De inhoud van de React-app bewerken met Universal Editor


## Overzicht van de Universal Editor

De Universele Redacteur machtigt inhoudsauteurs en ontwikkelaars (front-end en backend), herzien enkele zeer belangrijke voordelen van Universele Redacteur:

- Gebouwd om inhoud zonder kop en zonder kop te bewerken in de WYSIWYG-modus (what-you-see-is-what-you-get).
- Biedt een consistente ervaring voor het bewerken van inhoud op verschillende front-end technologieën, zoals Reageren, Angular, Waarde, enzovoort. De auteurs van de inhoud kunnen de inhoud dus bewerken zonder zich zorgen te maken over de onderliggende front-end technologie.
- Er is zeer weinig instrumentatie vereist om de Universal Editor in de front-end toepassing in te schakelen. Zo maximaliseert u de productiviteit van de ontwikkelaar en maakt u ze vrij om de ervaring op te bouwen.
- Scheiding van zorgen over drie rollen, inhoudauteurs, front-end ontwikkelaars, en backendontwikkelaars, die elke rol toestaan om zich op zijn kernverantwoordelijkheden te concentreren.


## Voorbeeld van React-app

Deze zelfstudie gebruikt [**WKND-teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) als het voorbeeld React app. De **WKND-teams** Reactie-app geeft een lijst met teamleden en hun gegevens weer.

De gegevens van het Team zoals titel, beschrijving, en teamleden worden opgeslagen zoals _Team_ Content Fragments in AEM. Op dezelfde manier worden de Persoon-details zoals naam, biografie en profielfoto opgeslagen als _Persoon_ Content Fragments in AEM.

De inhoud voor de React-app wordt geleverd door AEM met GraphQL API&#39;s en de gebruikersinterface is samengesteld met behulp van twee React-componenten. `Teams` en `Person`.

Er is een corresponderende zelfstudie beschikbaar om te leren hoe u de **WKND-teams** Reageer de app. U vindt de zelfstudie [hier](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview).

## Volgende stap

Leer hoe u [de plaatselijke ontwikkelomgeving opzetten](./local-development-setup.md).
