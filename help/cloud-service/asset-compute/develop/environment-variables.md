---
title: De omgevingsvariabelen configureren voor uitbreidbaarheid van de Asset compute
description: Omgevingsvariabelen worden in het .env-bestand bewaard voor lokale ontwikkeling en worden gebruikt om gegevens over Adobe I/O en cloudopslag te verstrekken die vereist zijn voor lokale ontwikkeling.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 144
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Omgevingsvariabelen configureren

![puntenv-bestand](assets/environment-variables/dot-env-file.png)

Voordat u begint met de ontwikkeling van workers in de Asset compute, moet u ervoor zorgen dat het project is geconfigureerd met informatie over Adobe I/O- en cloudopslag. Deze informatie wordt opgeslagen in de `.env`  die alleen wordt gebruikt voor lokale ontwikkeling en niet in Git. De `.env` bestand biedt een handige manier om sleutel-/waardeparen toegankelijk te maken voor de lokale ontwikkelomgeving van de lokale Asset compute. Wanneer [implementeren](../deploy/runtime.md) Asset compute werknemers naar Adobe I/O Runtime, `.env` Het bestand wordt niet gebruikt, maar er wordt een subset van waarden doorgegeven via omgevingsvariabelen. Andere aangepaste parameters en geheimen kunnen worden opgeslagen in het dialoogvenster `.env` en andere bestanden, zoals ontwikkelingsgegevens voor webservices van derden.

## Verwijs naar de `private.key`

![persoonlijke sleutel](assets/environment-variables/private-key.png)

Open de `.env` bestand, verwijder de commentaarmarkering voor `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` en het absolute pad op uw bestandssysteem naar de `private.key` die paren met het openbare certificaat dat aan uw Adobe I/O App Builder-project wordt toegevoegd.

+ Als uw sleutelpaar door Adobe I/O werd geproduceerd, werd het auto-gedownload als deel van  `config.zip`.
+ Als u de openbare sleutel aan Adobe I/O verstrekte, dan zou u ook in het bezit van de passende privé sleutel moeten zijn.
+ Als u deze sleutelparen niet hebt, kunt u nieuwe zeer belangrijke paren produceren of nieuwe openbare sleutels bij de bodem van uploaden:
  [https://console.adobe.com](https://console.adobe.io) > Uw Asset compute App Builder-project > Werkruimten @ Development > Service Account (JWT).

De `private.key` Het bestand moet niet in Git worden gecontroleerd omdat het geheimen bevat, maar moet op een veilige plaats buiten het project worden opgeslagen.

Op macOS ziet dit er bijvoorbeeld als volgt uit:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Inloggegevens voor cloudopslag configureren

Voor lokale ontwikkeling van Asset compute werknemers is toegang tot [cloudopslag](../set-up/accounts-and-services.md#cloud-storage). De referenties voor cloudopslag die worden gebruikt voor lokale ontwikkeling zijn te vinden in het dialoogvenster `.env` bestand.

Deze zelfstudie geeft de voorkeur aan het gebruik van Azure Blob Storage, maar Amazon S3 en de bijbehorende sleutels in de `.env` kan in plaats daarvan worden gebruikt.

### Azure Blob-opslag gebruiken

Verwijder de commentaarmarkering en vul de volgende toetsen in in het dialoogvenster `.env` en vult deze met de waarden voor de geleverde cloudopslag op Azure Portal.

![Azure Blob Storage](./assets/environment-variables/azure-portal-credentials.png)

1. Waarde voor de `AZURE_STORAGE_CONTAINER_NAME` key
1. Waarde voor de `AZURE_STORAGE_ACCOUNT` key
1. Waarde voor de `AZURE_STORAGE_KEY` key

Dit ziet er bijvoorbeeld als volgt uit (alleen voor illustraties):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Het resultaat `.env` Het bestand ziet er als volgt uit:

![Azure Blob Storage-referenties](assets/environment-variables/cloud-storage-credentials.png)

Als u NIET gebruikmaakt van Microsoft Azure Blob Storage, verwijdert u deze opmerkingen of laat u deze buiten beschouwing (door ze vooraf te bevestigen met `#`).

### Amazon S3-cloudopslag gebruiken{#amazon-s3}

Als u Amazon S3 cloud storage uncomment gebruikt en de volgende toetsen invult in de `.env` bestand.

Dit ziet er bijvoorbeeld als volgt uit (alleen voor illustraties):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## De projectconfiguratie valideren

Zodra het geproduceerde project van de Asset compute is gevormd, bevestig de configuratie voorafgaand aan het aanbrengen van codeveranderingen om te verzekeren de ondersteunende diensten worden provisioned, in `.env` bestanden.

Om het Hulpmiddel van de Ontwikkeling van de Asset compute voor het project van de Asset compute te beginnen:

1. Open een bevellijn in de het projectwortel van de Asset compute (in de Code van VS kan dit direct in winde via Terminal > Nieuwe Terminal) worden geopend, en voer het bevel uit:

   ```
   $ aio app run
   ```

1. Het lokale Hulpmiddel van de Ontwikkeling van de Asset compute zal in uw standaardbrowser van het Web bij openen __http://localhost:9000__.

   ![AIR-app uitgevoerd](assets/environment-variables/aio-app-run.png)

1. Bekijk de uitvoer van de opdrachtregel en de webbrowser voor foutberichten terwijl het hulpprogramma Ontwikkeling wordt geïnitialiseerd.
1. Tik op het gereedschap Asset compute ontwikkelen om het ontwikkelen te stoppen `Ctrl-C` in het venster dat wordt uitgevoerd `aio app run` om het proces te beëindigen.

## Problemen oplossen

+ [Development Tool kan niet worden gestart omdat private.key ontbreekt](../troubleshooting.md#missing-private-key)
