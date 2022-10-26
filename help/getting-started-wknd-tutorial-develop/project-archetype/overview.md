---
title: Aan de slag met AEM Sites - Project Archetype
description: Aan de slag met AEM Sites - Project Archetype. De WKND-zelfstudie is een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog geen ervaring hebben met Adobe Experience Manager. De zelfstudie doorloopt de implementatie van een AEM site voor een fictieve levensstijl, de WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, gemaakte archetypes, de Componenten van de Kern, Bewerkbare Malplaatjes, cliëntbibliotheken, en componentenontwikkeling.
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---

# Aan de slag met AEM Sites - Project Archetype {#project-archetype}

Welkom bij een meerdelige zelfstudie die is ontworpen voor nieuwe ontwikkelaars in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een AEM site voor een fictieve levensstijl, de WKND.

Deze zelfstudie begint met het gebruik van de [Projectarchetype AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) om een nieuw project te genereren.

De zelfstudie is ontworpen om te werken met **AEM as a Cloud Service** en is achterwaarts compatibel met **AEM 6.5.10+**. De site wordt geïmplementeerd met:

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [Kernonderdelen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)
* Verkoopmodellen
* [Bewerkbare sjablonen](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Stijlsysteem](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Schat 1-2 uur om elk onderdeel van de zelfstudie te doorlopen.*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. Screenshots en video worden vastgelegd met de AEM as a Cloud Service SDK die wordt uitgevoerd in een Mac OS-omgeving met [Visual Studio-code](https://code.visualstudio.com/) als IDE. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

### Vereiste software

Het volgende moet lokaal worden geïnstalleerd:

* [Lokale AEM **Auteur** instance](https://experience.adobe.com/#/downloads) (Cloud Service SDK, 6.5.10+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 of hoger)
* [Node.js](https://nodejs.org/en/) (LTS - langdurige ondersteuning)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio-code](https://code.visualstudio.com/) of gelijkwaardige IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Gereedschap dat tijdens de zelfstudie wordt gebruikt

>[!NOTE]
>
> **Nieuw bij AEM as a Cloud Service?** Kijk uit de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met behulp van de AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nieuw bij AEM 6.5?** Kijk uit de [volgende gids voor het opzetten van een lokale ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Github {#github}

Alle code voor het project is te vinden op Github in het AEM Guide repo:

**[GitHub: WKND-siteproject](https://github.com/adobe/aem-guides-wknd)**

Bovendien heeft elk deel van het leerprogramma zijn eigen tak in GitHub. Een gebruiker kan de zelfstudie op elk gewenst moment starten door gewoon de vertakking uit te checken die overeenkomt met het vorige onderdeel.

## Volgende stappen {#next-steps}

Waar wacht u op?! De zelfstudie starten door naar de [Projectinstelling](project-setup.md) hoofdstuk en leer hoe te om een nieuw project van Adobe Experience Manager te produceren gebruikend het AEM Project Archetype.
