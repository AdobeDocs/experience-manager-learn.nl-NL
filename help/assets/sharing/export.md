---
title: Elementen exporteren
description: Leer hoe u elementen bulksgewijs kunt exporteren en downloaden naar uw lokale computer.
feature: Asset Management
version: Cloud Service
topic: Content Management
role: Developer
level: Experienced
last-substantial-update: 2024-04-08T00:00:00Z
doc-type: Tutorial
jira: KT-15313
thumbnail: KT-15313.jpeg
exl-id: d04c3316-6f8f-4fd1-9df1-3fe09d44f735
duration: 256
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# Elementen exporteren

Leer hoe u elementen naar uw lokale computer exporteert met een aanpasbaar Node.js-script. Dit de uitvoermanuscript verstrekt een voorbeeld van hoe te om activa van AEM programmatically te downloaden gebruikend [ HTTP APIs van AEM Assets ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), specifiek het concentreren op de originele vertoningen om de hoogste kwaliteit te verzekeren. Deze is ontworpen om de mapstructuur van AEM Assets op uw lokale station te repliceren, zodat u eenvoudig back-ups kunt maken van middelen of deze kunt migreren.

Het script downloadt alleen de oorspronkelijke uitvoeringen van het element, zonder de bijbehorende metagegevens, tenzij die metagegevens als XMP in het element zijn ingesloten. Dit betekent dat beschrijvende informatie, categorieën of tags die zijn opgeslagen in AEM maar niet zijn geïntegreerd in de elementbestanden, niet zijn opgenomen in de download. Andere vertoningen kunnen ook worden gedownload door het manuscript te wijzigen om hen te omvatten. Zorg ervoor dat u voldoende ruimte hebt om de geëxporteerde elementen op te slaan.

Het script wordt doorgaans uitgevoerd tegen AEM Auteur, maar kan ook worden uitgevoerd tegen AEM Publish, zolang de eindpunten van de HTTP-API van AEM Assets en elementenuitvoeringen toegankelijk zijn via Dispatcher.

Voordat u het script uitvoert, moet u het configureren met de URL van de AEM instantie, de gebruikersgegevens (toegangstoken) en het pad naar de map die u wilt exporteren.

## Script exporteren

Het script, geschreven als een JavaScript-module, maakt deel uit van een Node.js-project, omdat het afhankelijk is van `node-fetch` . U kunt [ het project als zip dossier ](./assets/export/export-aem-assets-script.zip) downloaden, of het manuscript hieronder in een leeg project Node.js van type `module` kopiëren, en `npm install node-fetch` in werking stellen om het gebiedsdeel te installeren.

Met dit script wordt de mappenstructuur van AEM Assets doorlopen en worden elementen en mappen naar een lokale map op uw computer gedownload. Het gebruikt [ HTTP API van AEM Assets ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) om de omslag en activagegevens te halen, en de originele vertoningen van de activa te downloaden.

