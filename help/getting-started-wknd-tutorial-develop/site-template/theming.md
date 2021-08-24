---
title: Thematische workflow
seo-title: Aan de slag met AEM Sites - Thema
description: Leer hoe u de themabronnen van een Adobe Experience Manager-site kunt bijwerken om merkspecifieke stijlen toe te passen. Leer hoe u een proxyserver gebruikt om een live voorvertoning van CSS- en Javascript-updates weer te geven. Dit leerprogramma zal ook behandelen hoe te om themaupdates aan een AEMPlaats op te stellen gebruikend Acties GitHub.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Kernonderdelen
topic: Inhoudsbeheer, ontwikkeling
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---


# Thematische workflow {#theming}

>[!CAUTION]
>
> De functies voor snel maken van sites die hier worden getoond, worden in de tweede helft van 2021 gepubliceerd. De bijbehorende documentatie is beschikbaar voor voorbeelddoeleinden.

In dit hoofdstuk werken we de themabronnen van een Adobe Experience Manager-site bij om merkspecifieke stijlen toe te passen. We leren hoe u een proxyserver kunt gebruiken om een voorvertoning van CSS- en JavaScript-updates weer te geven terwijl we code schrijven op de livesite. Dit leerprogramma zal ook behandelen hoe te om themaupdates aan een AEMPlaats op te stellen gebruikend Acties GitHub.

Uiteindelijk wordt onze site bijgewerkt en worden er stijlen in opgenomen die passen bij het WKND-merk.

## Vereisten {#prerequisites}

Dit is een meerdelige zelfstudie en er wordt van uitgegaan dat de stappen in het hoofdstuk [Paginasjablonen](./page-templates.md) zijn voltooid.

## Doelstellingen

1. Leer hoe u de themabronnen voor een site kunt downloaden en wijzigen.
1. Leer hoe u code kunt vergelijken met de livesite voor een real-time voorvertoning.
1. Begrijp het werkschema van begin tot eind van het leveren van gecompileerde CSS en updates JavaScript als deel van een thema gebruikend Acties GitHub.

## Een thema bijwerken {#theme-update}

Breng vervolgens wijzigingen aan in de themabronnen, zodat de site overeenkomt met de vormgeving van het WKND-merk.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Stappen op hoog niveau voor de video:

1. Creeer een lokale gebruiker in AEM voor gebruik met een server van de volmachtsontwikkeling.
1. Download de themabronnen van AEM en open gebruikend lokale winde, zoals VSCode.
1. Wijzig de themabronnen en gebruik een proxy-ontwikkelserver om een voorvertoning van CSS- en JavaScript-wijzigingen in real-time weer te geven.
1. Werk de themabronnen bij zodat het artikel van het tijdschrift overeenkomt met de stijlen en modellen van het merk WKND.

### Oplossingsbestanden

Download de voltooide stijlen voor het [WKND-thema](assets/theming/WKND-THEME-src.zip)

## Een thema implementeren {#deploy-theme}

Stel updates aan een thema aan een AEM milieu op gebruikend Acties GitHub.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

Stappen op hoog niveau voor de video:

1. Voeg uw project van themabronnen aan [GitHub als nieuwe bewaarplaats](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line) toe.
1. Creeer [een persoonlijk toegangstoken in GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) en sparen aan een veilige plaats.
1. Vorm de Montages GitHub in uw AEM milieu om aan uw bewaarplaats te richten GitHub en uw persoonlijk toegangstoken te omvatten.
1. Creeer de volgende [gecodeerde geheimen](https://docs.github.com/en/actions/reference/encrypted-secrets) in uw Bewaarplaats GitHub:
   * **AEM_SITE** - hoofdmap van uw AEM-site (bijv.  `wknd`SITE)
   * **AEM_URL** - URL van uw AEM-auteuromgeving
   * **GIT_TOKEN**  - Persoonlijk toegangstoken van Stap 2.
1. Voer de GitHub-actie uit: **Github-artefacten maken en implementeren**. Dit heeft het downstreameffect van het uitvoeren van de actie: **Werk de themaconfig bij op AEM met artefact id**, die de themabronnen aan het AEM milieu zoals gespecificeerd door `AEM_URL` en `AEM_SITE` zal opstellen.

### Voorbeeldrepo&#39;s

Er zijn een paar rapporten van voorbeeld GitHub die als verwijzing kunnen worden gebruikt:

* [aem-plaats-malplaatje-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  - Gebruikt als voorbeeld voor &quot;real-world&quot;projecten.
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme) - Wordt gebruikt als voorbeeld voor diegenen die de zelfstudie volgen.

## Gefeliciteerd! {#congratulations}

Gefeliciteerd, u hebt zojuist een bijgewerkt thema gemaakt en een thema geïmplementeerd om te AEM!

### Volgende stappen {#next-steps}

Neem een diepere duik in om ontwikkeling te AEM en meer van de onderliggende technologie te begrijpen door een plaats te creëren gebruikend [AEM Project Archetype](../project-archetype/overview.md).
