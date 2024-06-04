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
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: f714adaa9bb637c0c7b17837c1d4b9f2be737c5c
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 0%

---

# De snelle ontwikkelomgeving instellen

Meer informatie **instellen** Rapid Development Environment (RDE) in AEM as a Cloud Service.

In deze video wordt getoond:

- Een RDE toevoegen aan uw programma met gebruik van Cloud Manager
- RDE-aanmeldstroom met Adobe IMS, hoe deze vergelijkbaar is met andere AEM as a Cloud Service omgeving
- Instellen van [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) ook bekend als `aio CLI`
- Opstelling en configuratie van AEM RDE en de Manager van de Wolk `aio CLI` met een niet-interactieve modus. Voor interactieve modus raadpleegt u de [installatie-instructies](#setup-the-aem-rde-plugin)

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

### CLI-plug-ins installeren en instellen

De AIR CLI moet geïnstalleerde insteekmodules en opstelling met de identiteitskaart van de Organisatie, van het Programma, en van het Milieu van RDE hebben om met uw RDE in wisselwerking te staan. Setup kan worden uitgevoerd via de AIR CLI met behulp van de eenvoudigere interactieve modus of de niet-interactieve modus.

>[!BEGINTABS]

>[!TAB Interactieve modus]

De AEM RDE-plug-ins installeren en instellen met de `aio cli`s `plugins:install` gebruiken.

1. Installeer de AEM RDE-plug-in van de AIR CLI met behulp van de `aio cli`s `plugins:install` gebruiken.

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   Met de AEM RDE-insteekmodule kunnen ontwikkelaars code en inhoud van de lokale computer implementeren.

2. Login aan de Verlengbare CLI van Adobe I/O Runtime door het volgende bevel in werking te stellen om het toegangstoken te krijgen. Meld u aan bij dezelfde Adobe als uw Cloud Manager.

   ```shell
   $ aio login
   ```

3. Voer de volgende opdracht uit om RDE in te stellen in de interactieve modus.

   ```shell
   $ aio aem:rde:setup
   ```

4. CLI zet u ertoe aan om identiteitskaart van de Organisatie, identiteitskaart van het Programma, en Milieu identiteitskaart in te gaan

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - Kies __Nee__  als u slechts met één enkele RDE werkt, en uw configuratie van RDE globaal op uw lokale machine wilt opslaan.

   - Kies __Ja__ als u met veelvoudige RDEs werkt, of uw configuratie van RDE plaatselijk, in de huidige omslag wilt opslaan `.aio` bestand, voor elk project.

5. Selecteer de organisatie-id, de programma-id en de RDE-omgeving-id in de lijst met beschikbare opties.

6. Verifieer de correcte Organisatie, het Programma en het Milieu opstelling door het volgende bevel in werking te stellen zijn.

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB Niet-interactieve modus]

Installeer en stel de insteekmodules Cloud Manager en AEM RDE in via de `aio cli`s `plugins:install` gebruiken.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

Met de insteekmodule Cloud Manager kunnen ontwikkelaars via de opdrachtregel communiceren met Cloud Manager.

Met de AEM RDE-insteekmodule kunnen ontwikkelaars code en inhoud van de lokale computer implementeren.

De insteekmodules van airCLI moeten worden gevormd om met uw RDE in wisselwerking te staan.

1. Kopieer eerst met de Cloud Manager de waarden van de organisatie-, programma- en milieu-id.

   - Organisatie-id: de waarde kopiëren van **Profielbeeld > Accountinformatie (intern) > Modal Window > Current Org ID**

   ![Organisatie-id](./assets/Org-ID.png)

   - Programma-id: de waarde kopiëren uit **Overzicht van programma > Omgevingen > {ProgramName}-rde > Browser URI > numbers between `program/` en`/environment`**

   ![Programma- en milieu-id](./assets/Program-Environment-Id.png)

   - Omgeving-id: de waarde kopiëren uit **Overzicht van programma > Omgevingen > {ProgramName}-rde > Browser URI > numbers after`environment/`**

   ![Programma- en milieu-id](./assets/Program-Environment-Id.png)

1. Gebruik de `aio cli`s `config:set` deze waarden in te stellen met de volgende opdracht.

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. Verifieer de huidige config waarden door het volgende bevel in werking te stellen.

   ```shell
   $ aio config:list
   ```

1. Van de schakelaar of overzicht welke Organisatie u momenteel het programma wordt geopend aan:

   ```shell
   $ aio where
   ```

>[!ENDTABS]

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

[Gebruik en opdrachten van AIR CLI](https://github.com/adobe/aio-cli#usage)

[Adobe I/O Runtime CLI-insteekmodule voor interactie met AEM Rapid Development Environment](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIR CLI-plug-in](https://github.com/adobe/aio-cli-plugin-cloudmanager)
