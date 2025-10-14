---
title: Het worden begonnen met de Redacteur van het KUUROORD en het Verre KUUROORD - Overzicht
description: Welkom bij de meerdelige zelfstudie voor ontwikkelaars die een bestaande Remote SPAs met bewerkbare AEM-inhoud willen uitbreiden met de AEM SPA Editor.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 1%

---

# Overzicht

{{edge-delivery-services}}

Welkom bij de meerdelige zelfstudie voor ontwikkelaars die een bestaande React-based (of Next.js) Verre SPAs met editable AEM inhoud willen uitbreiden gebruikend de Redacteur van AEM SPA.

Dit leerprogramma bouwt op [&#x200B; WKND GraphQL App &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=nl-NL) voort, een React app die de inhoud van het Fragment van de Inhoud van AEM over AEM GraphQL APIs verbruikt, nochtans verstrekt geen in-context authoring van de inhoud van het KUUROORD.

>[!VIDEO](https://video.tv.adobe.com/v/3444853?quality=12&learn=on&captions=dut)

## Over de zelfstudie

Het leerprogramma bedoeld om te illustreren hoe een Verre KUUROORD, of een KUUROORD die buiten de context van AEM loopt, kan worden bijgewerkt om inhoud te verbruiken en te leveren die in AEM wordt authored.

De meeste activiteiten in de zelfstudie zijn gericht op de ontwikkeling van JavaScript, maar er worden kritische aspecten behandeld die rond AEM gaan. Deze aspecten omvatten het bepalen van waar de inhoud wordt geschreven en in AEM wordt opgeslagen en het in kaart brengen van de routes van het KUUROORD aan de Pagina&#39;s van AEM.

Het leerprogramma wordt ontworpen om met **AEM as a Cloud Service** te werken en is samengesteld uit twee projecten:

1. Het __Project van AEM__ bevat configuratie en inhoud die aan AEM moet worden opgesteld.
1. __WKND App__ project is het KUUROORD dat met de Redacteur van het KUUROORD van AEM moet worden geïntegreerd

## Laatste code

+ Het uitgangspunt van de code van dit leerprogramma kan op [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) in de `remote-spa-tutorial` omslag worden gevonden.

## Vereisten

Voor deze zelfstudie is het volgende vereist:

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=nl-NL)
+ [&#x200B; Node.js v18 &#x200B;](https://nodejs.org/en/)
+ [&#x200B; Java™ 11 &#x200B;](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [&#x200B; Gemaakt 3.6+ &#x200B;](https://maven.apache.org/)
+ [&#x200B; Git &#x200B;](https://git-scm.com/downloads)
+ [&#x200B; aem-guides-wknd.all-2.1.0.zip of groter &#x200B;](https://github.com/adobe/aem-guides-wknd/releases)
+ [&#x200B; aem-guides-wknd-grafisch broncode &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Deze zelfstudie gaat uit van:

+ [&#x200B; Microsoft® Visual Studio Code &#x200B;](https://visualstudio.microsoft.com/) als winde
+ Een werkmap van `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ AEM SDK uitvoeren als een auteurservice op `http://localhost:4502`
+ AEM SDK uitvoeren met lokale `admin` account met wachtwoord `admin`
+ SPA uitvoeren op `http://localhost:3000`

>[!NOTE]
>
> **Heb hulp bij vestiging uw lokale ontwikkelomgeving nodig?** Controle uit de [&#x200B; volgende gids aan vestiging een lokale ontwikkelomgeving gebruikend AEM as a Cloud Service SDK &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL).

## &#x200B;1. Vorm AEM for SPA Editor

De configuraties van AEM worden vereist om het KUUROORD met de Redacteur van AEM te integreren SPA. Deze configuraties worden beheerd en geïmplementeerd via een AEM-project. In dit hoofdstuk, leer over welke configuraties noodzakelijk zijn en hoe te om hen te bepalen.

+ [Leer hoe te om AEM voor de Redacteur van het KUUROORD te vormen](./aem-configure.md)

## &#x200B;2. Bootstrap de SPA

Voor de Redacteur van AEM SPA om een KUUROORD in het auteurskader te integreren, moeten een paar toevoegingen aan het KUUROORD worden gemaakt.

+ [Leer hoe te laarzen SPA voor de Redacteur van AEM SPA](./spa-bootstrap.md)

## &#x200B;3. Bewerkbare vaste componenten

Eerst, onderzoek toevoegend een editable &quot;vaste component&quot;aan SPA. Dit illustreert hoe een ontwikkelaar een specifieke editable component, in het KUUROORD kan plaatsen. Hoewel de auteur de inhoud van de component kan wijzigen, kunnen deze de component niet verwijderen of de plaatsing, plaatsing of grootte ervan wijzigen.

+ [Meer informatie over bewerkbare vaste componenten](./spa-fixed-component.md)

## &#x200B;4. Bewerkbare containercomponenten

Daarna, onderzoek toevoegend een editable &quot;containercomponent&quot;aan SPA. Dit illustreert hoe een ontwikkelaar een containercomponent in het KUUROORD kan plaatsen. Met containercomponenten kunnen auteurs toegestane componenten erin plaatsen en de lay-out van de componenten aanpassen.

+ [Meer informatie over bewerkbare containercomponenten](./spa-container-component.md)

## &#x200B;5. Dynamische routes en bewerkbare componenten

Ten slotte, gebruik de concepten die in vorige hoofdstukken aan dynamische routes worden verklaard; routes die verschillende inhoud tonen die op de parameter van de route wordt gebaseerd. Dit illustreert hoe de Redacteur van AEM SPA aan auteursinhoud op routes kan worden gebruikt die programmatically worden gedreven en worden afgeleid.

+ [Meer informatie over dynamische routes en bewerkbare componenten](./spa-dynamic-routes.md)

## Aanvullende bronnen

+ [&#x200B; AEM SPA React Editable Componenten &#x200B;](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
