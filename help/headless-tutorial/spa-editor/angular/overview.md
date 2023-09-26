---
title: Aan de slag met de AEM SPA Editor en Angular
description: Maak uw eerste Angular Single Page Application (SPA) die in Adobe Experience Manager kan worden bewerkt, AEM met de WKND-SPA.
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Uw eerste Angular SPA in AEM maken {#introduction}

{{edge-delivery-services}}

Welkom bij een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog niet vertrouwd zijn met de **SPA Editor** in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een Angular-aanvraag voor een fictieve levensstijl, de WKND. De Angular-app is ontwikkeld en ontworpen om te worden geïmplementeerd met AEM SPA Editor, die Angular-componenten aan AEM componenten toewijst. De voltooide SPA, die aan AEM worden opgesteld, kan dynamisch met traditionele in-line het uitgeven hulpmiddelen van AEM worden ontworpen.

![Laatste SPA geïmplementeerd](assets/wknd-spa-implementation.png)

*Implementatie van WKND-SPA*

## Info

Het doel voor deze meerdelige zelfstudie is om een ontwikkelaar te leren hoe te om een toepassing van de Angular uit te voeren om met de eigenschap van de SPARedacteur van AEM te werken. In een echt scenario worden de ontwikkelingsactiviteiten uitgesplitst per persoon, waarbij vaak een **Front End-ontwikkelaar** en **Ontwikkelaar Terug**. Wij geloven het voor om het even welke ontwikkelaar die bij een project van de SPA van de AEM betrokken is om deze zelfstudie te voltooien.

De zelfstudie is ontworpen om te werken met **AEM as a Cloud Service** en is achterwaarts compatibel met **AEM 6.5.4+** en **AEM 6.4.8+**. De SPA wordt geïmplementeerd met:

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Kernonderdelen](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*Schat 1-2 uur om elk onderdeel van de zelfstudie te doorlopen.*

## Laatste code

Alle zelfstudiecode is te vinden op [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

De [meest recente codebasis](https://github.com/adobe/aem-guides-wknd-spa/releases) is beschikbaar als downloadbare AEM Pakketten.

## Vereisten

Voordat u deze zelfstudie start, hebt u het volgende nodig:

* Basiskennis van HTML, CSS en JavaScript
* Basiskennis van [Angular](https://angular.io/)
* [AS A CLOUD SERVICE SDK AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) of [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 of hoger)
* [Node.js](https://nodejs.org/en/) en [npm](https://www.npmjs.com/)

*Hoewel niet vereist, is het nuttig om een basisbegrip te hebben van [ontwikkeling van traditionele AEM Sites-componenten](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. Screenshots en video worden vastgelegd met de AEM as a Cloud Service SDK die wordt uitgevoerd in een Mac OS-omgeving met [Visual Studio-code](https://code.visualstudio.com/) als IDE. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

>[!NOTE]
>
> **Nieuw bij AEM as a Cloud Service?** Kijk uit de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met de AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nieuw bij AEM 6.5?** Kijk uit de [volgende gids voor het opzetten van een lokale ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Volgende stappen {#next-steps}

Waar wacht u op?! De zelfstudie starten door naar de [SPA Editor-project](create-project.md) hoofdstuk en leer hoe te om een SPARedacteur toegelaten project te produceren gebruikend het AEM Project Archetype.

## Achterwaartse compatibiliteit {#compatibility}

De projectcode voor deze zelfstudie is gemaakt voor AEM as a Cloud Service. Om de projectcode achterwaarts compatibel te maken voor **6.5.4+** en **6.4.8+** er zijn verscheidene wijzigingen aangebracht .

De [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** is opgenomen als een afhankelijkheid:

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

Een extra Maven-profiel, genaamd `classic` is toegevoegd om de build aan te passen aan AEM 6.x-omgevingen:

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

De `classic` profiel is standaard uitgeschakeld. Als u de zelfstudie met AEM 6.x volgt, voegt u de `classic` profiel wanneer de instructie wordt gegeven om een Maven-build uit te voeren:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Wanneer het produceren van een nieuw project voor een AEM implementatie altijd de recentste versie van gebruiken [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype) en de `aemVersion` om uw bedoelde versie van AEM te richten.
