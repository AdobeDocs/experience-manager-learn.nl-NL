---
title: Aan de slag met AEM Sites - Project Archetype
description: Aan de slag met AEM Sites - Project Archetype. De WKND-zelfstudie is een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog geen ervaring hebben met Adobe Experience Manager. De zelfstudie doorloopt de implementatie van een AEM-site voor een fictieve levensstijl, de WKND. De zelfstudie behandelt fundamentele onderwerpen zoals projectopstelling, gemaakte archetypes, de Componenten van de Kern, Bewerkbare Malplaatjes, cliëntbibliotheken, en componentenontwikkeling.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: display, noCatalog
duration: 74
source-git-commit: b612e2e36af8f56661a07577e979959c650564ee
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# Aan de slag met AEM Sites - Project Archetype {#project-archetype}

{{traditional-aem}}

Welkom bij een meerdelige zelfstudie die is ontworpen voor nieuwe ontwikkelaars in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een AEM-site voor een fictieve levensstijl, de WKND.

Dit leerprogramma begint door het [&#x200B; Archetype van het Project van AEM te gebruiken &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=nl-NL) om een nieuw project te produceren.

Het leerprogramma wordt ontworpen om met **AEM as a Cloud Service** te werken en is achterwaarts compatibel met **AEM 6.5.14+**. De site wordt geïmplementeerd met:

* [&#x200B; Gemaakt AEM Project Archetype &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=nl-NL)
* [&#x200B; Componenten van de Kern &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=nl-NL)
* [&#x200B; HTML &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=nl-NL)
* [&#x200B; Sling Models &#x200B;](https://sling.apache.org/documentation/bundles/models.html)
* [&#x200B; Bewerkbare Malplaatjes &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=nl-NL)
* [Stijlsysteem](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=nl-NL)

*schat 1-2 uren om door elk deel van het leerprogramma te krijgen.*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. De schermafbeeldingen en de video worden gevangen gebruikend AEM as a Cloud Service SDK die op een milieu van macOS met [&#x200B; Code van Visual Studio &#x200B;](https://code.visualstudio.com/) als winde loopt. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

### Vereiste software

Het volgende moet lokaal worden geïnstalleerd:

* [&#x200B; Lokale AEM **2&rbrace; instantie van de Auteur** (Cloud Service SDK of 6.5.14+)](https://experience.adobe.com/#/downloads)
* [&#x200B; Java™ 11 &#x200B;](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [&#x200B; Apache Maven &#x200B;](https://maven.apache.org/) (3.3.9 of nieuwer)
* [&#x200B; Node.js &#x200B;](https://nodejs.org/en/) (LTS - steun op lange termijn)
* [&#x200B; npm 6+ &#x200B;](https://www.npmjs.com/)
* [&#x200B; Git &#x200B;](https://git-scm.com/)
* [&#x200B; Code van Visual Studio &#x200B;](https://code.visualstudio.com/) of gelijkwaardige winde
   * [&#x200B; Sync van VSCode AEM &#x200B;](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - Hulpmiddel dat door leerprogramma wordt gebruikt

>[!NOTE]
>
> **Nieuw aan AEM as a Cloud Service?** Controle uit de [&#x200B; volgende gids aan vestiging een lokale ontwikkelomgeving gebruikend AEM as a Cloud Service SDK &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL).
>
> **Nieuw aan AEM 6.5?** Controle uit de [&#x200B; volgende gids aan vestiging een lokale ontwikkelomgeving &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=nl-NL).

## GitHub {#github}

De code van dit leerprogramma kan op GitHub in het antwoord van de Gids van AEM worden gevonden:

**[GitHub: Het Project van Plaatsen van WKND &#x200B;](https://github.com/adobe/aem-guides-wknd)**

Bovendien heeft elk deel van het leerprogramma zijn eigen tak in GitHub. Een gebruiker kan de zelfstudie op elk gewenst moment starten door gewoon de vertakking uit te checken die overeenkomt met het vorige onderdeel.

## Volgende stappen {#next-steps}

Waar wacht u op? Begin het leerprogramma door aan het [&#x200B; hoofdstuk van de Opstelling van het Project &#x200B;](project-setup.md) te navigeren en te leren hoe te om een nieuw project van Adobe Experience Manager te produceren gebruikend het Archetype van het Project van AEM.
