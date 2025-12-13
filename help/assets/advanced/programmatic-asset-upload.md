---
title: Programmatische middelen uploaden naar AEM as a Cloud Service
description: Leer hoe u elementen naar AEM as a Cloud Service kunt uploaden met de bibliotheek @adobe/aem-upload Node.js.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Asset Management
role: Developer
level: Intermediate
last-substantial-update: 2025-11-14T00:00:00Z
doc-type: Tutorial
jira: KT-19571
thumbnail: KT-19571.png
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 0%

---


# Programmatische middelen uploaden naar AEM as a Cloud Service

Leer hoe te om activa aan het milieu van AEM as a Cloud Service te uploaden gebruikend de cliënttoepassing die [&#x200B; aem-upload &#x200B;](https://github.com/adobe/aem-upload) Node.js bibliotheek gebruikt.


>[!VIDEO](https://video.tv.adobe.com/v/3476958?captions=dut&quality=12&learn=on)


## Wat u leert

In deze zelfstudie leert u:

+ Hoe te om de _directe binaire upload_ benadering te gebruiken om activa aan het milieu van AEM as a Cloud Service (RDE, Dev, Stadium, Prod) te uploaden gebruikend [&#x200B; aem-upload &#x200B;](https://github.com/adobe/aem-upload) Node.js bibliotheek.
+ Hoe te om te vormen en in werking te stellen [&#x200B; aem-activa-upload-steekproef &#x200B;](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) toepassing om activa aan het milieu van AEM as a Cloud Service te uploaden.
+ Controleer de code van de voorbeeldtoepassing en begrijp de implementatiedetails.
+ Begrijp de beste praktijken voor programmatic activa uploadt aan het milieu van AEM as a Cloud Service.

## Begrijpend de _directe binaire upload_ benadering

De _directe binaire upload_ benadering laat u dossiers van uw bronsysteem _direct aan wolkenopslag_ in het milieu van AEM as a Cloud Service gebruikend a _presigned URL_ uploaden. Het elimineert de behoefte om binaire gegevens door de processen van Java van AEM te leiden, resulterend in snellere uploads en lagere serverlading.

Voordat u de voorbeeldtoepassing uitvoert, moeten we de directe binaire uploadflow begrijpen.

In directe binaire uploadflow worden de binaire gegevens rechtstreeks geüpload naar cloudopslag met vooraf ondertekende URL&#39;s. De AEM as a Cloud Service is verantwoordelijk voor lichte verwerking, zoals het genereren van de vooraf ondertekende URL&#39;s en het op de hoogte brengen van de voltooiing van het uploaden aan AEM Asset Compute Service. Het volgende logische stroomdiagram illustreert de directe binaire upload stroom.

![&#x200B; Directe binaire upload stroom &#x200B;](./assets/programmatic-asset-upload/direct-binary-asset-upload-flow.png)

### De Aem-upload bibliotheek

De [&#x200B; aem-upload &#x200B;](https://github.com/adobe/aem-upload) Node.js bibliotheek abstracts de implementatiedetails van _directe binaire upload_ benadering. Het verstrekt twee klassen om het uploadproces te ordenen:

+ **FileSystemUpload** - gebruik het wanneer het uploaden van dossiers van het lokale dossiersysteem, met inbegrip van steun voor folderstructuren
+ **DirectBinaryUpload** - gebruik het voor meer fijnkorrelige controle over het binaire uploadproces, zoals het uploaden van stromen of buffers

>[!CAUTION]
>
>Er is GEEN equivalent van [&#x200B; a-upload &#x200B;](https://github.com/adobe/aem-upload) bibliotheek in Java. De cliënttoepassing moet in Node.js worden geschreven om de _directe binaire upload_ benadering te gebruiken. Voor extra informatie, zie [&#x200B; Experience Manager Assets APIs en verrichtingenpagina &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/assets/admin/developer-reference-material-apis#use-cases-and-apis).

## Voorbeeldtoepassing

Gebruik [&#x200B; a-element-activa-upload-steekproef &#x200B;](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) toepassing om het programmatic proces van de activaupload te leren. De steekproeftoepassing toont het gebruik van zowel `FileSystemUpload` als `DirectBinaryUpload` klassen van [&#x200B; aan a-upload &#x200B;](https://github.com/adobe/aem-upload) bibliotheek.

### Vereisten

Controleer voordat u de voorbeeldtoepassing uitvoert of aan de volgende voorwaarden is voldaan:

+ AEM as a Cloud Service-auteursomgeving zoals Rapid Development Environment (RDE), Dev-omgeving, enz.
+ Node.js (nieuwste LTS-versie)
+ Basisbegrip van Node.js en npm

>[!CAUTION]
>
> U kunt de AEM as a Cloud Service SDK (ook wel een lokale AEM-instantie genoemd) NIET gebruiken om het uploadproces voor programmatische middelen te testen. U moet een AEM as a Cloud Service-omgeving gebruiken, zoals Rapid Development Environment (RDE), Dev-omgeving, enzovoort.

### Download de voorbeeldtoepassing

1. Download het [&#x200B; aem-activa-upload-steekproef &#x200B;](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) dossier van toepassingsZIP en haal het uit.

   ```bash
   $ unzip aem-asset-upload-sample.zip
   ```

1. Open de uitgepakte map in uw favoriete code-editor.

   ```bash
   $ cd aem-asset-upload-sample
   $ code .
   ```

1. Gebruikend de terminal van de coderedacteur, installeer de gebiedsdelen.

   ```bash
   $ npm install
   ```

   ![&#x200B; toepassing van de Steekproef &#x200B;](./assets/programmatic-asset-upload/install-dependencies.png)

### De voorbeeldtoepassing configureren

Alvorens de steekproeftoepassing in werking te stellen, moet u het met noodzakelijke het omgevingsdetails van AEM as a Cloud Service zoals de Auteur URL van AEM, _authentificatiemethode_ en de weg van de activaomslag vormen.

Er zijn _veelvoudige authentificatiemethodes_ die door [&#x200B; worden gesteund aem-upload &#x200B;](https://github.com/adobe/aem-upload) Node.js bibliotheek. De volgende lijst vat de gesteunde _authentificatiemethodes_ en hun doel samen.

| | Basisverificatie | [&#x200B; Lokale ontwikkelingstoken &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token) | [&#x200B; geloofsbrieven van de Dienst &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials) | [&#x200B; OAuth S2S &#x200B;](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) | [&#x200B; OAuth Web App &#x200B;](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) | [&#x200B; OAuth SPA &#x200B;](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) |
|---|---|---|---|---|---|---|
| Wordt ondersteund? | &check; | &check; | &check; | &cross; | &cross; | &cross; |
| Doel | Lokale ontwikkeling | Lokale ontwikkeling | Productie | NVT | NVT | NVT |

Voer de volgende stappen uit om de voorbeeldtoepassing te configureren:

1. Kopieer het `env.example` -bestand naar het `.env` -bestand.

   ```bash
   $ cp env.example .env
   ```

1. Open het bestand `.env` en werk de omgevingsvariabele `AEM_URL` bij met de URL van de AEM as a Cloud Service-auteur.

1. Kies de verificatiemethode in de volgende opties en werk de bijbehorende omgevingsvariabelen bij.

>[!BEGINTABS]

>[!TAB  Basisauthentificatie ]

Om basisauthentificatie te gebruiken, moet u een gebruiker in het milieu van AEM as a Cloud Service tot stand brengen.

1. Meld u aan bij uw AEM as a Cloud Service-omgeving.

1. Navigeer aan de **Hulpmiddelen** > **Veiligheid** > **Gebruikers** en klik **creeer** knoop.

   ![&#x200B; creeer gebruiker &#x200B;](./assets/programmatic-asset-upload/create-user.png)

1. Voer de gebruikersgegevens in

   ![&#x200B; de details van de Gebruiker &#x200B;](./assets/programmatic-asset-upload/user-details.png)

1. In het **lusje van Groepen**, voeg de **DAM Gebruikers** groep toe. Klik **sparen en sluit** knoop.

   ![&#x200B; voeg de Groep van Gebruikers DAM &#x200B;](./assets/programmatic-asset-upload/add-dam-users-group.png) toe

1. Werk de omgevingsvariabelen `AEM_USERNAME` en `AEM_PASSWORD` bij met de gebruikersnaam en het wachtwoord van de gemaakte gebruiker.

>[!TAB  Lokale ontwikkelingstoken ]

Om het lokale ontwikkelingstoken te krijgen, moet u **AEM** Developer Console gebruiken. Het gegenereerde token is van het type JSON Web Token (JWT).

1. Login aan [&#x200B; Adobe Cloud Manager &#x200B;](https://experience.adobe.com/#/@aem/cloud-manager) en navigeer aan de gewenste **Milieu** detailspagina. Klik **&quot;...&quot;** en selecteer **Developer Console**.

   ![Developer Console](./assets/programmatic-asset-upload/developer-console.png)

1. Login aan AEM Developer Console en gebruik de _Nieuwe knoop van de Console_ om aan de nieuwere console over te schakelen.

1. Van de **sectie van Hulpmiddelen**, uitgezochte **Integraties** en klik **krijgen lokale symbolische** knoop.

   ![&#x200B; krijg lokaal teken &#x200B;](./assets/programmatic-asset-upload/get-local-token.png)

1. Kopieer de tokenwaarde en werk de omgevingsvariabele `AEM_BEARER_TOKEN` bij met de tokenwaarde.

Het token voor lokale ontwikkeling is 24 uur geldig en wordt uitgegeven door de gebruiker die het token heeft gegenereerd.

>[!TAB  geloofsbrieven van de Dienst ]

Om de de dienstgeloofsbrieven te krijgen, moet u **AEM** Developer Console gebruiken. Het wordt gebruikt om het teken van het Symbolische (JWT) type van het Web te produceren JSON gebruikend de [&#x200B; jwt-auth &#x200B;](https://www.npmjs.com/package/@adobe/jwt-auth) npm module.

1. Login aan [&#x200B; Adobe Cloud Manager &#x200B;](https://experience.adobe.com/#/@aem/cloud-manager) en navigeer aan de gewenste **Milieu** detailspagina. Klik **&quot;...&quot;** en selecteer **Developer Console**.

   ![Developer Console](./assets/programmatic-asset-upload/developer-console.png)

1. Login aan AEM Developer Console en gebruik de _Nieuwe knoop van de Console_ om aan de nieuwere console over te schakelen.

1. Van de **sectie van Hulpmiddelen**, uitgezochte **Integraties** en klik **creëren nieuwe technische rekening** knoop.

   ![&#x200B; krijgt de dienstgeloofsbrieven &#x200B;](./assets/programmatic-asset-upload/get-service-credentials.png)

1. Klik de **optie van de Mening** om de de dienstgeloofsbrieven JSON te kopiëren.

   ![&#x200B; geloofsbrieven van de Dienst &#x200B;](./assets/programmatic-asset-upload/service-credentials.png)

1. Maak een `service-credentials.json` -bestand in de hoofdmap van de voorbeeldtoepassing en plak de JSON-servicereferenties in het bestand.

1. Werk de omgevingsvariabele `AEM_SERVICE_CREDENTIALS_FILE` bij met het pad naar het bestand service-credentials.json.

1. Zorg ervoor dat de gebruiker van de de dienstreferentie de noodzakelijke toestemmingen heeft om activa aan het milieu van AEM as a Cloud Service te uploaden. Voor meer informatie, zie [&#x200B; toegang in AEM &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials#configure-access-in-aem) pagina vormen.

>[!ENDTABS]

Hier volgt een volledig voorbeeld van een `.env` -bestand waarin alle drie verificatiemethoden zijn geconfigureerd.

```
# AEM Environment Configuration
# Copy this file to .env and fill in your AEM as a Cloud Service details

# AEM as a Cloud Service Author URL (without trailing slash)
# Example: https://author-p12345-e67890.adobeaemcloud.com
AEM_URL=https://author-p63947-e1733365.adobeaemcloud.com

# Upload Configuration
# Target folder in AEM DAM where assets will be uploaded
TARGET_FOLDER=/content/dam

# DirectBinaryUpload Remote URLs (required for DirectBinaryUpload example)
# URLs for remote files to upload in the DirectBinaryUpload example
# These demonstrate uploading from remote sources (URLs, CDNs, APIs)
REMOTE_FILE_URL_1=https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets

################################################################
# Authentication - Choose one of the following methods:
################################################################

# Method 1: Service Credentials (RECOMMENDED for production)
# Download service credentials JSON from AEM Developer Console and save it locally
# Then provide the path to the file here
AEM_SERVICE_CREDENTIALS_FILE=./service-credentials.json

# Method 2: Bearer Token Authentication (for manual testing)
AEM_BEARER_TOKEN=eyJhbGciOiJSUzI1NiIsIng1dSI6Imltc19uYTEta2V5LWF0LTEuY2VyIiwia2lkIjoiaW1zX25hM....fsdf-Rgt5hm_8FHutTyNQnkj1x1SUs5OkqUfJaGBaKBKdqQ

# Method 3: Basic Authentication (for development/testing only)
AEM_USERNAME=asset-uploader-local-user
AEM_PASSWORD=asset-uploader-local-user

# Optional: Enable detailed logging
DEBUG=false
```

### De voorbeeldtoepassing uitvoeren

In de voorbeeldtoepassing worden drie verschillende manieren getoond om voorbeeldbestanden te uploaden naar de AEM as a Cloud Service-omgeving.

1. **FileSystemUpload** - upload dossiers van een lokaal dossiersysteem met de steun van de folderstructuur en auto-omslagverwezenlijking
1. **DirectBinaryUpload** - uploadt a [&#x200B; ver dossier &#x200B;](https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets). Het binaire bestand wordt gebufferd in het geheugen voordat het bestand naar de AEM as a Cloud Service-omgeving wordt geüpload.
1. **Geüpload de Partij** - uploadt veelvoudige dossiers van een lokaal dossiersysteem in partijen met automatische retry logica en foutenterugwinning. Achter de schermen wordt de klasse `FileSystemUpload` gebruikt om bestanden van het lokale bestandssysteem te uploaden.

Te uploaden elementen bevinden zich in de map `sample-assets` en bevatten `img` , `video` en `doc` submappen die elk een paar voorbeeldelementen bevatten.

1. Gebruik de volgende opdracht om de voorbeeldtoepassing uit te voeren:

```bash
$ npm start
```

1. Ga het gewenste optiesaantal _van de volgende keuzen in 0&rbrace;:_

```
╔════════════════════════════════════════════════════════════╗
║      AEM Asset Upload Sample Application                   ║
║      Demonstrating @adobe/aem-upload library               ║
╚════════════════════════════════════════════════════════════╝

Choose an upload method:

1. FileSystemUpload - Upload files from local filesystem with auto-folder creation
2. DirectBinaryUpload - Upload from remote URLs/streams to AEM
3. Batch Upload - Upload multiple files in batches with retry logic
4. Exit
```

Op de volgende tabbladen ziet u de uitgevoerde voorbeeldtoepassing, de uitvoer en geüploade middelen in de AEM as a Cloud Service-omgeving voor elke uploadmethode.

>[!BEGINTABS]

>[!TAB  FileSystemUpload ]

1. De voorbeeldtoepassing voor de optie `FileSystemUpload` :

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 2.67s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets geüpload met de optie `FileSystemUpload` in de AEM as a Cloud Service-omgeving:

   ![&#x200B; Geüploade activa in het milieu van AEM as a Cloud Service gebruikend klasse FileSystemUpload &#x200B;](./assets/programmatic-asset-upload/uploaded-assets-aem-file-system-upload.png)

>[!TAB  DirectBinaryUpload ]

1. De voorbeeldtoepassing voor de optie `DirectBinaryUpload` :

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 1
Successful: 1
Total time: 561ms
──────────────────────────────────────────────────

✅ Successfully uploaded to AEM: https://author-p63947-e1733365.adobeaemcloud.com/ui#/aem/assets.html/content/dam?appId=aemshell
  → remote-file-1.png
    Source: https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets
✓ 
All files uploaded successfully!
```

1. Assets geüpload met de optie `DirectBinaryUpload` in de AEM as a Cloud Service-omgeving:

![&#x200B; Geüploade activa in het milieu van AEM as a Cloud Service gebruikend klasse DirectBinaryUpload &#x200B;](./assets/programmatic-asset-upload/uploaded-assets-aem-direct-binary-upload.png)

>[!TAB  Partij uploaden ]

1. De voorbeeldtoepassing voor de optie `Batch Upload` :

```bash
...
ℹ Found 4 item(s) to upload in batches (directories + files)
ℹ Batch size: 2 (small for demo, use 10-50 for production)

...

✓ Batch 2 completed in 2.79s

Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 4.50s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets geüpload met de optie `Batch Upload` in de AEM as a Cloud Service-omgeving:

![&#x200B; Geüploade activa in het milieu van AEM as a Cloud Service die klasse BatchUpload gebruiken &#x200B;](./assets/programmatic-asset-upload/uploaded-assets-aem-batch-upload.png)

>[!ENDTABS]

## De voorbeeldtoepassingscode controleren

Het belangrijkste ingangspunt van de steekproeftoepassing is het `index.js` dossier. Deze bevat de functie `promptUser` die de gebruiker om een keuze vraagt en het geselecteerde voorbeeld uitvoert.

```javascript
/**
 * Prompts user for choice and executes the selected example
 */
function promptUser() {
  rl.question(chalk.bold('Enter your choice (1-4): '), async (answer) => {
    console.log('');

    try {
      switch (answer.trim()) {
        case '1':
          console.log(chalk.bold.green('\n▶ Running FileSystemUpload Example...\n'));
          await filesystemUpload.main();
          break;

        case '2':
          console.log(chalk.bold.green('\n▶ Running DirectBinaryUpload Example...\n'));
          await directBinaryUpload.main();
          break;

        case '3':
          console.log(chalk.bold.green('\n▶ Running Batch Upload Example...\n'));
          await batchUpload.main();
          break;

        case '4':
          rl.close();
          return;

        default:
          console.log(chalk.red('\n✗ Invalid choice. Please enter 1, 2, 3, or 4.\n'));
      }

      // After example completes, ask if user wants to continue
      rl.question(chalk.bold('\nPress Enter to return to menu or Ctrl+C to exit...'), () => {
        displayMenu();
        promptUser();
      });

    } catch (error) {
      console.error(chalk.red('\n✗ Error:'), error.message);
      rl.question(chalk.bold('\nPress Enter to return to menu...'), () => {
        displayMenu();
        promptUser();
      });
    }
  });
}
```

Voor volledige code raadpleegt u het `index.js` -bestand in de voorbeeldtoepassing.

In de volgende tabbladen ziet u de implementatiedetails van elke uploadmethode.

>[!BEGINTABS]

>[!TAB  FileSystemUpload ]

De klasse `FileSystemUpload` wordt gebruikt om bestanden te uploaden van het lokale bestandssysteem met ondersteuning voor de mapstructuur en het maken van automatische mappen.

```javascript
...
// Initialize FileSystemUpload
const upload = new FileSystemUpload();

const startTime = Date.now();
const spinner = createSpinner('Preparing upload...');

// Upload options for this specific upload
// For FileSystemUpload, the url should include the target folder path
const fullUrl = `${options.url}${targetFolder}`;

const uploadOptions = new FileSystemUploadOptions()
  .withUrl(fullUrl)
  .withDeepUpload(true);  // Enable recursive upload of subdirectories

// Add HTTP options including headers (auth is already in headers from config)
uploadOptions.withHttpOptions({
  headers: {
    ...options.headers,
    'X-Upload-Source': 'FileSystemUpload-Example'
  }
});

spinner.stop();

// Attach progress event handlers to the upload instance
handleUploadProgress(upload);

// Perform the upload and wait for completion
// Upload the contents (subdirectories and files) not the parent folder
const uploadResult = await upload.upload(uploadOptions, uploadPaths);
const totalTime = Date.now() - startTime;

// Analyze results using shared function
const analysis = analyzeUploadResult(uploadResult);

// Display summary
displayUploadSummary(analysis, totalTime);
...
```

Voor volledige code raadpleegt u het `examples/filesystem-upload.js` -bestand in de voorbeeldtoepassing.

>[!TAB  DirectBinaryUpload ]

De klasse `DirectBinaryUpload` wordt gebruikt om een extern bestand te uploaden naar de AEM as a Cloud Service-omgeving.

```javascript
...
/**
 * Creates upload file objects for DirectBinaryUpload from remote URLs
 * @param {Array<Object>} remoteFiles - Array of objects with url, fileName, targetFolder
 * @returns {Array<Object>} Array of upload file objects
 */
async function createUploadFilesFromUrls(remoteFiles) {
  const uploadFiles = [];
  
  for (const remoteFile of remoteFiles) {
    logInfo(`Fetching: ${remoteFile.fileName} from ${remoteFile.url}`);
    try {
      const fileBuffer = await fetchRemoteFile(remoteFile.url);
      uploadFiles.push({
        fileName: remoteFile.fileName,
        fileSize: fileBuffer.length,
        blob: fileBuffer,  // DirectBinaryUpload uses 'blob' for buffers
        targetFolder: remoteFile.targetFolder,
        targetFile: `${remoteFile.targetFolder}/${remoteFile.fileName}`,
        sourceUrl: remoteFile.url  // Track source URL for display in summary
      });
      logSuccess(`Downloaded: ${remoteFile.fileName} (${formatBytes(fileBuffer.length)})`);
    } catch (error) {
      logError(`Failed to fetch ${remoteFile.fileName}: ${error.message}`);
    }
  }
  
  return uploadFiles;
}

...

    // Initialize DirectBinaryUpload
    const upload = new DirectBinaryUpload();

    // Fetch remote files and create upload objects
    const uploadFiles = await createUploadFilesFromUrls(remoteFiles);

...    

    // Upload options for each file
    const uploadOptions = new DirectBinaryUploadOptions()
        .withUrl(fullUrl)
        .withUploadFiles([uploadFile]);
    
    // Add HTTP options (auth is already in headers from config)
    uploadOptions
        .withHttpOptions({
        headers: {
            ...options.headers,
            'X-Upload-Source': 'DirectBinaryUpload-Example'
        }
        })
        .withMaxConcurrent(5);

    // Upload individual file and wait for completion
    const uploadResult = await upload.uploadFiles(uploadOptions);
```

Voor volledige code raadpleegt u het `examples/direct-binary-upload.js` -bestand in de voorbeeldtoepassing.

>[!TAB  Partij uploaden ]

De bestanden worden in batches gesplitst en geüpload in batches met de automatische logica voor het opnieuw proberen en het herstellen van fouten. Achter de schermen wordt de klasse `FileSystemUpload` gebruikt om bestanden van het lokale bestandssysteem te uploaden.

```javascript
...
async function uploadInBatches(paths, options, targetFolder, batchSize = 2) {
  const allResults = [];
  const totalPaths = paths.length;
  const totalBatches = Math.ceil(totalPaths / batchSize);

  logInfo(`Processing ${totalPaths} item(s) in ${totalBatches} batch(es)`);

  for (let i = 0; i < totalPaths; i += batchSize) {
    const batchNumber = Math.floor(i / batchSize) + 1;
    const batch = paths.slice(i, i + batchSize);
    
    console.log(`\n${'='.repeat(50)}`);
    logInfo(`Batch ${batchNumber}/${totalBatches} - Uploading ${batch.length} item(s)`);
    console.log('='.repeat(50));

    const batchStartTime = Date.now();
    let retryCount = 0;
    const maxRetries = 3;
    let batchResults = null;

    // Retry logic for failed batches
    while (retryCount <= maxRetries) {
      try {
        // Create a fresh upload instance for each retry to avoid duplicate event listeners
        const upload = new FileSystemUpload();
        
        const fullUrl = `${options.url}${targetFolder}`;
        
        const uploadOptions = new FileSystemUploadOptions()
          .withUrl(fullUrl)
          .withDeepUpload(true);  // Enable recursive upload of subdirectories
        
        // Add HTTP options including headers (auth is already in headers from config)
        uploadOptions.withHttpOptions({
          headers: {
            ...options.headers,
            'X-Upload-Source': 'Batch-Upload-Example',
            'X-Batch-Number': batchNumber
          }
        });

        // Track progress - attach listeners to upload instance
        upload.on('foldercreated', (data) => {
          logSuccess(`Created folder: ${data.folderName} at ${data.targetFolder}`);
        });
        
        let currentFile = '';
        upload.on('filestart', (data) => {
          currentFile = data.fileName;
          logInfo(`Starting: ${currentFile}`);
        });

        upload.on('fileprogress', (data) => {
          const percentage = ((data.transferred / data.fileSize) * 100).toFixed(1);
          process.stdout.write(
            `\r  Progress: ${percentage}% - ${formatBytes(data.transferred)}/${formatBytes(data.fileSize)}`
          );
        });

        upload.on('fileend', (data) => {
          process.stdout.write('\n');
          logSuccess(`Completed: ${data.fileName}`);
        });

        upload.on('fileerror', (data) => {
          // Only show in DEBUG mode (may be retries)
          if (process.env.DEBUG === 'true') {
            process.stdout.write('\n');
            const errorMsg = data.error?.message || data.message || 'Unknown error';
            logWarning(`Error (may retry): ${data.fileName} - ${errorMsg}`);
          }
        });

        // Perform upload and wait for batch completion
        const uploadResult = await upload.upload(uploadOptions, batch);
        
        const batchEndTime = Date.now();
        const batchTime = batchEndTime - batchStartTime;
        
        logSuccess(`Batch ${batchNumber} completed in ${formatTime(batchTime)}`);
        
        // Extract detailed results from the upload result
        batchResults = uploadResult.detailedResult || [];
        break; // Success, exit retry loop

      } catch (error) {
        retryCount++;
        if (retryCount <= maxRetries) {
          logWarning(`Batch ${batchNumber} failed. Retry ${retryCount}/${maxRetries}...`);
          await new Promise(resolve => setTimeout(resolve, 2000 * retryCount)); // Exponential backoff
        } else {
          logError(`Batch ${batchNumber} failed after ${maxRetries} retries: ${error.message}`);
          // Mark all files in batch as failed
          batchResults = batch.map(file => ({
            fileName: path.basename(file),
            error: error,
            success: false
          }));
        }
      }
    }

    if (batchResults) {
      allResults.push(...batchResults);
    }
  }

  return allResults;
}
```

Voor volledige code raadpleegt u het `examples/batch-upload.js` -bestand in de voorbeeldtoepassing.

>[!ENDTABS]

Het `README.md` -bestand van de voorbeeldtoepassing bevat ook de gedetailleerde documentatie voor de voorbeeldtoepassing.

## Aanbevolen procedures

1. **kies de juiste authentificatiemethode:**
De dienstgeloofsbrieven van het gebruik voor productiemilieu&#39;s, het Lokale ontwikkelingstoken en de Basisauthentificatie voor ontwikkeling/het testen slechts. Zorg ervoor dat de gebruiker van de de dienstreferentie de noodzakelijke toestemmingen heeft om activa aan het milieu van AEM as a Cloud Service te uploaden.

1. **kies de juiste uploadmethode:**
Gebruik FileSystemUpload voor lokale bestanden met automatische mapaanmaak, DirectBinaryUpload voor streams/buffers/externe URL&#39;s met fijnkorrelige besturing en Batch Upload-patroon voor productieomgevingen met meer dan 1000 bestanden waarvoor opnieuw logica nodig is.

1. **de voorwerpen van het dossier van de Structuur DirectBinaryUpload correct**
Gebruik de eigenschap blob (not buffer) met de vereiste velden: { fileName, fileSize, blob: buffer, targetFolder } en onthoud dat DirectBinaryUpload GEEN automatisch mappen maakt.

1. **toepassing van de Steekproef als verwijzing:**
De voorbeeldtoepassing is een goede referentie voor de implementatiedetails van het uploadproces voor programmatische elementen. U kunt het gebruiken als uitgangspunt voor uw eigen implementatie.
