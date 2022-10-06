---
title: Aan de slag met de AEM SPA Editor en Reageren
description: Maak uw eerste React Single Page Application (SPA) die in Adobe Experience Manager AEM met de WKND-SPA kan worden bewerkt. Leer hoe u een SPA maakt met het React JS-framework met AEM SPA Editor. Deze meerdelige zelfstudie doorloopt de implementatie van een React-toepassing voor een fictief levensstijlmerk, de WKND. In de zelfstudie wordt het einde van de SPA en de integratie met AEM besproken.
sub-product: sites
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# Uw eerste SPA Reageren in AEM maken {#overview}

Welkom bij een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog niet vertrouwd zijn met de **SPA Editor** in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een React-toepassing voor een fictief levensstijlmerk, de WKND. De React-app is ontwikkeld en ontworpen om te worden geïmplementeerd met AEM SPA Editor, die React-componenten toewijst aan AEM componenten. De voltooide SPA, die aan AEM worden opgesteld, kan dynamisch met traditionele in-line het uitgeven hulpmiddelen van AEM worden ontworpen.

![Laatste SPA geïmplementeerd](assets/wknd-spa-implementation.png)

*Implementatie van WKND-SPA*

## Info

De zelfstudie is ontworpen om te werken met **AEM as a Cloud Service** en is achterwaarts compatibel met **AEM 6.5.4+** en **AEM 6.4.8+**.

## Laatste code

Alle zelfstudiecode is te vinden op [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

De [meest recente codebasis](https://github.com/adobe/aem-guides-wknd-spa/releases) is beschikbaar als downloadbare AEM Pakketten.

## Vereisten

Voordat u deze zelfstudie start, hebt u het volgende nodig:

* Basiskennis van HTML, CSS en JavaScript
* Basiskennis van [Reageren](https://reactjs.org/tutorial/tutorial.html)

*Hoewel niet vereist, is het nuttig om een basisbegrip te hebben van [ontwikkeling van traditionele AEM Sites-componenten](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. Screenshots en video worden vastgelegd met de AEM as a Cloud Service SDK die wordt uitgevoerd in een Mac OS-omgeving met [Visual Studio-code](https://code.visualstudio.com/) als IDE. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

### Vereiste software

* [as a Cloud Service SDK AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) of [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 of hoger)
* [Node.js](https://nodejs.org/en/) en [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Nieuw bij AEM as a Cloud Service?** Kijk uit de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met behulp van de AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nieuw bij AEM 6.5?** Kijk uit de [volgende gids voor het opzetten van een lokale ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Volgende stappen {#next-steps}

Waar wacht u op?! De zelfstudie starten door naar de [Project maken](create-project.md) hoofdstuk en leer hoe te om een SPARedacteur toegelaten project te produceren gebruikend het AEM Archieftype van het Project.
