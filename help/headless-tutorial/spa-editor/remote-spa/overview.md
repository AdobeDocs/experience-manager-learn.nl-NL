---
title: Aan de slag met SPA Editor en externe SPA - Overzicht
description: Welkom bij de meerdelige zelfstudie voor ontwikkelaars die een bestaande externe SPA willen uitbreiden met bewerkbare AEM met AEM SPA Editor.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 336
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 1%

---

# Overzicht

{{edge-delivery-services}}

Welkom bij de meerdelige zelfstudie voor ontwikkelaars die een bestaande, op React gebaseerde (of Next.js) externe SPA willen uitbreiden met bewerkbare AEM met AEM SPA Editor.

Deze zelfstudie bouwt verder op de [WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html), een React-app die AEM inhoud van inhoudsfragmenten via AEM GraphQL-API&#39;s gebruikt, maar die geen in-context-authoring van SPA inhoud biedt.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## Over de zelfstudie

De zelfstudie die is bedoeld om te illustreren hoe een externe SPA, of een SPA die buiten de context van AEM wordt uitgevoerd, kan worden bijgewerkt om inhoud te verbruiken en te leveren die in AEM is geschreven.

De meeste activiteiten in de zelfstudie richten zich op de ontwikkeling van JavaScript, maar kritieke aspecten worden behandeld die rond AEM draaien. Deze aspecten omvatten het definiëren van waar de inhoud wordt geschreven en opgeslagen in AEM en het toewijzen SPA routes naar AEM pagina&#39;s.

De zelfstudie is ontworpen om te werken met **AEM as a Cloud Service** en bestaat uit twee projecten:

1. De __AEM project__ bevat configuratie en inhoud die moeten worden opgesteld aan AEM.
1. __WKND-app__ project is de SPA die met AEM SPA Editor moet worden geïntegreerd

## Laatste code

+ Het beginpunt van de code van deze zelfstudie is te vinden op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) in de `remote-spa-tutorial` map.

## Vereisten

Voor deze zelfstudie is het volgende vereist:

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip of hoger](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql broncode](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Deze zelfstudie gaat uit van:

+ [Microsoft® Visual Studio-code](https://visualstudio.microsoft.com/) als IDE
+ Een werkmap van `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ De AEM SDK uitvoeren als een auteurservice ingeschakeld `http://localhost:4502`
+ De AEM SDK uitvoeren met de lokale `admin` account met wachtwoord `admin`
+ De SPA uitvoeren `http://localhost:3000`

>[!NOTE]
>
> **Hebt u hulp nodig bij het instellen van uw lokale ontwikkelomgeving?** Kijk uit de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met de AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).

## 1. Configureer AEM voor SPA Editor

AEM configuraties zijn vereist om de SPA te integreren met AEM SPA Editor. Deze configuraties worden beheerd en opgesteld via een AEM Project. In dit hoofdstuk, leer over welke configuraties noodzakelijk zijn en hoe te om hen te bepalen.

+ [Leer hoe u AEM voor SPA Editor configureert](./aem-configure.md)

## 2. Bootstrap van de SPA

Om AEM SPA Editor een SPA te kunnen integreren in de ontwerpcontext, moeten er enkele toevoegingen aan de SPA worden aangebracht.

+ [Leer hoe u de SPA voor AEM SPA Editor opnieuw kunt opstarten](./spa-bootstrap.md)

## 3. Bewerkbare vaste componenten

Verken eerst het toevoegen van een bewerkbare &#39;vaste&#39; component aan de SPA. Dit illustreert hoe een ontwikkelaar een specifieke bewerkbare component in de SPA kan plaatsen. Hoewel de auteur de inhoud van de component kan wijzigen, kunnen deze de component niet verwijderen of de plaatsing, plaatsing of grootte ervan wijzigen.

+ [Meer informatie over bewerkbare vaste componenten](./spa-fixed-component.md)

## 4. Bewerkbare containercomponenten

Hierna kunt u een bewerkbare &#39;containercomponent&#39; aan de SPA toevoegen. Dit illustreert hoe een ontwikkelaar een containercomponent in de SPA kan plaatsen. Met containercomponenten kunnen auteurs toegestane componenten erin plaatsen en de lay-out van de componenten aanpassen.

+ [Meer informatie over bewerkbare containercomponenten](./spa-container-component.md)

## 5. Dynamische routes en bewerkbare componenten

Ten slotte, gebruik de concepten die in vorige hoofdstukken aan dynamische routes worden verklaard; routes die verschillende inhoud tonen die op de parameter van de route wordt gebaseerd. Dit illustreert hoe AEM SPA Redacteur aan auteursinhoud op routes kan worden gebruikt die programmatically worden gedreven en worden afgeleid.

+ [Meer informatie over dynamische routes en bewerkbare componenten](./spa-dynamic-routes.md)

## Aanvullende bronnen

+ [Bewerkbare onderdelen SPA Reageren](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
