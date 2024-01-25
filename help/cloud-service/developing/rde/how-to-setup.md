---
title: De snelle ontwikkelomgeving instellen
description: Leer hoe u Rapid Development Environment instelt voor AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 505
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# De snelle ontwikkelomgeving instellen

Meer informatie **instellen** Rapid Development Environment (RDE) in AEM as a Cloud Service.

In deze video wordt getoond:

- Een RDE toevoegen aan uw programma met gebruik van Cloud Manager
- RDE-aanmeldstroom met Adobe IMS, hoe deze vergelijkbaar is met andere AEM as a Cloud Service omgeving
- Instellen van [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) ook bekend als `aio CLI`
- Opstelling en configuratie van AEM RDE en de Manager van de Wolk `aio CLI` insteekmodule

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Vereiste

Het volgende moet lokaal worden geïnstalleerd:

- [Node.js](https://nodejs.org/en/) (LTS - langdurige ondersteuning)
- [npm 8+](https://docs.npmjs.com/)

## Lokale instellingen

Om het [WKND-siteprojecten](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) de code en de inhoud op RDE van uw lokale machine, voltooi de volgende stappen.

### Adobe I/O Runtime Extensible CLI

Installeer de Extensible CLI van Adobe I/O Runtime, ook bekend als de `aio CLI` door het volgende bevel van de bevellijn in werking te stellen.

```shell
$ npm install -g @adobe/aio-cli
```

### AEM plug-ins

Installeer Cloud Manager en AEM RDE-plug-ins met de `aio cli`s `plugins:install` gebruiken.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

Met de insteekmodule Cloud Manager kunnen ontwikkelaars via de opdrachtregel communiceren met Cloud Manager.

Met de AEM RDE-insteekmodule kunnen ontwikkelaars code en inhoud van de lokale computer implementeren.

Als u de plug-ins wilt bijwerken, gebruikt u de `aio plugins:update` gebruiken.

## AEM insteekmodules configureren

De AEM insteekmodules moeten worden gevormd om met uw RDE in wisselwerking te staan. Kopieer eerst met de interface van Cloud Manager de waarden van de organisatie-, programma- en milieu-id.

1. Organisatie-id: de waarde kopiëren van **Profielbeeld > Accountinformatie (intern) > Modal Window > Current Org ID**

   ![Organisatie-id](./assets/Org-ID.png)

1. Programma-id: de waarde kopiëren uit **Overzicht van programma > Omgevingen > {ProgramName}-rde > Browser URI > numbers between `program/` en`/environment`**

1. Omgeving-id: de waarde kopiëren uit **Overzicht van programma > Omgevingen > {ProgramName}-rde > Browser URI > numbers after`environment/`**

   ![Programma- en milieu-id](./assets/Program-Environment-Id.png)

1. Vervolgens gebruikt u de `aio cli`s `config:set` deze waarden in te stellen met de volgende opdracht.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

U kunt de huidige configuratiewaarden verifiëren door het volgende bevel in werking te stellen.

```shell
$ aio config:list
```

Ook, om te schakelen of te weten welke organisatie u momenteel het programma wordt geopend aan, kunt u het hieronder bevel gebruiken.

```shell
$ aio where
```

## RDE-toegang verifiëren

Verifieer de de insteekinstallatie en configuratie van AEMRDE door het volgende bevel in werking te stellen.

```shell
$ aio aem:rde:status
```

De RDE statusinformatie wordt weergegeven als een omgevingsstatus, de lijst met _uw AEM_ bundels en configuraties op auteur en publicatieservice.

## Volgende stap

Meer informatie [gebruiken](./how-to-use.md) een RDE om code en inhoud van uw favoriete Geïntegreerde Milieu van de Ontwikkeling (winde) voor snellere ontwikkelingscycli op te stellen.


## Aanvullende bronnen

[RDE inschakelen in een programmadocumentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Instellen van [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) ook bekend als `aio CLI`

[AIO CLI-gebruik en -opdrachten](https://github.com/adobe/aio-cli#usage)

[Adobe I/O Runtime CLI-insteekmodule voor interactie met AEM Rapid Development Environment](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIO CLI-insteekmodule](https://github.com/adobe/aio-cli-plugin-cloudmanager)
