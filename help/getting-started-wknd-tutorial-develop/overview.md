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
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 2%

---


# Getting Started with AEM Sites - WKND Tutorial {#introduction}

Welkom bij een meerdelige zelfstudie die is ontworpen voor nieuwe ontwikkelaars in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een AEM site voor een fictieve levensstijl, de WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, de Componenten van de Kern, Bewerkbare Malplaatjes, cliënt-zijbibliotheken, en componentenontwikkeling met Adobe Experience Manager Sites.

## Overzicht {#wknd-tutorial-overview}

Het doel van deze meerdelige zelfstudie is om een ontwikkelaar te leren hoe hij een website implementeert met de nieuwste standaarden en technologieën in Adobe Experience Manager (AEM). Nadat deze zelfstudie is voltooid, moet een ontwikkelaar de basis van het platform begrijpen en kennis hebben van gemeenschappelijke ontwerppatronen in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

De zelfstudie is ontworpen om met **AEM als Cloud Service** te werken en is achterwaarts compatibel met **AEM 6.5+** en **AEM 6.4.2+**. De site wordt geïmplementeerd met:

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [Kernonderdelen](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Verkoopmodellen
* [Bewerkbare sjablonen](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Stijlsysteem](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Schat 1-2 uur om elk onderdeel van de zelfstudie te doorlopen.*

## Over de zelfstudie {#about-tutorial}

De WKND is een fictief online tijdschrift en blog dat zich concentreert op het nachtleven, activiteiten en gebeurtenissen in verschillende internationale steden.

### Adobe XD UI-kit

Om deze zelfstudie dichter bij een werkelijk scenario te brengen maakten de getalenteerde ontwerpers van UX de modellen voor de plaats gebruikend [Adobe XD](https://www.adobe.com/products/xd.html). In de loop van de zelfstudie worden verschillende onderdelen van de ontwerpen geïmplementeerd in een volledig voor de auteur geschikte AEM. In het bijzonder dank ik **Lorenzo Buosi** en **Kilian Amendola** die het prachtige ontwerp voor de WKND-site hebben gemaakt.

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
> Als u met de vorige versie van dit leerprogramma werkte, kunt u nog de [oplossingspakketten](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) en de [code](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) op GitHub vinden.

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. Schermafbeeldingen en video worden vastgelegd met de AEM als Cloud Service-SDK die wordt uitgevoerd in een Mac OS-omgeving. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

**Nieuw bij AEM als Cloud Service?** Raadpleeg de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met de AEM als Cloud Service-SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).

**Nieuw bij AEM 6.5?** Raadpleeg de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

### Vereiste software

Het volgende moet lokaal worden geïnstalleerd:

* [AEM als Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) of [AEM 6.5](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/technical-requirements.html) of [AEM 6.4 + SP2](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) (alleen AEM 6.5+)
* [Apache Maven](https://maven.apache.org/) (3.3.9 of hoger)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

### Geïntegreerde ontwikkelomgeving (IDE)

Deze zelfstudie gebruikt [Eclipse](https://www.eclipse.org/) met de [AEM Insteekmodule](https://eclipse.adobe.com/aem/dev-tools/) van het Hulpmiddel van de Ontwikkelaar als winde, nochtans om het even welke winde die steun voor Java en Gemaakt projecten heeft kan worden gebruikt. Het vertrouwen op specifieke eigenschappen van winde in dit leerprogramma is minimaal.

Voor gedetailleerde stappen voor het gebruiken van Verduistering of andere IDEs zoals de Code [of](https://code.visualstudio.com/) IntelliJ [van](https://www.jetbrains.com/idea/)Visual Studio, [controleer de volgende gids](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Referentiesite {#reference-site}

Een voltooide versie van de WKND-site is ook beschikbaar als referentie: [https://wknd.site/](https://wknd.site/)

De zelfstudie behandelt de belangrijkste ontwikkelingsvaardigheden die een AEM ontwikkelaar nodig heeft, maar bouwt *niet* de volledige site van begin tot eind. De voltooide verwijzingsplaats is een andere grote middel om meer van AEM uit de doosmogelijkheden te onderzoeken en te zien.

Om de recentste code te testen alvorens in het leerprogramma te springen, download en installeer de **[recentste versie van GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**.

### Powered by Adobe Stock

Veel van de afbeeldingen op de WKND Reference-website zijn afkomstig uit [Adobe Stock](https://stock.adobe.com/) en zijn materiaal van derden zoals gedefinieerd in de aanvullende voorwaarden voor demo-middelen op [https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html). Als u een Adobe Stock-afbeelding wilt gebruiken voor andere doeleinden dan het weergeven van deze demo-website, zoals het weergeven op een website of in marketingmaterialen, kunt u een licentie aanschaffen op Adobe Stock.

Met Adobe Stock hebt u toegang tot meer dan 140 miljoen hoogwaardige, royaltyvrije afbeeldingen, zoals foto&#39;s, afbeeldingen, video&#39;s en sjablonen om uw creatieve projecten snel te starten.

## Volgende stappen {#next-steps}

Waar wacht u op?! Begin het leerprogramma door aan het hoofdstuk van de Opstelling [van het](project-setup.md) Project te navigeren en te leren hoe te om een nieuw project van Adobe Experience Manager te produceren gebruikend het Archetype van het Project van de AEM.