```javascript
// export-assets.js

import fetch from 'node-fetch';
import { promises as fs } from 'fs';
import path from 'path';

// Do not process the contents of these well-known AEM system folders
const SKIP_FOLDERS = ['/content/dam/appdata', '/content/dam/projects', '/content/dam/_CSS', '/content/dam/_DMSAMPLE' ];

/**
 * Determine if the folder should be processed based on the entity and AEM path.
 * 
 * @param {Object} entity the AEM entity that should represent a folder returned from AEM Assets HTTP API
 * @param {String} aemPath the path in AEM of this source
 * @returns true if the entity should be processed, false otherwise
 */
function isValidFolder(entity, aemPath) {
    if (aemPath === '/content/dam') {
        // Always allow processing /content/dam 
        return true;
    } else if (!entity.class.includes('assets/folder')) {
        return false;
    } if (SKIP_FOLDERS.find((path) => path === aemPath)) {
        return false;
    } else if (entity.properties.hidden) {
        return false;
    }
    
    return true;
}

/**
 * Determine if the entity is downloadable.
 * @param {Object} entity the AEM entity that should represent an asset returned from AEM Assets HTTP API
 * @returns true if the entity is downloadable, false otherwise
 */
function isDownloadable(entity) {
    if (entity.class.includes('assets/folder')) {
        return false;
    } else if (entity.properties.contentFragment) {
        return false;
    }

    return true;
}

/**
 * Helper function to get the link from the entity based on the relationship name.
 * @param {Object} entity the entity from the AEM Assets HTTP API
 * @param {String} rel the relationship name
 * @returns 
 */
function getLink(entity, rel) {
    return entity.links.find(link => link.rel.includes(rel));
}

/**
 * Helper function to fetch JSON data from the AEM Assets HTTP API.
 * @param {String} url the AEM Assets HTTP API URL to fetch data from
 * @returns the JSON response of the AEM Assets HTTP API
 */
async function fetchJSON(url) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
            'Content-Type': 'application/json'
        }
    });

    if (!response.ok) {
        throw new Error(`Error: ${response.status}`);
    }

    return response.json();
}

/**
 * Helper function to download a file from AEM Assets.
 * @param {String} url the URL of the asset rendition to download
 * @param {String} outputPath the local path to save the downloaded file
 */
async function downloadFile(url, outputPath) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
        }
    });

    if (!response.ok) {
        throw new Error(`Failed to download file: ${response.statusText}`);
    }

    const arrayBuffer = await response.arrayBuffer();
    await fs.writeFile(outputPath, Buffer.from(arrayBuffer));

    console.log(`Downloaded asset: ${outputPath}`);
}

/**
 * Main entry
 * @param {Object} options the options for downloading assets
 * @param {String} options.folderUrl the URL of the AEM folder to download
 * @param {String} options.localPath the local path to save the downloaded assets
 * @param {String} options.aemPath the AEM path of the folder to download
 */
async function downloadAssets({apiUrl, localPath = LOCAL_DOWNLOAD_FOLDER, aemPath = '/content/dam'}) {    
    if (!apiUrl) {
        // Handle the initial call to the script, which should just provide the AEM path
        // Construct the proper AEM Assets HTTP API URL as it uses a truncated AEM path
        const prefix = "/content/dam/";
        let apiPath = aemPath.startsWith(prefix) ? aemPath.substring(prefix.length) : aemPath;    

        if (!apiPath.startsWith('/')) {
            apiPath = '/' + apiPath;
        }

        apiUrl = `${AEM_HOST}/api/assets.json${apiPath}`
    }
    
    const data = await fetchJSON(apiUrl);
    const entities = data.entities || [];

    // Process folders first
    for (const folder of entities.filter(entity => entity.class.includes('assets/folder'))) {
        const newLocalPath = path.join(localPath, folder.properties.name);
        const newAemPath = path.join(aemPath, folder.properties.name);

        if (!isValidFolder(folder, newAemPath)) {
            continue;
        }

        await fs.mkdir(newLocalPath, { recursive: true });
    
        await downloadAssets({
            apiUrl: getLink(folder, 'self')?.href, 
            localPath: newLocalPath, 
            aemPath: newAemPath
        });
    }

    let downloads = [];

    // Process assets
    for (const asset of entities.filter(entity => entity.class.includes('assets/asset'))) {
        const assetLocalPath = path.join(localPath, asset.properties.name);
        if (isDownloadable(asset)) {
            downloads.push(downloadFile(getLink(asset, 'content')?.href, assetLocalPath));
        }

        // Process in batches of MAX_CONCURRENT_DOWNLOADS
        if (downloads.length >= MAX_CONCURRENT_DOWNLOADS) {
            await Promise.all(downloads);
            downloads = [];
        }
    }

    // Wait for the remaining downloads to finish
    await Promise.all(downloads);
    downloads = [];

    // Handle pagination
    const nextUrl = getLink(data, 'next');
    if (nextUrl) {
        await downloadAssets({
            apiUrl: nextUrl?.href,
            localPath,
            aemPath
        });
    }
}

/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './exported-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;

/***** SCRIPT ENTRY POINT *****/

console.time('Download AEM assets');

await downloadAssets({
    aemPath: AEM_ASSETS_FOLDER, 
    localPath: LOCAL_DOWNLOAD_FOLDER
}).catch(console.error);

console.timeEnd('Download AEM assets');
```

## De export configureren

Werk de configuratievariabelen onder aan het script bij terwijl het script is gedownload.

`AEM_ACCESS_TOKEN` kan worden verkregen gebruikend de stappen in de [ op token-gebaseerde authentificatie aan AEM as a Cloud Service ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview) leerprogramma. Vaak volstaat het token voor ontwikkelaars van 24 uur, zolang het exporteren minder dan 24 uur duurt en de gebruiker die het token genereert leestoegang heeft tot de te exporteren middelen.

```javascript
...
/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './export-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;
```

## De elementen exporteren

Voer het script uit met Node.js om de elementen naar uw lokale computer te exporteren.

Afhankelijk van het aantal elementen en hun grootte kan het voltooien van het script enige tijd in beslag nemen. Aangezien het manuscript uitvoert, registreert het [ de vooruitgang ](#output) aan de console.

```shell
$ node export-assets.js
```

## Uitvoer exporteren

Het exportscript registreert de voortgang naar de console en geeft de elementen aan die worden gedownload. Wanneer het script is voltooid, worden de elementen opgeslagen in de lokale map die in de configuratie is opgegeven. Het logbestand wordt afgesloten met de totale tijd die nodig is om de elementen te downloaden.

```plaintext
...
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring3sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring5sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring6sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/wa_camping_adobe.pdf
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobestock-156407519.jpeg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3094.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3851.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a7083.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a6978.jpg
Download AEM assets: 24.770s
```

De geëxporteerde elementen bevinden zich in de lokale map die is opgegeven in de configuratie `LOCAL_DOWNLOAD_FOLDER` . De mappenstructuur weerspiegelt de AEM Assets-mappenstructuur, waarbij de elementen naar de juiste submappen worden gedownload. Deze dossiers kunnen aan [ gesteunde leveranciers van de wolkenopslag ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/assets-view/bulk-import-assets-view), voor [ bulkinvoer ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/migration/bulk-import) in andere AEM instanties, of voor reservedoeleinden worden geupload.

![ Uitgevoerde activa ](./assets/export/exported-assets.png)
