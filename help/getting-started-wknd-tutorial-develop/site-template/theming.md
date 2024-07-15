---
title: Thematische workflow | Snel site maken AEM
description: Leer hoe u de themabronnen van een Adobe Experience Manager-site kunt bijwerken om merkspecifieke stijlen toe te passen. Leer hoe u een proxyserver gebruikt om een live voorvertoning van CSS- en JavaScript-updates weer te geven. Dit leerprogramma zal ook behandelen hoe te om themaupdates aan een AEMPlaats op te stellen gebruikend Adobe Cloud Manager Front End Pipeline.
version: Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 0%

---

# Thematische workflow {#theming}

In dit hoofdstuk werken we de themabronnen van een Adobe Experience Manager-site bij om merkspecifieke stijlen toe te passen. We leren hoe u een proxyserver kunt gebruiken om een voorvertoning van CSS- en JavaScript-updates weer te geven terwijl we code schrijven op de livesite. Dit leerprogramma zal ook behandelen hoe te om themaupdates aan een AEMPlaats op te stellen gebruikend Adobe Cloud Manager Front End Pipeline.

Uiteindelijk wordt onze site bijgewerkt en worden er stijlen in opgenomen die passen bij het WKND-merk.

## Vereisten {#prerequisites}

Dit is een meerdelig leerprogramma en men veronderstelt dat de stappen die in het [ hoofdstuk van de Malplaatjes van de Pagina ](./page-templates.md) worden geschetst zijn voltooid.

## Doelstellingen

1. Leer hoe u de themabronnen voor een site kunt downloaden en wijzigen.
1. Leer hoe u code kunt vergelijken met de livesite voor een real-time voorvertoning.
1. Begrijp de werkschema van begin tot eind van het leveren van gecompileerde CSS en JavaScript updates als deel van een thema gebruikend de Adobe Voorste Pijpleiding van Cloud Manager.

## Een thema bijwerken {#theme-update}

Breng vervolgens wijzigingen aan in de themabronnen, zodat de site overeenkomt met de vormgeving van het WKND-merk.

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

Stappen op hoog niveau voor de video:

1. Creeer een lokale gebruiker in AEM voor gebruik met een server van de volmachtsontwikkeling.
1. Download de themabronnen van AEM en open gebruikend lokale winde, zoals VSCode.
1. Wijzig de themabronnen en gebruik een proxy-ontwikkelserver om een voorvertoning van CSS- en JavaScript-wijzigingen in real-time weer te geven.
1. Werk de themabronnen bij zodat het artikel van het tijdschrift overeenkomt met de stijlen en modellen van het merk WKND.

### Oplossingsbestanden

Download de gebeëindigde stijlen voor het [ Thema van de Steekproef WKND ](assets/theming/WKND-THEME-src-1.1.zip)

## Een thema implementeren met behulp van een vooruiteindepipet {#deploy-theme}

Implementeer updates van een thema in een AEM omgeving met behulp van Cloud Manager Front End Pipeline.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Stappen op hoog niveau voor de video:

1. Creeer een nieuwe git [ bewaarplaats in Cloud Manager ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. Voeg uw project met themabronnen toe aan de Cloud Manager Git-opslagplaats:

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Vorm a [ Voorste Pijpleiding van het Eind ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) in Cloud Manager om de front eindcode op te stellen.
1. Stel de Voorste Pijpleiding van het Eind in werking om updates aan het doel AEM milieu op te stellen.

### Voorbeeldrepo&#39;s

Er zijn een paar rapporten van voorbeeld GitHub die als verwijzing kunnen worden gebruikt:

* [ aem-plaats-plaats-malplaatje-standaard ](https://github.com/adobe/aem-site-template-standard)
* [ a-plaats-plaats-malplaatje-basic-theme-e2e ](https://github.com/adobe/aem-site-template-basic-theme-e2e) - gebruikt als voorbeeld voor &quot;real-world&quot;projecten.

## Gefeliciteerd! {#congratulations}

U hebt zojuist een thema bijgewerkt en geïmplementeerd om dit te AEM.

### Volgende stappen {#next-steps}

Neem een diepere duik binnen aan AEM ontwikkeling en begrijp meer van de onderliggende technologie door een plaats te creëren gebruikend [ AEM Archetype van het Project ](../project-archetype/overview.md).
