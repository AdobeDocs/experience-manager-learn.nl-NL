---
title: Voorste pijplijn inschakelen voor Standaard AEM Projectarchetype
description: Converteer een standaard AEM-project voor snellere implementatie van statische bronnen, zoals CSS, JavaScript, Fonts, Icons met behulp van front-end pipe. En scheiding van front-end ontwikkeling van full-stack back-end ontwikkeling op AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Voorste pijplijn inschakelen voor Standaard AEM Projectarchetype{#enable-front-end-pipeline-standard-aem-project}

Leer hoe u het standaard AEM project dat u maakt met [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype) om statische middelen zoals CSS, JavaScript, Doopvonten, Pictogrammen op te stellen gebruikend de pijpleiding van het Voorste-Eind voor snellere ontwikkeling-aan-plaatsingscyclus. En scheiding van front-end ontwikkeling van full-stack back-end ontwikkeling op AEM. U leert ook hoe deze statische bronnen zijn __niet__ gediend van AEM bewaarplaats maar van CDN, een verandering in leveringsparadigm.

Een nieuwe front-end pijpleiding wordt gecreeerd in de Manager van de Wolk van Adobe die slechts bouwt en opstelt `ui.frontend` artefacten aan CDN en informeert AEM over zijn plaats. Tijdens het genereren van de HTML van de webpagina `<link>` en `<script>` tags verwijzen naar deze locatie in het dialoogvenster `href` kenmerkwaarde.

Na de standaard AEM projectomzetting, kunnen de front-end ontwikkelaars afzonderlijk van en parallel aan om het even welke volledig-stapel achterste-eindontwikkeling op AEM werken, die zijn eigen plaatsingspijpleidingen heeft.

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>Dit is alleen van toepassing op AEM as a Cloud Service en niet voor op AMS gebaseerde Adobe Cloud Manager-implementaties.

