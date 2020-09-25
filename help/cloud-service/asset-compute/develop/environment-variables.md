---
title: Omgevingsvariabelen configureren voor de rekbaarheid van Asset Compute
description: Omgevingsvariabelen worden in het .env-bestand bewaard voor lokale ontwikkeling en worden gebruikt om Adobe I/O-gegevens en gegevens voor cloudopslag te verstrekken die vereist zijn voor lokale ontwikkeling.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 0%

---


# Omgevingsvariabelen configureren

![puntenv-bestand](assets/environment-variables/dot-env-file.png)

Voordat u begint met de ontwikkeling van Asset Compute-workers, moet u controleren of het project is geconfigureerd met Adobe I/O- en cloudopslaggegevens. Deze informatie wordt opgeslagen in het project `.env` dat slechts voor lokale ontwikkeling, en niet sparen in Git wordt gebruikt. Het `.env` bestand biedt een handige manier om sleutel-/waardeparen beschikbaar te maken voor de lokale ontwikkelomgeving voor het berekenen van bedrijfsmiddelen. Bij de [implementatie](../deploy/runtime.md) van Asset Compute-workers in Adobe I/O Runtime wordt het `.env` bestand niet gebruikt, maar wordt een subset van waarden doorgegeven via omgevingsvariabelen. Andere aangepaste parameters en geheimen kunnen ook in het `.env` bestand worden opgeslagen, zoals ontwikkelingsgegevens voor externe webservices.

## Verwijs naar de `private.key`

![persoonlijke sleutel](assets/environment-variables/private-key.png)

Open het `.env` dossier, uncomment de `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` sleutel, en verstrek de absolute weg op uw filesystem aan `private.key` die paren met het openbare certificaat aan uw Adobe I/O Vuurvliegproject wordt toegevoegd.

+ Als uw sleutelpaar door Adobe I/O werd geproduceerd, werd het auto-gedownload als deel van `config.zip`.
+ Als u de openbare sleutel aan Adobe I/O verstrekte, dan zou u ook in het bezit van de passende privé sleutel moeten zijn.
+ Als u deze sleutelparen niet hebt, kunt u nieuwe zeer belangrijke paren produceren of nieuwe openbare sleutels bij de bodem van uploaden:
   [https://console.adobe.com](https://console.adobe.io) > Your Asset Compute Firefly Project > Workspaces @ Development > Service Account (JWT).

Herinner het `private.key` dossier niet in Git zou moeten worden gecontroleerd aangezien het geheimen bevat, eerder het zou op een veilige plaats buiten het project moeten worden opgeslagen.

Op MacOS ziet dit er bijvoorbeeld als volgt uit:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Inloggegevens voor cloudopslag configureren

Voor de lokale ontwikkeling van medewerkers van Asset Compute is toegang tot [cloudopslag](../set-up/accounts-and-services.md#cloud-storage)vereist. De gegevens voor cloudopslag die worden gebruikt voor lokale ontwikkeling, staan in het `.env` bestand.

Deze zelfstudie geeft de voorkeur aan het gebruik van Azure Blob Storage, maar Amazon S3 en de overeenkomstige sleutels in het `.env` bestand kunnen in plaats daarvan worden gebruikt.

### Azure Blob-opslag gebruiken

Verwijder de commentaarmarkering en vul de volgende sleutels in het `.env` bestand en vul deze met de waarden voor de geleverde cloudopslag op Azure Portal.

![Azure Blob Storage](./assets/environment-variables/azure-portal-credentials.png)

1. Waarde voor de `AZURE_STORAGE_CONTAINER_NAME` toets
1. Waarde voor de `AZURE_STORAGE_ACCOUNT` toets
1. Waarde voor de `AZURE_STORAGE_KEY` toets

Dit ziet er bijvoorbeeld als volgt uit (alleen voor illustraties):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Het resulterende `.env` bestand ziet er als volgt uit:

![Azure Blob Storage-referenties](assets/environment-variables/cloud-storage-credentials.png)

Als u GEEN Microsoft Azure Blob Storage gebruikt, verwijdert of laat u deze opmerkingen weg (door ze vooraf te bevestigen met `#`).

### Amazon S3-cloudopslag gebruiken{#amazon-s3}

Als u Amazon S3 cloud storage uncomment en de volgende sleutels in het `.env` dossier vult.

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

Nadat het gegenereerde project Asset Compute is geconfigureerd, valideert u de configuratie voordat u codewijzigingen aanbrengt om ervoor te zorgen dat de ondersteunende services in de `.env` bestanden zijn ingericht.

Om het Hulpmiddel van de Ontwikkeling van de Verwerking van Activa voor het project van de Verwerking van Activa te beginnen:

1. Open een bevellijn in het Activum Compute projectwortel (in de Code van VS kan dit direct in winde via Terminal > Nieuwe Terminal) worden geopend, en voer het bevel uit:

   ```
   $ aio app run
   ```

1. Het lokale Hulpmiddel van de Ontwikkeling van het Vermogen van Activa zal in uw standaardbrowser van het Web op __http://localhost:9000__ openen.

   ![AIR-app uitgevoerd](assets/environment-variables/aio-app-run.png)

1. Bekijk de uitvoer van de opdrachtregel en de webbrowser voor foutberichten terwijl het hulpprogramma Ontwikkeling wordt geïnitialiseerd.
1. Tik in het venster dat is uitgevoerd om het proces te beëindigen `Ctrl-C` `aio app run` om het Asset Compute Development Tool te stoppen.

## Problemen oplossen

### De tools voor lokale ontwikkeling van Asset Compute kunnen niet worden gestart omdat private.key ontbreekt

+ __Fout:__ Lokale Dev ServerError: Vereiste bestanden ontbreken bij validatePrivateKeyFile.... (via standaard uit van `aio app run` opdracht)
+ __Oorzaak:__ De `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in het `.env` bestand verwijst niet naar `private.key` of `private.key` is niet leesbaar voor de huidige gebruiker.
+ __Resolutie:__ Controleer de `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` waarde in het `.env` bestand en controleer of het volledige, absolute pad naar het bestand `private.key` op het bestandssysteem is opgenomen.
