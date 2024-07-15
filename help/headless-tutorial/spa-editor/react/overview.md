---
title: Aan de slag met de AEM SPA Editor en Reageren
description: Maak uw eerste React Single Page Application (SPA) die in Adobe Experience Manager AEM met de WKND-SPA kan worden bewerkt. Leer hoe u een SPA maakt met het React JS-framework met AEM SPA Editor. Deze meerdelige tutorial doorloopt de implementatie van een React-toepassing voor een fictief lifestylemerk, de WKND. In de tutorial wordt het end-to-end maken van de SPA en de integratie met AEM besproken.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 16%

---

# Uw eerste SPA Reageren in AEM maken {#overview}

{{edge-delivery-services}}

Onthaal aan een meerdelige zelfstudie die voor ontwikkelaars wordt ontworpen nieuw aan de **SPA** eigenschap van de Redacteur in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een React-toepassing voor een fictief levensstijlmerk, de WKND. De React app wordt ontwikkeld en ontworpen om met AEM SPA Redacteur worden opgesteld, die React componenten aan AEM componenten in kaart brengt. De voltooide SPA, die aan AEM worden opgesteld, kan dynamisch met traditionele in-line het uitgeven hulpmiddelen van AEM worden ontworpen.

![ Geïmplementeerde definitieve SPA ](assets/wknd-spa-implementation.png)

*WKND SPA Implementatie*

## Info

Het leerprogramma wordt ontworpen om met **AEM as a Cloud Service** te werken en is achterwaarts compatibel met **AEM 6.5.4+** en **AEM 6.4.8+**.

## Laatste code

Al leerprogramma code kan op [ GitHub ](https://github.com/adobe/aem-guides-wknd-spa) worden gevonden.

De [ recentste codebasis ](https://github.com/adobe/aem-guides-wknd-spa/releases) is beschikbaar als downloadbare AEM Pakketten.

## Vereisten

Voordat u deze zelfstudie start, hebt u het volgende nodig:

* Basiskennis van HTML, CSS en JavaScript
* De basis vertrouwdheid met [ Reageert ](https://reactjs.org/tutorial/tutorial.html)

*terwijl niet vereist, is het nuttig om een basisbegrip van [ het ontwikkelen van traditionele componenten van AEM Sites ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) te hebben.*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. De schermafbeeldingen en de video worden gevangen gebruikend AEM as a Cloud Service SDK die op een milieu van Mac OS met [ Code van Visual Studio ](https://code.visualstudio.com/) als winde loopt. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

### Vereiste software

* [ AEM as a Cloud Service SDK ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [ AEM 6.5.4+ ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) of [ AEM 6.4.8+ ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [ Java ](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [ Apache Maven ](https://maven.apache.org/) (3.3.9 of nieuwer)
* [ Node.js ](https://nodejs.org/en/) en [ npm ](https://www.npmjs.com/)

>[!NOTE]
>
> **Nieuw aan AEM as a Cloud Service?** Controle uit de [ volgende gids aan vestiging een lokale ontwikkelomgeving gebruikend AEM as a Cloud Service SDK ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nieuw aan AEM 6.5?** Controle uit de [ volgende gids aan vestiging een lokale ontwikkelomgeving ](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Volgende stappen {#next-steps}

Waar wacht u op?! Begin het leerprogramma door aan [ te navigeren creeer het hoofdstuk van het Project ](create-project.md) en leer hoe te om een SPA Redacteur te produceren toegelaten project gebruikend het Archetype van het AEM Project.
