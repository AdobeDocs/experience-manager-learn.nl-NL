---
title: Adobe-project veilig instellen voor de rekbaarheid van middelen
description: De projecten van de Compute van middelen zijn speciaal bepaalde projecten van het Project van Adobe Vuurwerk, en als dusdanig, vereisen toegang tot het Project van Adobe in de Console van de Ontwikkelaar van de Adobe om hen te vestigen en op te stellen.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Adobe-project probleemloos instellen

De projecten van de Compute van middelen zijn speciaal bepaalde projecten van het Project van Adobe Vuurwerk, en als dusdanig, vereisen toegang tot het Project van Adobe in de Console van de Ontwikkelaar van de Adobe om hen te vestigen en op te stellen.

## Adobe-project veilig maken en instellen in Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)
_Doorklikken voor probleemloos instellen van Adobe-project (geen audio)_

1. Meld u aan bij de [Adobe Developer Console](https://console.adobe.io) met de Adobe ID die is gekoppeld aan de geleverde [accounts en services](./accounts-and-services.md). Zorg ervoor dat u een __Systeembeheerder__ of in de __Developer Role__ voor de juiste Adobe Org bent.
1. Creeer een Vuurwerk project door te tikken __creeer nieuw project > Project van malplaatje > Project Firefly__

   _Als of__ Create nieuwe projectknoop __of het type van__ Project Firefly __niet beschikbaar is, betekent dit uw Adobe Org niet[voorzien van Project Firefly](#request-adobe-project-firefly)._

   + __Titel__ van project: `WKND AEM Asset Compute`
   + __Toepassingsnaam__: `wkndAemAssetCompute<YourName>`
      + De naam __van de__ app moet uniek zijn in alle projecten van Firefly en kan later niet worden gewijzigd. Het voorschrijven van de naam van uw bedrijf of organisatie en het achteraf plaatsen met een zinvol achtervoegsel is een goede aanpak, zoals: `wkndAemAssetCompute`.
      + Voor zelf-enablement is het vaak best om uw naam aan de naam __van de__ App, zoals te postfix `wkndAemAssetComputeJaneDoe` om botsingen met andere projecten van het Project te vermijden Frefly.
   + Voeg onder __Werkruimten__ een nieuwe omgeving toe met de naam `Development`
   + Zorg ervoor dat onder __Adobe I/O Runtime__ de optie Runtime __opnemen bij elke werkruimte__ is ingeschakeld
   + Tik op __Opslaan__ om het project op te slaan
1. Selecteer in het project Adobe Firefly een optie `Development` in de werkruimteselector
1. Tik op __+ Voeg service > API__ toe om de wizard __Add an API__ (Een API toevoegen) te openen. Voeg op deze manier de volgende API&#39;s toe:

   + __Experience Cloud > Asset Compute__
      + Selecteer __Genereer een sleutelpaar__ en tik op de knop __Genereren sleutelpaar__ en sla het gedownloade `config.zip` op een veilige locatie op voor [later gebruik](#private-key)
      + Tik __op volgende__
      + Selecteer de __Integratie van het productprofiel - Cloud Service__ en tik op __geconfigureerde API opslaan__
   + __Adobe Services > I/O-gebeurtenissen__ en tik op __geconfigureerde API opslaan__
   + __Adobe Services > I/O Management API__ en tik op __geconfigureerde API opslaan__

## De toets private.key openen{#private-key}

Bij het instellen van de integratie [van de API voor](#set-up) middelenberekening is een nieuw sleutelpaar gegenereerd en wordt er automatisch een `config.zip` bestand gedownload. Dit `config.zip` bevat het gegenereerde openbare certificaat en het bijbehorende `private.key` bestand.

1. Pak `config.zip` het uitpakken uit naar een veilige plaats op uw bestandssysteem, aangezien het `private.key` later wordt [gebruikt](../develop/environment-variables.md)
   + Geheimen en persoonlijke sleutels mogen nooit als een kwestie van veiligheid aan Git worden toegevoegd.

## Controleer de referenties van de serviceaccount (JWT)

De referenties van dit Adobe I/O-project worden gebruikt door het lokale [Asset Compute Development Tool](../develop/development-tool.md) voor interactie met Adobe I/O Runtime en moeten worden opgenomen in het Asset Compute-project. Verken uzelf met de JWT-referenties (Service Account).

![Accountgegevens van Adobe Developer Service](./assets/firefly/service-account.png)

1. Van het Adobe I/O project van het Project Firefly, zorg ervoor de `Development` werkruimte wordt geselecteerd
1. Tik op de __serviceaccount (JWT)__ onder __Credentials__
1. De weergegeven Adobe I/O-referenties controleren
   + De __openbare sleutel__ die bij de bodem wordt vermeld heeft het __private.key__ tegenhanger in `config.zip` gedownload toen de __Activa Compute API__ aan dit project werd toegevoegd.
      + Als de persoonlijke sleutel verloren gaat of gecompromitteerd, kan de passende openbare sleutel worden verwijderd, en een nieuw zeer belangrijk paar dat in wordt geproduceerd of aan Adobe I/O gebruikend deze interface wordt geupload.
