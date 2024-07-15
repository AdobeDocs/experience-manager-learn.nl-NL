---
title: JWT-toegangstoken genereren in App Builder-actie
description: Leer hoe u een toegangstoken genereert door JWT-referenties te gebruiken voor gebruik in een App Builder-handeling.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 151
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# JWT-toegangstoken genereren in App Builder-actie

App Builder-acties moeten mogelijk communiceren met Adobe-API&#39;s die zijn gekoppeld aan Adobe Developer Console-projecten die de App Builder-app ook implementeert.

Dit kan de actie van App Builder vereisen om zijn eigen toegangstoken JWT verbonden aan het gewenste project van Adobe Developer Console te produceren.

>[!IMPORTANT]
>
> Het overzicht [ de veiligheidsdocumentatie van App Builder ](https://developer.adobe.com/app-builder/docs/guides/security/) om te begrijpen wanneer het aangewezen is om toegangstokens tegenover het gebruiken van verstrekte toegangstokens te produceren.
>
> De aangepaste actie moet mogelijk eigen beveiligingscontroles uitvoeren om ervoor te zorgen dat alleen gebruikers die toegang hebben tot de App Builder-actie en de Adobe-services die eraan ten grondslag liggen, toegang hebben tot deze actie.


## .env-bestand

Voeg in het `.env` -bestand van het App Builder-project aangepaste sleutels toe voor elk van de JWT-referenties van het Adobe Developer Console-project. De geloofsbrieven JWT kunnen van de geloofsbrieven van het Adobe Developer Console-project __worden verkregen >__ Rekening van de Dienst (JWT) __voor een bepaalde werkruimte.__

![ Adobe Developer Console JWT de Credentials van de Dienst ](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

De waarden voor `JWT_CLIENT_ID` , `JWT_CLIENT_SECRET` , `JWT_TECHNICAL_ACCOUNT_ID` , `JWT_IMS_ORG` kunnen rechtstreeks worden gekopieerd vanuit het JWT Credentials-scherm van het Adobe Developer Console-project.

### Metaretten

Bepaal de Adobe-API&#39;s en de bijbehorende metafoons met de App Builder-actie. U kunt metatomen met komma&#39;s als scheidingsteken weergeven in de `JWT_METASCOPES` -toets. De geldige metarealen zijn vermeld in [ JWT van de Adobe documentatie ](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


De volgende waarde kan bijvoorbeeld worden toegevoegd aan de `JWT_METASCOPES` -toets in de `.env` :

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### Persoonlijke sleutel

De `JWT_PRIVATE_KEY` moet speciaal zijn opgemaakt omdat deze een waarde met meerdere regels is. Deze waarde wordt niet ondersteund in `.env` -bestanden. De gemakkelijkste manier is om base64 de privé sleutel te coderen. Base64 die de privé sleutel (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) coderen kan worden gedaan gebruikend inheemse hulpmiddelen die door uw werkend systeem worden verstrekt.

>[!BEGINTABS]

>[!TAB  macOS ]

1. Openen `Terminal`
1. De opdracht uitvoeren `base64 -i /path/to/private.key | pbcopy`
1. De base64-uitvoer wordt automatisch gekopieerd naar het klembord
1. Plakken in `.env` als waarde op corresponderende toets

>[!TAB  Vensters ]

1. Openen `Command Prompt`
1. De opdracht uitvoeren `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. De opdracht uitvoeren `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. De base64-uitvoer naar het klembord kopiëren
1. Plakken in `.env` als waarde op corresponderende toets

>[!TAB  Linux® ]

1. Open terminal
1. De opdracht uitvoeren `base64 private.key`
1. De base64-uitvoer naar het klembord kopiëren
1. Plakken in `.env` als waarde op corresponderende toets

>[!ENDTABS]

De volgende privé-sleutel met base64-codering kan bijvoorbeeld worden toegevoegd aan de `JWT_PRIVATE_KEY` -toets in de `.env` :

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## Invoer toewijzen

Als de JWT-referentie in het `.env` -bestand is ingesteld, moeten deze worden toegewezen aan invoer van de AppBuilder-actie zodat deze in de handeling zelf kan worden gelezen. Hiervoor voegt u items toe voor elke variabele in de `ext.config.yaml` handeling `inputs` in de notatie: `PARAMS_INPUT_NAME: $ENV_KEY` .

Bijvoorbeeld:

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

De sleutels die onder `inputs` worden gedefinieerd, zijn beschikbaar voor het `params` -object dat aan de App Builder-handeling wordt geleverd.


## JWT-referenties voor toegang tot token

In de actie van App Builder, zijn de geloofsbrieven JWT beschikbaar in het `params` voorwerp, en bruikbaar door [`@adobe/jwt-auth` ](https://www.npmjs.com/package/@adobe/jwt-auth) om een toegangstoken te produceren, die beurtelings tot andere Adobe APIs en de diensten kan toegang hebben.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
