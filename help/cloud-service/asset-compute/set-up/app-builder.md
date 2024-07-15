---
title: App Builder instellen voor uitbreidbaarheid van Asset compute
description: Asset compute-projecten zijn speciaal gedefinieerde App Builder-projecten en hebben als zodanig toegang tot App Builder in de Adobe Developer Console nodig om ze op te zetten en te implementeren.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 197
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# App Builder instellen

Asset compute-projecten zijn speciaal gedefinieerde App Builder-projecten en hebben als zodanig toegang tot App Builder in de Adobe Developer Console nodig om ze op te zetten en te implementeren.

## App Builder maken en instellen in Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_klik-door van vestiging upApp Builder (Geen audio)_

1. Login aan [ Adobe Developer Console ](https://console.adobe.io) gebruikend Adobe ID verbonden aan de provisioned [ rekeningen en de diensten ](./accounts-and-services.md). Verzeker u de Beheerder van het a __Systeem__ of in de __Rol van de Ontwikkelaar__ voor de correcte Adobe Org.
1. Creeer een project van App Builder door te tikken __creeer nieuw project > Project van malplaatje > App Builder__

   _als één van beide__ nieuw project __knoop of het__ App Builder __type niet beschikbaar is, betekent dit uw Adobe Org niet [ provisioned met App Builder ](#request-adobe-project-app-builder)._

   + __Titel van het Project__: `WKND AEM Asset Compute`
   + __App naam__: `wkndAemAssetCompute<YourName>`
      + De __naam van de App__ moet uniek over al FApp zijn bouwt af:leiden projecten en is niet later wijzigbaar. Het voorschrijven van de naam van uw bedrijf of organisatie en het achtervoegsel met een zinvol achtervoegsel is een goede aanpak, zoals: `wkndAemAssetCompute` .
      + Voor zelf-enablement is het vaak best om uw naam aan de __naam van de App__, zoals `wkndAemAssetComputeJaneDoe` postfix om botsingen met andere projecten van App Builder te vermijden.
   + Onder __Werkruimten__ voeg een nieuw genoemd milieu toe `Development`
   + Onder __Adobe I/O Runtime__ verzekert __Runtime van de omvat met elke werkruimte__ wordt geselecteerd
   + Tik __sparen__ om het project te bewaren
1. Selecteer in het App Builder-project `Development` in de werkruimteselector
1. Tik __+ voeg de Dienst > API__ toe om __te openen voeg een API__ tovenaar toe, gebruik deze benadering om volgende APIs toe te voegen:

   + __Experience Cloud > Asset compute__
      + Selecteer __produceer een zeer belangrijk paar__ en tik __sleutelpaar__ knoop, en sparen gedownloade `config.zip` aan een veilige plaats voor [ later gebruik ](#private-key)
      + Tik __daarna__
      + Selecteer de het profiel van het Product __Integraties - Cloud Service__ en ontvang __gevormde API__
   + __de Diensten van Adobe > I/O Gebeurtenissen__ en tikken __sparen gevormde API__
   + __de Diensten van Adobe > I/O Beheer API__ en tikken __sparen gevormde API__

## De toets private.key openen{#private-key}

Toen vestiging werd de [ Asset compute API integratie ](#set-up) een nieuw zeer belangrijk paar geproduceerd en een `config.zip` dossier werd automatisch gedownload. Dit `config.zip` bevat het gegenereerde openbare certificaat en het overeenkomende `private.key` -bestand.

1. Unzip `config.zip` aan een veilige plaats op uw dossiersysteem aangezien `private.key` [ later ](../develop/environment-variables.md) wordt gebruikt
   + Geheimen en persoonlijke sleutels mogen nooit als een kwestie van veiligheid aan Git worden toegevoegd.

## Controleer de referenties van de serviceaccount (JWT)

De geloofsbrieven van dit Adobe I/O project worden gebruikt door het lokale [ Hulpmiddel van de Ontwikkeling van de Asset compute ](../develop/development-tool.md) om met Adobe I/O Runtime in wisselwerking te staan, en zullen in het project van de Asset compute moeten worden opgenomen. Verken uzelf met de JWT-referenties (Service Account).

![ de geloofsbrieven van de Rekening van de Dienst van Adobe Developer ](./assets/app-builder/service-account.png)

1. Controleer in het Adobe I/O Project App Builder-project of de `Development` -werkruimte is geselecteerd
1. Tik op __Rekening van de Dienst (JWT)__ onder __Referenties__
1. De weergegeven Adobe I/O Credentials controleren
   + De __openbare sleutel__ die bij de bodem wordt vermeld heeft het __private.key__ tegendeel in `config.zip` gedownload toen __Asset compute API__ aan dit project werd toegevoegd.
      + Als de persoonlijke sleutel verloren gaat of gecompromitteerd, kan de passende openbare sleutel worden verwijderd, en een nieuw zeer belangrijk paar dat binnen wordt geproduceerd of aan Adobe I/O wordt geupload gebruikend deze interface.
