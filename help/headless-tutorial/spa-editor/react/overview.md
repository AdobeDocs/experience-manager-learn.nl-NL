---
title: Aan de slag met de AEM SPA Editor en Reageren
description: Maak uw eerste React Single Page Application (SPA) die in Adobe Experience Manager AEM met de WKND-SPA kan worden bewerkt. Leer hoe u een SPA maakt met het React JS-framework met AEM SPA Editor. Deze meerdelige tutorial doorloopt de implementatie van een React-toepassing voor een fictief lifestylemerk, de WKND. In de tutorial wordt het end-to-end maken van de SPA en de integratie met AEM besproken.
version: Experience Manager as a Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 71
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 16%

---

# Uw eerste React SPA in AEM maken {#overview}

{{spa-editor-deprecation}}

Onthaal aan een multi-part leerprogramma dat voor ontwikkelaars nieuw aan de **eigenschap van de Redacteur van het KUUROORD** in Adobe Experience Manager (AEM) wordt ontworpen. In deze zelfstudie wordt de implementatie besproken van een React-toepassing voor een fictief levensstijlmerk, de WKND. De React app wordt ontwikkeld en ontworpen om met de Redacteur van het KUUROORD van AEM worden opgesteld, die React componenten aan de componenten van AEM in kaart brengt. De voltooide SPA, die aan AEM wordt opgesteld, kan dynamisch worden ontworpen met traditionele in-line het uitgeven hulpmiddelen van AEM.

![ Definitief Uitgevoerde SPA ](assets/wknd-spa-implementation.png)

*WKND de Implementatie van het KUUROORD*

## Info

Het leerprogramma wordt ontworpen om met **AEM as a Cloud Service** te werken en is achterwaarts compatibel met **AEM 6.5.4+** en **AEM 6.4.8+**.

## Laatste code

Al leerprogramma code kan op [ GitHub ](https://github.com/adobe/aem-guides-wknd-spa) worden gevonden.

De [ recentste codebasis ](https://github.com/adobe/aem-guides-wknd-spa/releases) is beschikbaar als downloadbare Pakketten van AEM.

## Vereisten

Voordat u deze zelfstudie start, hebt u het volgende nodig:

* Basiskennis van HTML, CSS en JavaScript
* De basis vertrouwdheid met [ Reageert ](https://reactjs.org/tutorial/tutorial.html)

*terwijl niet vereist, is het nuttig om een basisbegrip van [ het ontwikkelen van traditionele componenten van AEM Sites ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=nl-NL) te hebben.*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. De schermafbeeldingen en de video worden gevangen gebruikend AEM as a Cloud Service SDK die op een milieu van Mac OS met [ Code van Visual Studio ](https://code.visualstudio.com/) als winde loopt. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

### Vereiste software

* [ AEM as a Cloud Service SDK ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=nl-NL), [ AEM 6.5.4+ ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=nl-NL#aem-65) of [ AEM 6.4.8+ ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=nl-NL#aem-64)
* [ Java ](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [ Apache Maven ](https://maven.apache.org/) (3.3.9 of nieuwer)
* [ Node.js ](https://nodejs.org/en/) en [ npm ](https://www.npmjs.com/)

>[!NOTE]
>
> **Nieuw aan AEM as a Cloud Service?** Controle uit de [ volgende gids aan vestiging een lokale ontwikkelomgeving gebruikend AEM as a Cloud Service SDK ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL).
>
> **Nieuw aan AEM 6.5?** Controle uit de [ volgende gids aan vestiging een lokale ontwikkelomgeving ](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=nl-NL).

## Volgende stappen {#next-steps}

Waar wacht u op?! Begin het leerprogramma door aan [ te navigeren creeer het hoofdstuk van het Project ](create-project.md) en leer hoe te om een redacteur van het KUUROORD toegelaten project te produceren gebruikend het Archetype van het Project van AEM.
