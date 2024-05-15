---
title: Fouten opsporen in AEM met Repository Browser
description: Browser van de Bewaarplaats is een krachtig hulpmiddel dat zicht in AEM onderliggende gegevensopslag verstrekt, die voor gemakkelijke zuivering van AEM as a Cloud Service milieu toestaat.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Foutopsporing AEM as a Cloud Service met Repository Browser

Browser van de Bewaarplaats is een krachtig hulpmiddel dat zicht in AEM onderliggende gegevensopslag verstrekt, die voor gemakkelijke zuivering van AEM as a Cloud Service milieu toestaat. Browser van de Bewaarplaats steunt een read-only mening van de middelen en de eigenschappen van AEM op Productie, Stadium, en Ontwikkeling, evenals auteur, Publish, en de diensten van de Voorproef.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

Browser voor opslagplaats is __ALLEEN__ beschikbaar in AEM as a Cloud Service omgevingen (gebruik [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) om fouten op te sporen in de lokale AEM (SDK).

## Toegang tot de gegevensopslagbrowser

Toegang krijgen tot de Repository Browser op AEM as a Cloud Service:

1. Controleer of de gebruiker [de vereiste toegang](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. Aanmelden bij [Cloud Manager](https://my.cloudmanager.adobe.com)
1. Selecteer het Programma dat de AEM as a Cloud Service omgeving bevat die u wilt debuggen
1. Open de [Ontwerpconsole](./developer-console.md) komt overeen met de AEM as a Cloud Service omgeving waarin fouten moeten worden opgespoord
1. Selecteer de __Browser voor opslagplaats__ tab
1. Selecteer de AEM servicelaag om te bladeren
   + Alle auteurs
   + Alle uitgevers
   + Alle voorvertoningen
1. Selecteren __Browser voor opslagplaats openen__

Browser van de Bewaarplaats opent voor de geselecteerde de dienstrij (Auteur, Publish, of Voorproef) op read-only wijze, tonend middelen en eigenschappen uw gebruiker heeft toegang tot.

## Toegang voor publiceren en voorvertonen

Standaard is de toegang tot Publiceren of Voorvertoning beperkt, waardoor de beschikbare bronnen in de Repository Browser afnemen. [Als u alle bronnen op de knop Publiceren (of Voorvertoning) wilt weergeven, voegt u gebruikers toe aan de rol Beheerders publiceren (of Voorvertonen).](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
