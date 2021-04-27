---
title: Aan de slag met SPA Editor en externe SPA - Overzicht
description: Welkom bij de meerdelige zelfstudie voor ontwikkelaars die een bestaande externe SPA willen uitbreiden met bewerkbare AEM met AEM SPA Editor.
topic: Zwaardeloze, SPA, ontwikkeling
feature: SPA Editor, kerncomponenten, API's, ontwikkelen
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: kt-7630.jpg
translation-type: tm+mt
source-git-commit: b6f63110f14ede51fa2dd740aea7cbb623cbec60
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 1%

---


# Overzicht

Welkom bij de meerdelige zelfstudie voor ontwikkelaars die een bestaande, op React gebaseerde (of Next.js) externe SPA willen uitbreiden met bewerkbare AEM met AEM SPA Editor.

Deze zelfstudie is gebaseerd op de [WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html), een React-app die AEM inhoud van inhoudsfragment gebruikt in vergelijking met AEM GraphQL API&#39;s, maar biedt geen in-context-authoring van SPA inhoud.

## Over de zelfstudie

De zelfstudie die is bedoeld om te illustreren hoe een externe SPA, of een SPA die buiten de context van AEM wordt uitgevoerd, kan worden bijgewerkt om inhoud te verbruiken en te leveren die in AEM is geschreven.

De meeste activiteiten in de zelfstudie richten zich op de ontwikkeling van JavaScript, maar kritieke aspecten worden behandeld die rond AEM draaien. Deze aspecten omvatten het definiëren van waar de inhoud wordt geschreven en opgeslagen in AEM en het toewijzen SPA routes naar AEM pagina&#39;s.

De zelfstudie is ontworpen om te werken met **AEM als een Cloud Service** en bestaat uit twee projecten:

1. Het __AEM Project__ bevat configuratie en inhoud die aan AEM moeten worden opgesteld.
1. __WKND__ Appproject is de SPA die met AEM SPA Editor moet worden geïntegreerd

## Laatste code

+ De code van deze zelfstudie kan op [GitHub](https://github.com/adobe/aem-guides-wknd-graphq) op `feature/spa-editor` tak worden gevonden.

## Vereisten

Voor deze zelfstudie is het volgende vereist:

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip of hoger](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql broncode](https://github.com/adobe/aem-guides-wknd-graphql)

Deze zelfstudie gaat uit van:

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas winde
+ Een werkmap van `~/Code/wknd-app`
+ De AEM SDK uitvoeren als een auteurservice op `http://localhost:4502`
+ De AEM SDK uitvoeren met de lokale `admin`-account met wachtwoord `admin`
+ De SPA uitvoeren op `http://localhost:3000`

>[!NOTE]
>
> **Hebt u hulp nodig bij het instellen van uw lokale ontwikkelomgeving?** Raadpleeg de  [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met de AEM als Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).


## Snelle installatie

Met Quick Setup kunt u binnen 15 minuten aan de slag met de WKND App SPA en AEM SPA Editor. Deze versnelde opstelling neemt u rechtstreeks aan de eindstaat van het leerprogramma, toestaand u om het ontwerpen van de SPA in AEM SPA Redacteur te onderzoeken.

+ [Meer informatie over snelle installatie](./quick-setup.md)

## 1. AEM configureren voor SPA Editor

AEM configuraties zijn vereist om de SPA te integreren met AEM SPA Editor. Deze configuraties worden beheerd en opgesteld via een AEM Project. In dit hoofdstuk, leer over welke configuraties noodzakelijk zijn en hoe te om hen te bepalen.

+ [Leer hoe u AEM voor SPA Editor configureert](./aem-configure.md)

## 2. Bootstrap de SPA

Om AEM SPA Editor een SPA te kunnen integreren in de ontwerpcontext, moeten er enkele toevoegingen aan de SPA worden aangebracht.

+ [Leer hoe u de SPA voor AEM SPA Editor opnieuw kunt opstarten](./spa-bootstrap.md)

## 3. Bewerkbare vaste componenten

Verken eerst het toevoegen van een bewerkbare &#39;vaste&#39; component aan de SPA. Dit illustreert hoe een ontwikkelaar een specifieke bewerkbare component in de SPA kan plaatsen. Hoewel de auteur de inhoud van de component kan wijzigen, kunnen deze de component niet verwijderen of de plaatsing, plaatsing of grootte ervan wijzigen.

+ [Meer informatie over bewerkbare vaste componenten](./spa-fixed-component.md)

## 4. Bewerkbare containercomponenten

Hierna kunt u een bewerkbare &#39;containercomponent&#39; aan de SPA toevoegen. Dit illustreert hoe een ontwikkelaar een containercomponent in de SPA kan plaatsen. Met containercomponenten kunnen auteurs toegestane componenten erin plaatsen en de lay-out van de componenten aanpassen.

+ [Meer informatie over bewerkbare containercomponenten](./spa-container-component.md)

## 5. Dynamische routes en bewerkbare componenten

Ten slotte, gebruik de concepten die in vorige hoofdstukken zijn toegelicht aan dynamische routes; routes die verschillende inhoud tonen die op de parameter van de route wordt gebaseerd. Dit illustreert hoe AEM SPA Redacteur aan auteursinhoud op routes kan worden gebruikt die programmatically worden gedreven en worden afgeleid.

+ [Meer informatie over dynamische routes en bewerkbare componenten](./spa-dynamic-routes.md)

## Aanvullende bronnen

+ [Een externe SPA bewerken in AEM documenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCM-componenten - React Core-implementatie](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCM-componenten - Spa-editor - React Core-implementatie](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
