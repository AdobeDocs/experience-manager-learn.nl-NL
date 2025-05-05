---
title: Aan de slag met de AEM SPA Editor en Angular
description: Maak uw eerste Angular Single Page Application (SPA) die in Adobe Experience Manager en AEM met de WKND SPA kan worden bewerkt.
version: Experience Manager as a Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 123
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---

# Maak je eerste Angular SPA in AEM {#introduction}

{{edge-delivery-services}}

Onthaal aan een multi-part leerprogramma dat voor ontwikkelaars nieuw aan de **eigenschap van de Redacteur van het KUUROORD** in Adobe Experience Manager (AEM) wordt ontworpen. In deze zelfstudie wordt de implementatie besproken van een Angular-toepassing voor een fictief levensstijlmerk, de WKND. De Angular-app is ontwikkeld en ontworpen voor implementatie met de AEM SPA Editor, die Angular-componenten toewijst aan AEM-componenten. De voltooide SPA, die aan AEM wordt opgesteld, kan dynamisch worden ontworpen met traditionele in-line het uitgeven hulpmiddelen van AEM.

![ Definitief Uitgevoerde SPA ](assets/wknd-spa-implementation.png)

*WKND de Implementatie van het KUUROORD*

## Info

Het doel voor dit meerdelige leerprogramma is een ontwikkelaar te leren hoe te om een toepassing van Angular uit te voeren om met de eigenschap van de Redacteur van het KUUROORD van AEM te werken. In een real-world scenario worden de ontwikkelingsactiviteiten onderverdeeld door persona, vaak het impliceren van de ontwikkelaar van het Voorste Eind van a **&#x200B;**&#x200B;en a **Achterste ontwikkelaar van het Eind**. Wij geloven het voor om het even welke ontwikkelaar die bij een project van de Redacteur van AEM SPA betrokken is om deze zelfstudie te voltooien.

Het leerprogramma wordt ontworpen om met **AEM as a Cloud Service** te werken en is achterwaarts compatibel met **AEM 6.5.4+** en **AEM 6.4.8+**. Het SPA wordt uitgevoerd gebruikend:

* [ Gemaakt AEM Project Archetype ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=nl-NL)
* [ de Redacteur van AEM SPA ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=nl-NL#content-editing-experience-with-spa)
* [ Componenten van de Kern ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=nl-NL)
* [ Angular ](https://angular.io/)

*schat 1-2 uren om door elk deel van het leerprogramma te krijgen.*

## Laatste code

Al leerprogramma code kan op [ GitHub ](https://github.com/adobe/aem-guides-wknd-spa) worden gevonden.

De [ recentste codebasis ](https://github.com/adobe/aem-guides-wknd-spa/releases) is beschikbaar als downloadbare Pakketten van AEM.

## Vereisten

Voordat u deze zelfstudie start, hebt u het volgende nodig:

* Basiskennis van HTML, CSS en JavaScript
* Basis vertrouwdheid met [ Angular ](https://angular.io/)
* [ AEM as a Cloud Service SDK ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=nl-NL#download-the-aem-as-a-cloud-service-sdk), [ AEM 6.5.4+ ](https://helpx.adobe.com/nl/experience-manager/aem-releases-updates.html#65) of [ AEM 6.4.8+ ](https://helpx.adobe.com/nl/experience-manager/aem-releases-updates.html#64)
* [ Java ](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [ Apache Maven ](https://maven.apache.org/) (3.3.9 of nieuwer)
* [ Node.js ](https://nodejs.org/en/) en [ npm ](https://www.npmjs.com/)

*terwijl niet vereist, is het nuttig om een basisbegrip van [ het ontwikkelen van traditionele componenten van AEM Sites ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=nl-NL) te hebben.*

## Lokale ontwikkelomgeving {#local-dev-environment}

Er is een lokale ontwikkelomgeving nodig om deze zelfstudie te voltooien. De schermafbeeldingen en de video worden gevangen gebruikend AEM as a Cloud Service SDK die op een milieu van Mac OS met [ Code van Visual Studio ](https://code.visualstudio.com/) als winde loopt. Opdrachten en code moeten onafhankelijk zijn van het lokale besturingssysteem, tenzij anders aangegeven.

>[!NOTE]
>
> **Nieuw aan AEM as a Cloud Service?** Controle uit de [ volgende gids aan vestiging een lokale ontwikkelomgeving gebruikend AEM as a Cloud Service SDK ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL).
>
> **Nieuw aan AEM 6.5?** Controle uit de [ volgende gids aan vestiging een lokale ontwikkelomgeving ](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=nl-NL).

## Volgende stappen {#next-steps}

Waar wacht u op?! Begin het leerprogramma door aan het [ hoofdstuk van het Project van de Redacteur van het KUUROORD ](create-project.md) te navigeren en te leren hoe te om een Redacteur van het KUUROORD toegelaten project te produceren gebruikend het Archetype van het Project van AEM.

## Achterwaartse compatibiliteit {#compatibility}

De projectcode voor deze zelfstudie is gemaakt voor AEM as a Cloud Service. Om de projectcode achterwaarts compatibel te maken voor **6.5.4+** en **6.4.8+** zijn verscheidene wijzigingen aangebracht.

[ UberJar ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=nl-NL#what-is-the-uberjar) **v6.4.4** is inbegrepen als gebiedsdeel:

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

Er is een extra Maven-profiel met de naam `classic` toegevoegd om de build aan te passen aan de AEM 6.x-omgevingen:

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

Het profiel `classic` is standaard uitgeschakeld. Als u de zelfstudie met AEM 6.x volgt, voegt u het `classic` -profiel toe wanneer u de instructie hebt gekregen om een Maven-build uit te voeren:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Wanneer het produceren van een nieuw project voor een implementatie van AEM gebruikt altijd de recentste versie van [ Archetype van het Project van AEM ](https://github.com/adobe/aem-project-archetype) en werkt `aemVersion` bij om uw voorgenomen versie van AEM te richten.
