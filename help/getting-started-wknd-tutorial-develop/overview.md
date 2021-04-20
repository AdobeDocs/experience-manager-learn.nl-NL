---
title: Aan de slag met AEM Sites - WKND-zelfstudie
description: Aan de slag met AEM Sites - WKND-zelfstudie. De WKND-zelfstudie is een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog geen ervaring hebben met Adobe Experience Manager. De zelfstudie doorloopt de implementatie van een AEM site voor een fictieve levensstijl, de WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, gemaakte archetypes, de Componenten van de Kern, Bewerkbare Malplaatjes, cliëntbibliotheken, en componentenontwikkeling.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 2%

---


# Aan de slag met AEM Sites - WKND-zelfstudie {#introduction}

Welkom bij een meerdelige zelfstudie die is ontworpen voor nieuwe ontwikkelaars in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een AEM site voor een fictieve levensstijl, de WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, de Componenten van de Kern, Bewerkbare Malplaatjes, cliënt-zijbibliotheken, en componentenontwikkeling met Adobe Experience Manager Sites.

## Overzicht {#wknd-tutorial-overview}

Het doel van deze meerdelige zelfstudie is om een ontwikkelaar te leren hoe hij een website implementeert met de nieuwste standaarden en technologieën in Adobe Experience Manager (AEM). Nadat deze zelfstudie is voltooid, moet een ontwikkelaar de basis van het platform begrijpen en kennis hebben van gemeenschappelijke ontwerppatronen in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

De zelfstudie is ontworpen om te werken met **AEM als een Cloud Service** en is achterwaarts compatibel met **AEM 6.5.5.0+** en **AEM 6.4.8.1+**. De site wordt geïmplementeerd met:

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [Kernonderdelen](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Verkoopmodellen
* [Bewerkbare sjablonen](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Stijlsysteem](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Schat 1-2 uur om elk onderdeel van de zelfstudie te doorlopen.*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. Schermafbeeldingen en video worden vastgelegd met behulp van de AEM als Cloud Service SDK die wordt uitgevoerd in een Mac OS-omgeving met [Visual Studio Code](https://code.visualstudio.com/) als IDE. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

### Vereiste software

Het volgende moet lokaal worden geïnstalleerd:

* Lokale AEM **Auteur** instantie (Cloud Service SDK, 6.5.5+ of 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)  (3.3.9 of hoger)
* [Node.js](https://nodejs.org/en/) (LTS - langdurige ondersteuning)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio ](https://code.visualstudio.com/) Codeor gelijkwaardige winde
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  - Gereedschap wordt gebruikt tijdens de zelfstudie

>[!NOTE]
>
> **Nieuw bij AEM als Cloud Service?** Raadpleeg de  [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met de AEM als Cloud Service-SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nieuw bij AEM 6.5?** Raadpleeg de  [volgende handleiding voor het instellen van een lokale ontwikkelomgeving](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Informatie over de zelfstudie {#about-tutorial}

De WKND is een fictief online tijdschrift en blog dat zich concentreert op het nachtleven, activiteiten en gebeurtenissen in verschillende internationale steden.

### Adobe XD UI-kit

Om deze zelfstudie dichter bij een werkelijk scenario te brengen hebben de getalenteerde ontwerpers van UX de modellen voor de plaats gebruikend [Adobe XD](https://www.adobe.com/products/xd.html) gecreeerd. In de loop van de zelfstudie worden verschillende onderdelen van de ontwerpen geïmplementeerd in een volledig voor de auteur geschikte AEM. Met name dankzij **Lorenzo Buosi** en **Kilian Amendola** die het prachtige ontwerp voor de WKND-site hebben gemaakt.

Download de XD UI-kits:

* [UI-kit AEM Core Component](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND-UI-kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

De naam WKND past goed omdat we verwachten dat een ontwikkelaar het betere deel van een ***weekend*** neemt om de zelfstudie te voltooien.

### Github {#github}

Alle code voor het project is te vinden op Github in het AEM Guide repo:

**[GitHub: WKND-siteproject](https://github.com/adobe/aem-guides-wknd)**

Bovendien heeft elk deel van het leerprogramma zijn eigen tak in GitHub. Een gebruiker kan de zelfstudie op elk gewenst moment starten door gewoon de vertakking uit te checken die overeenkomt met het vorige onderdeel.

>[!NOTE]
>
> Als u met de vorige versie van dit leerprogramma werkte, kunt u [oplossingspakketten](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) en [code](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) op GitHub nog vinden.

## Referentiesite {#reference-site}

Een voltooide versie van de WKND-site is ook beschikbaar als referentie: [https://wknd.site/](https://wknd.site/)

De zelfstudie behandelt de belangrijkste ontwikkelingsvaardigheden die nodig zijn voor een AEM ontwikkelaar, maar *niet* bouwt de volledige site van begin tot eind. De voltooide verwijzingsplaats is een andere grote middel om meer van AEM uit de doosmogelijkheden te onderzoeken en te zien.

Om de recentste code te testen alvorens in het leerprogramma te springen, download en installeer **[recentste versie van GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Powered by Adobe Stock

Veel van de afbeeldingen op de WKND Reference-website zijn afkomstig van [Adobe Stock](https://stock.adobe.com/) en zijn materiaal van derden zoals gedefinieerd in de aanvullende voorwaarden voor demo-middelen op [https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html). Als u een Adobe Stock-afbeelding wilt gebruiken voor andere doeleinden dan het weergeven van deze demo-website, zoals het weergeven op een website of in marketingmaterialen, kunt u een licentie aanschaffen op Adobe Stock.

Met Adobe Stock hebt u toegang tot meer dan 140 miljoen hoogwaardige, royaltyvrije afbeeldingen, zoals foto&#39;s, afbeeldingen, video&#39;s en sjablonen om uw creatieve projecten snel te starten.

## Volgende stappen {#next-steps}

Waar wacht u op?! Begin het leerprogramma door aan [het hoofdstuk van de Opstelling van het Project ](project-setup.md) te navigeren en te leren hoe te om een nieuw project van Adobe Experience Manager te produceren gebruikend het AEM Project Archetype.
