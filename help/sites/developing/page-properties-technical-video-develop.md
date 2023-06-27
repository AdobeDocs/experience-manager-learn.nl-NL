---
title: Pagina-eigenschappen uitbreiden in AEM Sites
description: Leer hoe u de metagegevensvelden van Pagina-eigenschappen in Adobe Experience Manager Sites kunt uitbreiden. In deze video wordt de meest effectieve manier beschreven om dit te bereiken met de functies van de samenvoeging van bronnen voor bloeden.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 0%

---

# Pagina-eigenschappen uitbreiden {#extending-page-properties-in-aem-sites}

Het aanpassen van de meta-gegevensgebieden voor de Eigenschappen van de Pagina is een algemeen vereiste in om het even welke implementatie van Plaatsen. In deze video wordt de meest effectieve manier beschreven om dit te bereiken met de functies van de samenvoeging van bronnen voor bloeden.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

In de video hierboven ziet u hoe u de pagina-eigenschappen voor de [WKND-naslagsite](https://github.com/adobe/aem-guides-wknd).

## Voorbeeld van pakket met WKND-pagina-eigenschappen

U kunt de meegeleverde [voorbeeld WKND-pagina-eigenschappenpakket](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) bevattende **WKND** en **Basis** tabaanpassingen die in bovenstaande video worden weergegeven. De **SocialMedia** tabaanpassing is niet beschikbaar zoals [WKND-paginacomponent](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) gebruikt nu V3-versie van WCM Core Components en in V3-versie de [delen via sociale media is afgekeurd](https://github.com/adobe/aem-core-wcm-components/pull/1930).

Voor leerdoeleinden kunt u echter de WKND-paginacomponent verwijzen naar de V2-versie van WCM Core-componenten met de functie `sling:resourceSuperType` eigenschapswaarde en bedekking de [Sociale media](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) tab. Zie voor meer informatie [De pagina-eigenschappen configureren](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

Dit voorbeeldpakket moet worden geïnstalleerd op de lokale AEM SDK of AEM 6.X.X-instantie voor leerdoeleinden.
