---
title: Fouten opsporen in een implementatie van tags
description: Een inleiding aan sommige gemeenschappelijke hulpmiddelen en technieken om een implementatie van Markeringen te zuiveren. Leer hoe te om de de ontwikkelaarsconsole van browser en de Debugger van het Experience Platform uitbreiding te gebruiken om zeer belangrijke aspecten van een implementatie van Markeringen te identificeren en problemen op te lossen.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Fouten opsporen in een implementatie van tags {#debug-tags-implementation}

Een inleiding aan gemeenschappelijke hulpmiddelen en technieken die worden gebruikt om een implementatie van Markeringen te zuiveren. Leer hoe te om de de ontwikkelaarsconsole van browser en de Debugger van het Experience Platform uitbreiding te gebruiken om zeer belangrijke aspecten van een implementatie van Markeringen te identificeren en problemen op te lossen.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Foutopsporing aan de clientzijde via satellietobject

Foutopsporing op de client is handig om het laden of de volgorde van uitvoering van de regel met tageigenschappen te controleren. Wanneer een eigenschap Tag aan de website wordt toegevoegd, `_satellite` JavaScript-object is aanwezig in de browser om het bijhouden van gebeurtenissen en gegevens op de client mogelijk te maken.

Als u foutopsporing op de client wilt inschakelen, roept u de `setDebug(true)` op de `_satellite` object.

1. Open de browserconsole en voer de opdracht onder uit.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Laad de AEM sitepagina opnieuw en verifieer de presentaties van het consolelogboek _regel geactiveerd_ bericht zoals hieronder.

   ![Eigenschap labelen op auteur- en publicatiepagina&#39;s](assets/satellite-object-debugging.png)

## Foutopsporing via Adobe Experience Platform Debugger

Adobe biedt Adobe Experience Platform Debugger [Chrome-extensie](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) en [Firefox-invoegtoepassing](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) om fouten op te sporen, te begrijpen en inzicht te krijgen in de integratie.

1. Open de extensie Adobe Experience Platform Debugger en open de sitepagina op de instantie Publish

1. In de **Adobe Experience Platform Debugger > Summary > Adobe Experience Platform Tags** sectie, verifieer uw de bezitsdetails van de Markering zoals Naam, Versie, Bouwstijl Datum, Milieu, en Uitbreidingen.

   ![Adobe Experience Platform-foutopsporing en eigenschappengegevens voor tags](assets/tag-property-details.png)

## Aanvullende bronnen {#additional-resources}

+ [Inleiding tot de Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Verwijzing naar satellietobject](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
