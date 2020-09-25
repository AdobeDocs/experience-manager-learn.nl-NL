---
title: Begonnen het worden met de AEM Redacteur van het KUUROORD en Reageren
description: Creeer uw eerste React Enige Toepassing van de Pagina (SPA) die in de AEM van Adobe Experience Manager met WKND SPA editable is. Leer hoe te om een KUUROORD tot stand te brengen gebruikend het React JS kader met AEM Redacteur van het KUUROORD. Deze meerdelige zelfstudie doorloopt de implementatie van een React-toepassing voor een fictief levensstijlmerk, de WKND. Het leerprogramma behandelt het eind tot eind verwezenlijking van SPA en de integratie met AEM.
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 2%

---


# Creeer uw eerste React SPA in AEM {#overview}

Welkom bij een meerdelige zelfstudie die is ontworpen voor ontwikkelaars die nog niet vertrouwd zijn met de functie **SPA Editor** in Adobe Experience Manager (AEM). In deze zelfstudie wordt de implementatie besproken van een React-toepassing voor een fictief levensstijlmerk, de WKND. React app zal worden ontwikkeld en ontworpen om met AEM Redacteur van het KUUROORD worden opgesteld, die React componenten aan AEM componenten in kaart brengt. Het voltooide KUUROORD, die aan AEM wordt opgesteld, kan dynamisch met traditionele in-line het uitgeven hulpmiddelen van AEM worden ontworpen.

![Uiteindelijke SPA geïmplementeerd](assets/wknd-spa-implementation.png)

*WKND SPA-implementatie*

## Info

Het doel voor dit meerdelige leerprogramma is een ontwikkelaar te onderwijzen hoe te om een React toepassing uit te voeren om met de eigenschap van de Redacteur van het KUUROORD van AEM te werken. In een real-world scenario worden de ontwikkelingsactiviteiten verdeeld door persoonlijkheid, die vaak een ontwikkelaar **van het** Voorste Eind en een ontwikkelaar **van het** Achtereind impliceert. Wij geloven het voor om het even welke ontwikkelaar die aan een AEM project van de Redacteur van het KUUROORD zal worden betrokken om dit leerprogramma te voltooien.

De zelfstudie is ontworpen om met **AEM als Cloud Service** te werken en is achterwaarts compatibel met **AEM 6.5.4+** en **AEM 6.4.8+**. Het SPA wordt uitgevoerd gebruikend:

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA-editor](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Kernonderdelen](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)
* [React JS](https://reactjs.org/)
* [React-app maken](https://create-react-app.dev/)

*Schat 1-2 uur om elk onderdeel van de zelfstudie te doorlopen.*

## Laatste code

Alle leerprogramma code kan op [GitHub](https://github.com/adobe/aem-guides-wknd-spa)worden gevonden.

De [recentste codebasis](https://github.com/adobe/aem-guides-wknd-spa/releases) is beschikbaar als downloadbare AEM Pakketten.

## Vereisten

Voordat u deze zelfstudie start, hebt u het volgende nodig:

* Basiskennis van HTML, CSS en JavaScript
* Basiskennis van [Reageren](https://reactjs.org/tutorial/tutorial.html)
* [AEM als Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) of [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 of hoger)
* [Node.js](https://nodejs.org/en/) en [npm](https://www.npmjs.com/)

*Hoewel dit niet nodig is, is het nuttig om een basisbegrip te hebben van de[ontwikkeling van traditionele AEM Sites-componenten](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html).*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. Schermafbeeldingen en video worden gevangen gebruikend de AEM als Cloud Service SDK die op een milieu van MAC OS met de Code [van](https://code.visualstudio.com/) Visual Studio als winde loopt. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

>[!NOTE]
>
> **Nieuw bij AEM als Cloud Service?** Raadpleeg de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving met de AEM als Cloud Service-SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html).
>
> **Nieuw bij AEM 6.5?** Raadpleeg de [volgende handleiding voor het instellen van een lokale ontwikkelomgeving](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Volgende stappen {#next-steps}

Waar wacht u op?! Begin het leerprogramma door aan het hoofdstuk van het Project [van de Redacteur van het](create-project.md) KUUROORD te navigeren en te leren hoe te om een Redacteur van het KUUROORD toegelaten project te produceren gebruikend het Archetype van het Project van de AEM.

## Achterwaartse compatibiliteit {#compatibility}

De projectcode voor dit leerprogramma werd gebouwd voor AEM als Cloud Service. Om de projectcode achterwaarts compatibel te maken voor **6.5.4+** en **6.4.8+** zijn verschillende wijzigingen aangebracht in de POM-bestanden van de zelfstudie.

De [UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** is opgenomen als een afhankelijkheid:

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

Een extra Maven-profiel, genaamd, `classic` is toegevoegd om de build aan te passen aan AEM 6.x-omgevingen:

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

Het `classic` profiel is standaard uitgeschakeld. Als u de zelfstudie met AEM 6.x volgt, voegt u het `classic` profiel toe wanneer u de instructie krijgt om een Maven-build uit te voeren, dat wil zeggen:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Wanneer het produceren van een nieuw project voor een AEM implementatie altijd gebruik de recentste versie van het [AEM Archieftype](https://github.com/adobe/aem-project-archetype) van het Project en werk `aemVersion` bij om uw voorgenomen versie van AEM te richten.
