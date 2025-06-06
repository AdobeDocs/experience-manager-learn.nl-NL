---
title: Fouten opsporen in een implementatie van tags
description: Een inleiding aan sommige gemeenschappelijke hulpmiddelen en technieken om een implementatie van Markeringen te zuiveren. Leer hoe te om de de ontwikkelaarsconsole van browser en de Debugger van het Experience Platform uitbreiding te gebruiken om zeer belangrijke aspecten van een implementatie van Markeringen te identificeren en problemen op te lossen.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 259
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# Fouten opsporen in een implementatie van tags {#debug-tags-implementation}

Een inleiding aan gemeenschappelijke hulpmiddelen en technieken die worden gebruikt om een implementatie van Markeringen te zuiveren. Leer hoe te om de de ontwikkelaarsconsole van browser en de Debugger van het Experience Platform uitbreiding te gebruiken om zeer belangrijke aspecten van een implementatie van Markeringen te identificeren en problemen op te lossen.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Foutopsporing aan de clientzijde via satellietobject

Foutopsporing op de client is handig om het laden of de volgorde van uitvoering van de regel met tageigenschappen te controleren. Wanneer een eigenschap Tag aan de website wordt toegevoegd, is het `_satellite` JavaScript-object aanwezig in de browser om het bijhouden van gebeurtenissen en gegevens op de client te vergemakkelijken.

Als u foutopsporing op de client wilt inschakelen, roept u de methode `setDebug(true)` op het `_satellite` -object aan.

1. Open de browserconsole en voer de opdracht onder uit.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Laad de AEM sitepagina opnieuw, en verifieer consolelogboek toont _in brand gestoken regel_ bericht zoals hieronder.

   ![ Bezit van de Markering op de Pagina&#39;s van de Auteur en van Publish ](assets/satellite-object-debugging.png)

## Foutopsporing via Adobe Experience Platform Debugger

Adobe verstrekt de uitbreiding van Chrome van het Adobe Experience Platform Debugger [&#128279;](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) om, inzicht in de integratie te zuiveren te begrijpen en te krijgen.

1. Open de extensie Adobe Experience Platform Debugger en open de sitepagina op de Publish-instantie

2. In het **Adobe Experience Platform Debugger > Samenvatting > de sectie van de Markeringen van Adobe Experience Platform**, verifieer uw het bezitsdetails van de Markering zoals Naam, Versie, Bouwstijl Datum, Milieu, en Uitbreidingen.

   ![ Adobe Experience Platform Debugger en de Details van het Bezit van de Markering ](assets/tag-property-details.png)

## Aanvullende bronnen {#additional-resources}

+ [ Inleiding aan het Adobe Experience Platform Debugger ](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=nl-NL)

+ [ Satellite objecten verwijzing ](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html?lang=nl-NL)
