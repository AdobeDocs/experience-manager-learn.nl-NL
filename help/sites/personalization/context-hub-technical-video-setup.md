---
title: ContextHub instellen voor Personalization met AEM Sites
description: ContextHub is een kader voor het opslaan van, het manipuleren van, en het voorstellen van contextgegevens. De JavaScript API van ContextHub laat u toe om tot opslag toegang te hebben om, gegevens tot stand te brengen bij te werken en te schrappen zonodig. Als dusdanig, vertegenwoordigt ContextHub een gegevenslaag op uw pagina's. Op deze pagina wordt beschreven hoe u een contexthub kunt toevoegen aan uw AEM-sitepagina's.
feature: Context Hub
version: Experience Manager 6.4, Experience Manager 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 357
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 1%

---

# ContextHub instellen voor Personalization {#set-up-contexthub}

ContextHub is een kader voor het opslaan van, het manipuleren van, en het voorstellen van contextgegevens. De JavaScript API van ContextHub laat u toe om tot opslag toegang te hebben om, gegevens tot stand te brengen bij te werken en te schrappen zonodig. Als dusdanig, vertegenwoordigt ContextHub een gegevenslaag op uw pagina&#39;s. Op deze pagina wordt beschreven hoe u een contexthub kunt toevoegen aan uw AEM-sitepagina&#39;s.

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>We gebruiken de WKND-referentiesite voor deze video en deze maakt geen deel uit van de AEM-release. U kunt de [ recentste versie hier downloaden ](https://github.com/adobe/aem-guides-wknd/releases).

Voeg ContextHub aan uw pagina&#39;s toe om de eigenschappen ContextHub toe te laten en aan de bibliotheken van JavaScript te verbinden ContextHub. De ContextHub JavaScript API verleent toegang tot de contextgegevens die ContextHub beheert.

## ContextHub toevoegen aan een paginacomponent {#adding-contexthub-to-a-page-component}

Als u de ContextHub-functies wilt inschakelen en een koppeling wilt maken naar de ContextHub JavaScript-bibliotheken, neemt u de component `contexthub` op in de sectie `<head>` van uw webpagina. De HTML-code voor uw paginacomponent lijkt op het volgende voorbeeld:

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## De Configuratie van de plaats en Segmenten ContextHub {#site-configuration-and-contexthub-segments}

ContextHub omvat een segmenteringsmotor die segmenten beheert en bepaalt welke segmenten voor de huidige context worden opgelost. Verschillende segmenten zijn gedefinieerd. U kunt Javascript API gebruiken om [ bepaalde opgeloste segmenten ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments) te bepalen. Laat de segmenten ContextHub voor uw plaats onder [[!UICONTROL Configuration Browser] toe ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html).

## Segmenten maken {#create-segments}

Maak AEM-segmenten die fungeren als regels voor de theers. Met andere woorden, ze definiëren wanneer inhoud binnen een taser op een webpagina wordt weergegeven. De inhoud kan dan specifiek op de behoeften en belangen van de bezoeker worden gericht, afhankelijk van het segment of de segmenten die zij aanpassen.

## Cloud Configuration, Segmentpad en ContextHub-pad toewijzen aan uw site {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

Het toewijzen van de de configuratiepad van de Wolk, segmentatiepad en weg ContextHub aan uw knoop van de plaatswortel zodat kunt u een gepersonaliseerde ervaring voor uw publiek tot stand brengen. Gebruikend ContextHub, kunt u de contextgegevens manipuleren en uw opgeloste segmenten testen.

![CRXDE Lite](assets/crx-de-properties.png)

U kunt meer over ContextHub en segmentatie hieronder lezen:

* [ ContextHub ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [ Toevoegend de Hub van de Context aan pagina en het Toegang hebben tot Sporen ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [Inzicht in segmentering](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [ het Vormen Segmentatie met ContextHub ](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
