---
title: Aan de slag met AEM Sites - Project Archetype
description: Aan de slag met AEM Sites - Project Archetype. De WKND-zelfstudie is een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog geen ervaring hebben met Adobe Experience Manager. De zelfstudie doorloopt de implementatie van een AEM site voor een fictieve levensstijl, de WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, gemaakte archetypes, de Componenten van de Kern, Bewerkbare Malplaatjes, cliëntbibliotheken, en componentenontwikkeling.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
index: y
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Inhoudsbeheer, ontwikkeling
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: fb6c56dfc85fbcb36a68210f068fd496849c352e
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 3%

---


# Aan de slag met AEM Sites - Project Archetype {#project-archetype}

Welkom bij een meerdelige zelfstudie die is ontworpen voor nieuwe ontwikkelaars in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een AEM site voor een fictieve levensstijl, de WKND.

Deze zelfstudie begint met het gebruik van [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) om een nieuw project te genereren.

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

## Github {#github}

Alle code voor het project is te vinden op Github in het AEM Guide repo:

**[GitHub: WKND-siteproject](https://github.com/adobe/aem-guides-wknd)**

Bovendien heeft elk deel van het leerprogramma zijn eigen tak in GitHub. Een gebruiker kan de zelfstudie op elk gewenst moment starten door gewoon de vertakking uit te checken die overeenkomt met het vorige onderdeel.

## Volgende stappen {#next-steps}

Waar wacht u op?! Begin het leerprogramma door aan [het hoofdstuk van de Opstelling van het Project ](project-setup.md) te navigeren en te leren hoe te om een nieuw project van Adobe Experience Manager te produceren gebruikend het AEM Project Archetype.
