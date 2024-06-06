---
title: Github.com
description: Leer hoe te om een webhaverzoek van Github.com in een actie van de Bouwer van de App te verifiëren.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
source-git-commit: 4b9f784de5fff7d9ba8cf7ddbe1802c271534010
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Github.com

Met webhooks kunt u integraties maken of instellen die zich abonneren op bepaalde gebeurtenissen op GitHub.com. Wanneer één van die gebeurtenissen wordt teweeggebracht, verzendt GitHub een nuttige lading van de POST van HTTP naar de gevormde URL van Webhaak. Nochtans, voor veiligheidsredenen, is het belangrijk om te verifiëren dat het inkomende webhaverzoek eigenlijk van GitHub en niet van een kwaadwillige actor is. Deze zelfstudie begeleidt u door de stappen om een GitHub.com webshverzoek in een actie van de Bouwer van de Adobe te verifiëren App gebruikend een gedeeld geheim.

## Cadeaugeheim instellen in AppBuilder

1. **geheim toevoegen aan `.env` bestand:**

   In het project App Builder `.env` bestand, voeg een aangepaste sleutel toe voor het GitHub.com-websitegeheim:

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **Bijwerken `ext.config.yaml` bestand:**

   De `ext.config.yaml` bestand moet worden bijgewerkt om de aanvraag van de GitHub.com-webhaak te controleren.

   - De handeling AppBuilder instellen `web` configuratie aan `raw` om de ruwe aanvraaginstantie te ontvangen van GitHub.com.
   - Onder `inputs` in de actieconfiguratie AppBuilder, voeg toe `GITHUB_SECRET` toets, toewijzen aan de `.env` veld dat het geheim bevat. De waarde van deze toets is de `.env` veldnaam, voorafgegaan door `$`.
   - Stel de `require-adobe-auth` aantekening in de actieconfiguratie AppBuilder aan `false` om toe te staan dat de actie wordt geroepen zonder de authentificatie van de Adobe te vereisen.

   Het resultaat `ext.config.yaml` Het bestand moet er ongeveer als volgt uitzien:

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

Voeg vervolgens de hieronder opgegeven JavaScript-code toe (gekopieerd van [Documentatie van GitHub.com](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)) aan uw handeling AppBuilder. Zorg ervoor dat u de `verifySignature` functie.

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

Daarna, verifieer dat het verzoek van GitHub door de handtekening in de verzoekkopbal aan de handtekening komt te vergelijken die door wordt geproduceerd `verifySignature` functie.

In de actie AppBuilder `index.js`voegt u de volgende code toe aan de `main` functie:


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

Ga terug naar GitHub.com en geef GitHub.com dezelfde geheime waarde op wanneer u de webhaak maakt. Gebruik de geheime waarde die in uw `.env` bestand `GITHUB_SECRET` toets.

Ga in GitHub.com naar de instellingen van de opslagplaats en bewerk de webhaak. Geef in de instellingen van de webhaak de geheime waarde op in het dialoogvenster `Secret` veld. Klikken __Webhaak bijwerken__ onder aan om de wijzigingen op te slaan.

![Github Webhaak Secret](./assets/github-webhook-verification/github-webhook-settings.png)

Door deze stappen te volgen, kunt u ervoor zorgen dat uw actie App Builder veilig kan verifiëren dat de inkomende webhaverzoeken inderdaad van uw GitHub.com webhaak zijn.
