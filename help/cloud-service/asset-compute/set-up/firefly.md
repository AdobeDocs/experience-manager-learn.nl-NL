---
title: Het Project van de Adobe van de opstelling brandt voor Asset compute rekbaarheid
description: De projecten van de asset compute zijn speciaal bepaalde projecten van het Project van de Adobe Vuurwerk, en als dusdanig, vereisen toegang tot het Project van Adobe in de Console van de Ontwikkelaar van de Adobe om hen te vestigen en op te stellen.
feature: asset compute microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integratie, ontwikkeling
role: Developer
level: Tussentijdse, ervaren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---


# Adobe-project probleemloos instellen

De projecten van de asset compute zijn speciaal bepaalde projecten van het Project van de Adobe Vuurwerk, en als dusdanig, vereisen toegang tot het Project van Adobe in de Console van de Ontwikkelaar van de Adobe om hen te vestigen en op te stellen.

## Adobe-project probleemloos maken en instellen in de Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Doorklikken voor probleemloos instellen van Adobe-project (geen audio)_

1. Meld u aan bij [Adobe Developer Console](https://console.adobe.io) met de Adobe ID die is gekoppeld aan de geleverde [accounts en services](./accounts-and-services.md). Zorg ervoor dat u een __Systeembeheerder__ of __Developer Role__ voor de juiste Adobe Org bent.
1. Creeer een Vuurwerk project door __Nieuw project tot stand te brengen > Project van malplaatje > Project Firefly__ te tikken

   _Als één van beiden nieuwe__ projectknoop __creëren of het__ Project __Fireflytype niet beschikbaar is, betekent dit uw Adobe Org niet  [provisioned met Project Firefly](#request-adobe-project-firefly)._

   + __Titel__ van project:  `WKND AEM Asset Compute`
   + __Toepassingsnaam__:  `wkndAemAssetCompute<YourName>`
      + De __App-naam__ moet uniek zijn voor alle Firefly-projecten en kan later niet worden gewijzigd. Het voorschrijven van de naam van uw bedrijf of organisatie en het achteraf plaatsen met een zinvol achtervoegsel is een goede aanpak, zoals: `wkndAemAssetCompute`.
      + Voor zelf-enablement is het vaak best om uw naam aan __App naam__, zoals `wkndAemAssetComputeJaneDoe` te postfix om botsingen met andere Project te vermijden Firefly projecten.
   + Onder __Workspaces__ voeg een nieuwe omgeving toe met de naam `Development`
   + Onder __Adobe I/O Runtime__ zorgt ervoor dat __Runtime opnemen bij elke werkruimte__ is geselecteerd
   + Tik __Opslaan__ om het project op te slaan
1. Selecteer `Development` in het project Adobe Firefly in de werkruimteselector
1. Tik op __+ Service toevoegen > API__ om de wizard __Voeg een API__ toe te voegen. Gebruik deze methode om de volgende API&#39;s toe te voegen:

   + __Experience Cloud > Asset compute__
      + Selecteer __Een sleutelpaar genereren__ en tik op de knop __Keypair genereren__ en sla de gedownloade `config.zip` op een veilige locatie op voor [later gebruik](#private-key)
      + Tik __Volgende__
      + Selecteer het productprofiel __Integratie - Cloud Service__ en tik __gevormde API opslaan__
   + __Adobe Services > I/O-__ gebeurtenissen en tik op  __geconfigureerde API opslaan__
   + __Adobe Services > I/O Management__ API en tik op  __geconfigureerde API opslaan__

## Access the private.key{#private-key}

Bij het instellen van de [Asset compute-API-integratie](#set-up) is een nieuw sleutelpaar gegenereerd en is automatisch een `config.zip`-bestand gedownload. Deze `config.zip` bevat het gegenereerde openbare certificaat en het overeenkomende `private.key`-bestand.

1. `config.zip` uitpakken naar een veilige plaats op uw bestandssysteem aangezien `private.key` [later wordt gebruikt](../develop/environment-variables.md)
   + Geheimen en persoonlijke sleutels mogen nooit als een kwestie van veiligheid aan Git worden toegevoegd.

## Controleer de referenties van de serviceaccount (JWT)

De geloofsbrieven van dit Adobe I/O project worden gebruikt door het lokale [Hulpmiddel van de Ontwikkeling van de Asset compute](../develop/development-tool.md) om met Adobe I/O Runtime in wisselwerking te staan, en zullen in het project van de Asset compute moeten worden opgenomen. Verken uzelf met de JWT-referenties (Service Account).

![Accountgegevens van Adobe Developer Service](./assets/firefly/service-account.png)

1. Van het project van het Project van de Adobe I/O Vuurwerk, zorg ervoor de `Development` werkruimte wordt geselecteerd
1. Tik op __Serviceaccount (JWT)__ onder __Referenties__
1. De weergegeven Adobe I/O Credentials controleren
   + De __openbare sleutel__ die onderaan wordt vermeld heeft het __private.key__ tegenhanger in `config.zip` gedownload toen __Asset compute API__ aan dit project werd toegevoegd.
      + Als de persoonlijke sleutel verloren gaat of gecompromitteerd, kan de passende openbare sleutel worden verwijderd, en een nieuw zeer belangrijk paar dat binnen wordt geproduceerd of aan Adobe I/O wordt geupload gebruikend deze interface.
