---
title: Github.com
description: Leer hoe u een webhaverzoek van Github.com kunt verifiëren in een App Builder-actie.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
exl-id: 5492dc7b-f034-4a7f-924d-79e083349e26
source-git-commit: 8f64864658e521446a91bb4c6475361d22385dc1
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Github.com

Met webhooks kunt u integraties maken of instellen die zich abonneren op bepaalde gebeurtenissen op GitHub.com. Wanneer één van die gebeurtenissen wordt teweeggebracht, verzendt GitHub een nuttige lading van de POST van HTTP naar de gevormde URL van Webhaak. Nochtans, voor veiligheidsredenen, is het belangrijk om te verifiëren dat het inkomende webhaverzoek eigenlijk van GitHub en niet van een kwaadwillige actor is. Deze zelfstudie begeleidt u door de stappen om een GitHub.com webshverzoek in een Adobe App Builder actie te verifiëren gebruikend een gedeeld geheim.

## Cadeaugeheim instellen in AppBuilder

1. **voeg geheim aan `.env` dossier toe:**

   Voeg in het `.env` -bestand van het App Builder-project een aangepaste sleutel toe voor het GitHub.com-websitegeheim:

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **Update `ext.config.yaml` dossier:**

   Het bestand `ext.config.yaml` moet worden bijgewerkt om de aanvraag van de GitHub.com-website te controleren.

   - Stel de configuratie van de handeling AppBuilder `web` in op `raw` om de onbewerkte aanvraaginstantie van GitHub.com te ontvangen.
   - Voeg onder `inputs` in de actieconfiguratie AppBuilder de `GITHUB_SECRET` -toets toe en wijs deze toe aan het `.env` -veld dat het geheim bevat. De waarde van deze toets is de veldnaam `.env` die vooraf is ingesteld door `$` .
   - Stel de `require-adobe-auth` -annotatie in de actieconfiguratie AppBuilder in op `false` zodat de handeling kan worden aangeroepen zonder dat verificatie van de Adobe vereist is.

   Het resulterende `ext.config.yaml` -bestand moet er ongeveer als volgt uitzien:

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## Verificatiecode toevoegen aan AppBuilder-actie

Daarna, voeg de hieronder verstrekte code van JavaScript (die van [ wordt gekopieerd GitHub.com documentatie ](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)) aan uw actie AppBuilder toe. Exporteer de functie `verifySignature` .

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## Verificatie implementeren in AppBuilder-actie

Daarna, verifieer dat het verzoek van GitHub door de handtekening in de verzoekkopbal te vergelijken met de handtekening komt die door de `verifySignature` functie wordt geproduceerd.

Voeg de volgende code toe aan de functie `main` in het actiepakket AppBuilder `index.js` :


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## Webhaak configureren in GitHub

Ga terug naar GitHub.com en geef GitHub.com dezelfde geheime waarde op wanneer u de webhaak maakt. Gebruik de geheime waarde die is opgegeven in de sleutel `GITHUB_SECRET` van het bestand `.env` .

Ga in GitHub.com naar de instellingen van de opslagplaats en bewerk de webhaak. Geef in de instellingen van de webhaak de geheime waarde op in het veld `Secret` . Klik __WebHaak van de Update__ bij de bodem om de veranderingen te bewaren.

![ Github Webhaak Geheim ](./assets/github-webhook-verification/github-webhook-settings.png)

Als u deze stappen uitvoert, zorgt u ervoor dat uw App Builder-actie veilig kan controleren of binnenkomende webhaanvragen afkomstig zijn van uw GitHub.com-webhaak.
