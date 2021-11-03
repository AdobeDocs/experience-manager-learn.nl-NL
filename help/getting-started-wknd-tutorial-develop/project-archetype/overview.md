---
title: Aan de slag met AEM Sites - Project Archetype
description: Aan de slag met AEM Sites - Project Archetype. De WKND-zelfstudie is een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog geen ervaring hebben met Adobe Experience Manager. The tutorial walks through the implementation of an AEM site for a fictitious lifestyle brand, the WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, gemaakte archetypes, de Componenten van de Kern, Bewerkbare Malplaatjes, cliëntbibliotheken, en componentenontwikkeling.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: 08146f57235f3de7fd5ab73754166cc85e1f7dda
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---

# Getting Started with AEM Sites - Project Archetype {#project-archetype}

Welkom bij een meerdelige zelfstudie die is ontworpen voor nieuwe ontwikkelaars in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een AEM site voor een fictieve levensstijl, de WKND.

Deze zelfstudie begint met het gebruik van de [Projectarchetype AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) om een nieuw project te genereren.

De zelfstudie is ontworpen om te werken met **AEM as a Cloud Service** en is achterwaarts compatibel met **AEM 6.5.5.0+** en **AEM 6.4.8.1+**. De site wordt geïmplementeerd met:

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [Kernonderdelen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)
* Sling Models
* [Editable Templates](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Stijlsysteem](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Schat 1-2 uur om elk onderdeel van de zelfstudie te doorlopen.*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. Screenshots en video worden vastgelegd met de AEM as a Cloud Service SDK die wordt uitgevoerd in een Mac OS-omgeving met [Visual Studio-code](https://code.visualstudio.com/) als IDE. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

### Vereiste software

The following should be installed locally:

* Local AEM **Author** instance (Cloud Service SDK, 6.5.5+ or 6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 or newer)
* [Node.js](https://nodejs.org/en/) (LTS - Long Term Support)
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

Bovendien heeft elk deel van het leerprogramma zijn eigen tak in GitHub. A user can begin the tutorial at any point by simply checking out the branch that corresponds to the previous part.

## Next Steps {#next-steps}

Waar wacht u op?! De zelfstudie starten door naar de [Projectinstelling](project-setup.md) hoofdstuk en leer hoe te om een nieuw project van Adobe Experience Manager te produceren gebruikend het AEM Project Archetype.
