---
title: De omgevingsvariabelen configureren voor uitbreidbaarheid van de Asset compute
description: Omgevingsvariabelen worden in het .env-bestand bewaard voor lokale ontwikkeling en worden gebruikt om Adobe I/O-referenties en gegevens voor cloudopslag te verstrekken die vereist zijn voor lokale ontwikkeling.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---


# Omgevingsvariabelen configureren

![puntenv-bestand](assets/environment-variables/dot-env-file.png)

Voordat u begint met de ontwikkeling van workers in de Asset compute, moet u controleren of het project is geconfigureerd met Adobe I/O en informatie over cloudopslag. Deze informatie wordt opgeslagen in `.env` van het project dat slechts voor lokale ontwikkeling, en niet sparen in Git wordt gebruikt. Het `.env` dossier verstrekt een geschikte manier om sleutel/waardeparen aan de lokale ontwikkelomgeving van de Asset compute bloot te stellen. Wanneer [implementating](../deploy/runtime.md) Asset compute workers to Adobe I/O Runtime, wordt het `.env` bestand niet gebruikt, maar wordt een subset van waarden doorgegeven via omgevingsvariabelen. Andere aangepaste parameters en geheimen kunnen ook in het `.env`-bestand worden opgeslagen, zoals ontwikkelingsgegevens voor externe webservices.

## Verwijs naar `private.key`

![persoonlijke sleutel](assets/environment-variables/private-key.png)

Open het `.env` dossier, uncomment de `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` sleutel, en verstrek de absolute weg op uw filesystem aan `private.key` die paren met het openbare certificaat aan uw project Adobe I/O FireFly wordt toegevoegd.

+ Als uw sleutelpaar door Adobe I/O werd geproduceerd, werd het auto-gedownload als deel van `config.zip`.
+ Als je Adobe I/O de openbare sleutel hebt gegeven, dan zou je ook in het bezit moeten zijn van de overeenkomende persoonlijke sleutel.
+ Als u deze sleutelparen niet hebt, kunt u nieuwe zeer belangrijke paren produceren of nieuwe openbare sleutels bij de bodem van uploaden:
   [https://console.adobe.com](https://console.adobe.io) > Uw Asset compute werkt probleemloos > Workspaces @ Development > Service Account (JWT).

Herinner het `private.key` dossier niet in Git zou moeten worden gecontroleerd aangezien het geheimen bevat, eerder het op een veilige plaats buiten het project zou moeten worden opgeslagen.

Op MacOS ziet dit er bijvoorbeeld als volgt uit:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Inloggegevens voor cloudopslag configureren

Voor lokale ontwikkeling van medewerkers van Asset computen is toegang tot [cloudopslag](../set-up/accounts-and-services.md#cloud-storage) vereist. De referenties voor cloudopslag die worden gebruikt voor lokale ontwikkeling, staan in het bestand `.env`.

Deze zelfstudie geeft de voorkeur aan het gebruik van Azure Blob Storage, maar Amazon S3 en de bijbehorende sleutels in het `.env`-bestand kunnen in plaats daarvan worden gebruikt.

### Azure Blob-opslag gebruiken

Verwijder de commentaarmarkering en vul de volgende sleutels in het `.env` dossier, en bevolk hen met de waarden voor de geleverde wolkenopslag die op Azure Portal wordt gevonden.

![Azure Blob Storage](./assets/environment-variables/azure-portal-credentials.png)

1. Waarde voor de `AZURE_STORAGE_CONTAINER_NAME`-toets
1. Waarde voor de `AZURE_STORAGE_ACCOUNT`-toets
1. Waarde voor de `AZURE_STORAGE_KEY`-toets

Dit ziet er bijvoorbeeld als volgt uit (alleen voor illustraties):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Het resulterende `.env`-bestand ziet er als volgt uit:

![Azure Blob Storage-referenties](assets/environment-variables/cloud-storage-credentials.png)

Als u GEEN Microsoft Azure Blob Storage gebruikt, verwijdert of laat u deze opmerkingen weg (door ze vooraf in te stellen met `#`).

### Amazon S3-cloudopslag gebruiken{#amazon-s3}

Als u Amazon S3 wolkenopslag uncomment gebruikt en de volgende sleutels in het `.env` dossier vult.

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

Zodra het geproduceerde project van de Asset compute is gevormd, bevestig de configuratie voorafgaand aan het aanbrengen van codeveranderingen om ervoor te zorgen de ondersteunende diensten, in de `.env` dossiers worden geleverd.

Om het Hulpmiddel van de Ontwikkeling van de Asset compute voor het project van de Asset compute te beginnen:

1. Open een bevellijn in de het projectwortel van de Asset compute (in de Code van VS kan dit direct in winde via Terminal > Nieuwe Terminal) worden geopend, en voer het bevel uit:

   ```
   $ aio app run
   ```

1. Het lokale Hulpmiddel van de Ontwikkeling van de Asset compute zal in uw standaardbrowser van het Web op __http://localhost:9000__ openen.

   ![AIR-app uitgevoerd](assets/environment-variables/aio-app-run.png)

1. Bekijk de uitvoer van de opdrachtregel en de webbrowser voor foutberichten terwijl het hulpprogramma Ontwikkeling wordt geïnitialiseerd.
1. Tik `Ctrl-C` in het venster dat `aio app run` heeft uitgevoerd om het proces te beëindigen om het Asset compute Development Tool te stoppen.

## Problemen oplossen

+ [Development Tool kan niet worden gestart omdat private.key ontbreekt](../troubleshooting.md#missing-private-key)
