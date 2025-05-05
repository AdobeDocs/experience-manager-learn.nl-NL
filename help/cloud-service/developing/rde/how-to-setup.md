---
title: De snelle ontwikkelomgeving instellen
description: Leer hoe u Rapid Development Environment voor AEM as a Cloud Service instelt.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 0%

---

# De snelle ontwikkelomgeving instellen

Leer **hoe te opstelling** Snelle Milieu van de Ontwikkeling (RDE) in AEM as a Cloud Service.

In deze video wordt getoond:

- Een RDE toevoegen aan uw programma met Cloud Manager
- RDE-aanmeldstroom met Adobe IMS, hoe vergelijkbaar met andere AEM as a Cloud Service-omgevingen
- Opstelling van [ Extensible CLI van Adobe I/O Runtime ](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) ook gekend als `aio CLI`
- Instellen en configureren van AEM RDE- en Cloud Manager `aio CLI` -insteekmodule in de niet-interactieve modus. Voor interactieve wijze, zie de [ opstellingsinstructies ](#setup-the-aem-rde-plugin)

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Vereiste

Het volgende moet lokaal worden geïnstalleerd:

- [ Node.js ](https://nodejs.org/en/) (LTS - steun op lange termijn)
- [ npm 8+ ](https://docs.npmjs.com/)

## Lokale instellingen

Om de [&#128279;](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) code en inhoud van het Project van de Plaatsen van 0&rbrace; WKND &lbrace;op te stellen RDE van uw lokale machine, voltooi de volgende stappen.

### Adobe I/O Runtime Extensible CLI

Installeer de Extensible CLI van Adobe I/O Runtime, die ook als `aio CLI` wordt bekend door het volgende bevel van de bevellijn in werking te stellen.

```shell
$ npm install -g @adobe/aio-cli
```

### CLI-plug-ins installeren en instellen

De AIR CLI moet geïnstalleerde insteekmodules en opstelling met de identiteitskaart van de Organisatie, van het Programma, en van het Milieu van RDE hebben om met uw RDE in wisselwerking te staan. Setup kan worden uitgevoerd via de AIR CLI met behulp van de eenvoudigere interactieve modus of de niet-interactieve modus.

>[!BEGINTABS]

>[!TAB  Interactieve wijze ]

Installeer de AEM RDE-plug-ins en stel deze in met de opdracht `aio cli` s `plugins:install` .

1. Installeer de AEM RDE-plug-in van de AIR CLI met de opdracht `aio cli` &#39;&#39;s `plugins:install` .

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   Met de AEM RDE-insteekmodule kunnen ontwikkelaars code en inhoud van de lokale computer implementeren.

2. Login aan de Verlengbare CLI van Adobe I/O Runtime door het volgende bevel in werking te stellen om het toegangstoken te krijgen. Meld u aan bij dezelfde Adobe Org als uw Cloud Manager.

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

   - Kies __Nr__ als u slechts met één enkele RDE werkt, en uw configuratie van RDE globaal op uw lokale machine wilt opslaan.

   - Kies __ja__ als u met veelvoudige RDEs werkt, of uw configuratie van RDE, in het 2&rbrace; dossier van de huidige omslag &lbrace;, voor elk project lokaal wilt opslaan.`.aio`

5. Selecteer de organisatie-id, de programma-id en de RDE-omgeving-id in de lijst met beschikbare opties.

6. Verifieer de correcte Organisatie, het Programma en het Milieu opstelling door het volgende bevel in werking te stellen zijn.

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB  Niet-interactieve wijze ]

Installeer de Cloud Manager- en AEM RDE-plug-ins en stel deze in met de opdracht `aio cli` s `plugins:install` .

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

Met de Cloud Manager-insteekmodule kunnen ontwikkelaars via de opdrachtregel communiceren met Cloud Manager.

Met de AEM RDE-insteekmodule kunnen ontwikkelaars code en inhoud van de lokale computer implementeren.

De insteekmodules van airCLI moeten worden gevormd om met uw RDE in wisselwerking te staan.

1. Kopieer eerst met de Cloud Manager de waarden van de organisatie-, programma- en milieu-id.

   - Organisatie-id: Kopieer de waarde van **Beeld van het Profiel > (interne) Accountinfo > Modal Venster > Huidige organisatie-id**

   ![ identiteitskaart van de Organisatie ](./assets/Org-ID.png)

   - Programma-id: kopieer de waarde uit **Program Overview > Environments > {ProgramName} Rand > Browser URI > numbers between `program/` en`/environment`**

   ![ identiteitskaart van het Programma en van het Milieu ](./assets/Program-Environment-Id.png)

   - Omgeving-id: kopieer de waarde uit **Program Overview > Environment > {ProgramName} -rd > Browser URI > numbers after`environment/`**

   ![ identiteitskaart van het Programma en van het Milieu ](./assets/Program-Environment-Id.png)

1. Gebruik de opdracht `aio cli` `config:set` om deze waarden in te stellen met de volgende opdracht.

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

Controleer de installatie en configuratie van de AEM RDE-insteekmodule met de volgende opdracht.

```shell
$ aio aem:rde:status
```

De RDE statusinformatie wordt getoond als milieustatus, de lijst van _uw het projectbundels en configuraties van AEM_ op auteur en publiceer de dienst.

## Volgende stap

Leer [ hoe te om ](./how-to-use.md) RDE te gebruiken om code en inhoud van uw favoriete Geïntegreerde Milieu van de Ontwikkeling (winde) voor snellere ontwikkelingscycli op te stellen.


## Aanvullende bronnen

[ toelatend RDE in een programmadocumentatie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Opstelling van [ Extensible CLI van Adobe I/O Runtime ](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) ook gekend als `aio CLI`

[ het gebruik en bevelen van ATM CLI ](https://github.com/adobe/aio-cli#usage)

[ de stop van Adobe I/O Runtime CLI voor interactie met de Milieu&#39;s van de Snelle Ontwikkeling van AEM ](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[ de insteekmodule van Cloud Manager CLI ](https://github.com/adobe/aio-cli-plugin-cloudmanager)
