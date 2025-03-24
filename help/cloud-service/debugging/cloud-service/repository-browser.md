---
title: Fouten opsporen in AEM met Repository Browser
description: De Browser van de Bewaarplaats is een krachtig hulpmiddel dat zicht in de onderliggende gegevensopslag van AEM verstrekt, die voor het gemakkelijke zuiveren van het milieu van AEM as a Cloud Service toestaat.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Fouten opsporen in AEM as a Cloud Service met Repository Browser

De Browser van de Bewaarplaats is een krachtig hulpmiddel dat zicht in de onderliggende gegevensopslag van AEM verstrekt, die voor het gemakkelijke zuiveren van het milieu van AEM as a Cloud Service toestaat. Browser van de Bewaarplaats steunt een read-only mening van de middelen en de eigenschappen van AEM op Productie, Stadium, en Ontwikkeling, evenals de auteur, de Publish, en de diensten van de Voorproef.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

Browser van de Bewaarplaats is __slechts__ beschikbaar op de milieu&#39;s van AEM as a Cloud Service (gebruik [ CRXDE Lite ](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) om lokale AEM SDK te zuiveren).

## Toegang tot de gegevensopslagbrowser

Toegang krijgen tot de Repository Browser op AEM as a Cloud Service:

1. Zorg ervoor dat uw gebruiker [ de vereiste toegang ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites) heeft
1. Login aan [ Cloud Manager ](https://my.cloudmanager.adobe.com)
1. Selecteer het programma met de AEM as a Cloud Service-omgeving waarin u de foutopsporing wilt uitvoeren
1. Open [ Developer Console ](./developer-console.md) die aan het milieu van AEM as a Cloud Service beantwoorden om te zuiveren
1. Selecteer Browser van de Bewaarplaats __lusje 0}__
1. Selecteer de AEM service tier om te bladeren
   + Alle auteurs
   + Alle uitgevers
   + Alle voorvertoningen
1. Selecteer __Open Browser van de Bewaarplaats__

Browser van de Bewaarplaats opent voor de geselecteerde de dienstrij (Auteur, Publish, of Voorproef) op read-only wijze, tonend middelen en eigenschappen uw gebruiker heeft toegang tot.

## Toegang voor publiceren en voorvertonen

Standaard is de toegang tot Publiceren of Voorvertoning beperkt, waardoor de beschikbare bronnen in de Repository Browser afnemen. [ om alle middelen bij te bekijken publiceren (of Voorproef) voegt gebruikers aan toe publiceren (of Voorproef) de rol van Beheerders.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
