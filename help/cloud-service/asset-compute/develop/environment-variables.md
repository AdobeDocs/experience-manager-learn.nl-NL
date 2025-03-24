---
title: Omgevingsvariabelen configureren voor Asset Compute-uitbreidbaarheid
description: Omgevingsvariabelen worden in het .env-bestand bewaard voor lokale ontwikkeling en worden gebruikt om Adobe I/O-referenties en gegevens voor cloudopslag te verstrekken die vereist zijn voor lokale ontwikkeling.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 121
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Omgevingsvariabelen configureren

![ dot env- dossier ](assets/environment-variables/dot-env-file.png)

Voordat u begint met de ontwikkeling van Asset Compute-workers, moet u controleren of het project is geconfigureerd met Adobe I/O en informatie over cloudopslag. Deze informatie wordt opgeslagen in het project `.env` dat alleen wordt gebruikt voor lokale ontwikkeling en niet in Git. Het bestand `.env` biedt een handige manier om sleutel-/waardeparen beschikbaar te maken voor de lokale Asset Compute-ontwikkelomgeving. Wanneer [ opstellend ](../deploy/runtime.md) de arbeiders van Asset Compute aan Adobe I/O Runtime, wordt het `.env` dossier niet gebruikt, maar eerder wordt een ondergroep van waarden overgegaan binnen via omgevingsvariabelen. Andere aangepaste parameters en geheimen kunnen ook in het `.env` -bestand worden opgeslagen, zoals ontwikkelingsgegevens voor externe webservices.

## Verwijzen naar de `private.key`

![ privé sleutel ](assets/environment-variables/private-key.png)

Open het `.env` -bestand, verwijder de commentaarmarkering voor de `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` -toets en geef het absolute pad op uw bestandssysteem op naar het `private.key` -bestand dat het openbare certificaat bevat dat aan uw Adobe I/O App Builder-project is toegevoegd.

+ Als uw sleutelpaar door Adobe I/O werd geproduceerd, werd het auto-gedownload als deel van `config.zip`.
+ Als je Adobe I/O de openbare sleutel hebt gegeven, dan zou je ook in het bezit moeten zijn van de overeenkomende persoonlijke sleutel.
+ Als u deze sleutelparen niet hebt, kunt u nieuwe zeer belangrijke paren produceren of nieuwe openbare sleutels bij de bodem van uploaden:
  [ https://console.adobe.com ](https://console.adobe.io) > Uw project van Asset Compute App Builder > Werkruimten @ Ontwikkeling > de Rekening van de Dienst (JWT).

Herinner het `private.key` dossier niet in Git zou moeten worden gecontroleerd aangezien het geheimen bevat, eerder het op een veilige plaats buiten het project zou moeten worden opgeslagen.

Op macOS ziet dit er bijvoorbeeld als volgt uit:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Inloggegevens voor cloudopslag configureren

De lokale ontwikkeling van de arbeiders van Asset Compute vereist toegang tot [ wolkenopslag ](../set-up/accounts-and-services.md#cloud-storage). De referenties voor cloudopslag die worden gebruikt voor lokale ontwikkeling, staan in het `.env` -bestand.

Deze zelfstudie geeft de voorkeur aan het gebruik van Azure Blob Storage, maar Amazon S3 en de bijbehorende sleutels in het `.env` -bestand kunnen in plaats daarvan worden gebruikt.

### Azure Blob-opslag gebruiken

Verwijder de commentaarmarkering en vul de volgende sleutels in het `.env` -bestand en vul deze met de waarden voor de geleverde cloudopslag op Azure Portal.

![ Azure Blob Storage ](./assets/environment-variables/azure-portal-credentials.png)

1. Waarde voor de `AZURE_STORAGE_CONTAINER_NAME` -toets
1. Waarde voor de `AZURE_STORAGE_ACCOUNT` -toets
1. Waarde voor de `AZURE_STORAGE_KEY` -toets

Dit ziet er bijvoorbeeld als volgt uit (alleen voor illustraties):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Het resulterende `.env` -bestand ziet er als volgt uit:

![ Azure Blob Storage geloofsbrieven ](assets/environment-variables/cloud-storage-credentials.png)

Als u NIET gebruikmaakt van Microsoft Azure Blob Storage, verwijdert of laat u deze opmerkingen achter (door ze vooraf in te stellen met `#` ).

### Amazon S3-cloudopslag gebruiken{#amazon-s3}

Als u Amazon S3 cloud storage uncomment en de volgende toetsen vult in het `.env` bestand.

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

Nadat het gegenereerde Asset Compute-project is geconfigureerd, valideert u de configuratie voordat u codewijzigingen aanbrengt om ervoor te zorgen dat de ondersteunende services worden geleverd in de `.env` -bestanden.

Asset Compute Development Tool starten voor het Asset Compute-project:

1. Open een bevellijn in de het projectwortel van Asset Compute (in VS Code kan dit direct in winde via Terminal > Nieuwe Terminal worden geopend), en voer het bevel uit:

   ```
   $ aio app run
   ```

1. Het lokale Hulpmiddel van de Ontwikkeling van Asset Compute zal in uw standaardbrowser van het Web in __http://localhost:9000__ openen.

   ![ de looppas van de audio app ](assets/environment-variables/aio-app-run.png)

1. Bekijk de uitvoer van de opdrachtregel en de webbrowser voor foutberichten terwijl het hulpprogramma Ontwikkeling wordt geïnitialiseerd.
1. Tik op `Ctrl-C` in het venster dat `aio app run` heeft uitgevoerd om het Asset Compute Development Tool te stoppen.

## Problemen oplossen

+ [Development Tool kan niet worden gestart omdat private.key ontbreekt](../troubleshooting.md#missing-private-key)
