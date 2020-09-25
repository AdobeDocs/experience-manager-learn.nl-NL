---
title: ContextHub van de opstelling voor Personalisatie met AEM Sites
description: ContextHub is een kader voor het opslaan van, het manipuleren van, en het voorstellen van contextgegevens. De JavaScript API van ContextHub laat u toe om tot opslag toegang te hebben om, gegevens tot stand te brengen bij te werken en te schrappen zonodig. Als dusdanig, vertegenwoordigt ContextHub een gegevenslaag op uw pagina's. Deze pagina beschrijft hoe te om contexthub aan uw AEM sitepagina's toe te voegen.
feature: context-hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 1%

---


# ContextHub instellen voor personalisatie {#set-up-contexthub}

ContextHub is een kader voor het opslaan van, het manipuleren van, en het voorstellen van contextgegevens. De JavaScript API van ContextHub laat u toe om tot opslag toegang te hebben om, gegevens tot stand te brengen bij te werken en te schrappen zonodig. Als dusdanig, vertegenwoordigt ContextHub een gegevenslaag op uw pagina&#39;s. Deze pagina beschrijft hoe te om contexthub aan uw AEM sitepagina&#39;s toe te voegen.

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>We gebruiken de WKND-referentiesite voor deze video en deze maakt geen deel uit van AEM release. U kunt de [nieuwste versie hier](https://github.com/adobe/aem-guides-wknd/releases)downloaden.

Voeg ContextHub aan uw pagina&#39;s toe om de eigenschappen ContextHub toe te laten en met de bibliotheken van JavaScript te verbinden ContextHub. De JavaScript-API van ContextHub biedt toegang tot de contextgegevens die door ContextHub worden beheerd.

## ContextHub toevoegen aan een component Page {#adding-contexthub-to-a-page-component}

Om de eigenschappen ContextHub toe te laten en aan de bibliotheken van JavaScript te verbinden ContextHub, omvat de `contexthub` component in de `<head>` sectie van uw Web-pagina. De HTML-code voor uw paginacomponent lijkt op het volgende voorbeeld:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## De Configuratie van de plaats en Segmenten ContextHub {#site-configuration-and-contexthub-segments}

ContextHub omvat een segmenteringsmotor die segmenten beheert en bepaalt welke segmenten voor de huidige context worden opgelost. Verschillende segmenten zijn gedefinieerd. U kunt de Javascript API gebruiken om opgeloste segmenten [te](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)bepalen. Laat de segmenten ContextHub voor uw plaats onder toe [!UICONTROL Configuration Browser].

## Segmenten maken {#create-segments}

Maak AEM segmenten die fungeren als regels voor de theers. Met andere woorden, ze definiëren wanneer inhoud binnen een taser op een webpagina wordt weergegeven. De inhoud kan dan specifiek op de behoeften en belangen van de bezoeker worden gericht, afhankelijk van het segment of de segmenten die zij aanpassen.

## Cloud Configuration, Segmentpad en ContextHub-pad toewijzen aan uw site {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Het toewijzen van de de configuratiepad van de Wolk, segmentatiepad en weg ContextHub aan uw knoop van de plaatswortel zodat kunt u een gepersonaliseerde ervaring voor uw publiek tot stand brengen. Gebruikend ContextHub, kunt u de contextgegevens manipuleren en uw opgeloste segmenten testen.

![CRXDE Lite](assets/crx-de-properties.png)

U kunt meer over ContextHub en segmentatie hieronder lezen:

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [Contexthub toevoegen aan pagina en Toegang tot winkels](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Inzicht in segmentering](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [Het vormen Segmentatie met ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
