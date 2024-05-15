---
title: App Builder instellen voor uitbreidbaarheid van Asset compute
description: De projecten van de asset compute zijn speciaal bepaalde projecten App Builder, en als dusdanig, vereisen toegang tot App Builder in de Console van Adobe Developer om hen te opstelling en op te stellen.
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

De projecten van de asset compute zijn speciaal bepaalde projecten App Builder, en als dusdanig, vereisen toegang tot App Builder in de Console van Adobe Developer om hen te opstelling en op te stellen.

## App Builder maken en instellen in Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Doorklikken bij instellen van App Builder (geen audio)_

1. Aanmelden bij [Adobe Developer Console](https://console.adobe.io) de Adobe ID gebruiken die is gekoppeld aan de provisioning [rekeningen en diensten](./accounts-and-services.md). Zorg ervoor dat u een __Systeembeheerder__ of in de __Rol van ontwikkelaar__ voor de juiste Adobe Org.
1. Creeer een project van de Bouwer van de App door te tikken __Nieuw project maken > Project van sjabloon > App Builder__

   _Indien__ Nieuw project maken __of de__ App Builder __type niet beschikbaar is, dit betekent dat uw Adobe Org niet [geleverd met App Builder](#request-adobe-project-app-builder)._

   + __Projecttitel__: `WKND AEM Asset Compute`
   + __Toepassingsnaam__: `wkndAemAssetCompute<YourName>`
      + De __Toepassingsnaam__ moet uniek zijn voor alle projecten van FApp Buildregulier en kan later niet worden gewijzigd. Het voorschrijven van de naam van uw bedrijf of organisatie en het achteraf plaatsen met een zinvol achtervoegsel is een goede aanpak, zoals: `wkndAemAssetCompute`.
      + Voor zelfactivering is het vaak het beste om uw naam naar de __Toepassingsnaam__, zoals `wkndAemAssetComputeJaneDoe` om botsingen met andere projecten te vermijden App Builder.
   + Onder __Werkruimten__ een nieuwe omgeving toevoegen met de naam `Development`
   + Onder __Adobe I/O Runtime__ waarborgen __Runtime opnemen bij elke werkruimte__ is geselecteerd
   + Tikken __Opslaan__ om het project op te slaan
1. In het project App Builder selecteert u `Development` vanuit de werkruimteselector
1. Tikken __+ Service toevoegen > API__ om de __Een API toevoegen__ Deze methode gebruiken om de volgende API&#39;s toe te voegen:

   + __EXPERIENCE CLOUD > ASSET COMPUTE__
      + Selecteren __Een sleutelpaar genereren__ en tik op de __Keypair genereren__ en slaat het gedownloade `config.zip` naar een veilige locatie voor [later gebruiken](#private-key)
      + Tikken __Volgende__
      + Selecteer het productprofiel __Integratie - Cloud Service__ en tikken __geconfigureerde API opslaan__
   + __Adobe Services > I/O-gebeurtenissen__ en tikken __geconfigureerde API opslaan__
   + __Adobe Services > I/O Management API__ en tikken __geconfigureerde API opslaan__

## De toets private.key openen{#private-key}

Wanneer u de [Asset compute API-integratie](#set-up) een nieuw sleutelpaar werd geproduceerd en `config.zip` bestand is automatisch gedownload. Dit `config.zip` bevat het gegenereerde openbare certificaat en de bijbehorende overeenkomst `private.key` bestand.

1. Unzip `config.zip` op een veilige plaats in uw bestandssysteem als de `private.key` is [later gebruikt](../develop/environment-variables.md)
   + Geheimen en persoonlijke sleutels mogen nooit als een kwestie van veiligheid aan Git worden toegevoegd.

## Controleer de referenties van de serviceaccount (JWT)

De geloofsbrieven van dit Adobe I/O-project worden gebruikt door de lokale [Asset compute ontwikkelen](../develop/development-tool.md) om met Adobe I/O Runtime te kunnen communiceren en in het project van de Asset compute moet worden opgenomen. Verken uzelf met de JWT-referenties (Service Account).

![Referenties Adobe Developer-serviceaccount](./assets/app-builder/service-account.png)

1. Van het project van App Builder van het Project van Adobe I/O, zorg ervoor `Development` werkruimte is geselecteerd
1. Tikken __Serviceaccount (JWT)__ krachtens __Credentials__
1. De weergegeven Adobe I/O Credentials controleren
   + De __openbare sleutel__ onderaan vermeld: __private.key__ tegenhanger van de `config.zip` gedownload bij de __Asset compute-API__ is toegevoegd aan dit project.
      + Als de persoonlijke sleutel verloren gaat of gecompromitteerd, kan de passende openbare sleutel worden verwijderd, en een nieuw zeer belangrijk paar dat binnen wordt geproduceerd of aan Adobe I/O wordt geupload gebruikend deze interface.
