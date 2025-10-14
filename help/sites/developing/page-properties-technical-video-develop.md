---
title: Pagina-eigenschappen uitbreiden in AEM Sites
description: Leer hoe u de metagegevensvelden van Pagina-eigenschappen in Adobe Experience Manager Sites kunt uitbreiden. In deze video wordt de meest effectieve manier beschreven om dit te bereiken met de functies van de samenvoeging van bronnen voor bloeden.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Experience Manager as a Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Pagina-eigenschappen uitbreiden {#extending-page-properties-in-aem-sites}

Het aanpassen van de meta-gegevensgebieden voor de Eigenschappen van de Pagina is een algemeen vereiste in om het even welke implementatie van Plaatsen. In deze video wordt de meest effectieve manier beschreven om dit te bereiken met de functies van de samenvoeging van bronnen voor bloeden.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

De bovenstaande video toont het aanpassen van de paginaeigenschappen voor de [&#x200B; Plaats van de Verwijzing van WKND &#x200B;](https://github.com/adobe/aem-guides-wknd).

## Voorbeeld van pakket met WKND-pagina-eigenschappen

U kunt het verstrekte [&#x200B; pakket van de de paginaaigenschappen van de steekproefWKND &#x200B;](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) gebruiken die **WKND** en **Basis** lusjeverbeteringen bevatten die in hierboven video worden getoond. De **tabaanpassing 0&rbrace; SocialMedia &lbrace;wordt niet verstrekt aangezien [&#x200B; de component van de Pagina van WKND &#x200B;](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) nu V3 versie van de Componenten van de Kern WCM en in V3 versie het [&#x200B; sociale delen &#x200B;](https://github.com/adobe/aem-core-wcm-components/pull/1930) wordt afgekeurd.**

Nochtans voor het leren doeleinden, kunt u de component van de Pagina WKND aan V2 versie van de Componenten van de Kern van WCM richten gebruikend de `sling:resourceSuperType` bezitswaarde en bedekking de [&#x200B; Sociale Media &#x200B;](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) tabel. Voor meer informatie, zie [&#x200B; Vormend uw Eigenschappen van de Pagina &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=nl-NL#configuring-your-page-properties)

Dit voorbeeldpakket moet voor leerdoeleinden worden geïnstalleerd op de lokale AEM SDK- of AEM 6.X.X-instantie.
