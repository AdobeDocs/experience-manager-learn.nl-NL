---
title: Toegangstoken server-naar-server genereren in App Builder-actie
description: Leer hoe te om een toegangstoken te produceren door Server-aan-Server geloofsbrieven OAuth voor gebruik in een actie van App Builder te gebruiken.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: 122
exl-id: 919cb9de-68f8-4380-940a-17274183298f
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Toegangstoken server-naar-server genereren in App Builder-actie

De acties van App Builder kunnen met Adobe APIs moeten in wisselwerking staan die **OAuth Server-aan-Server geloofsbrieven** steunen en met de projecten van Adobe Developer Console verbonden zijn App Builder app wordt opgesteld.

Deze gids verklaart hoe te om een toegangstoken te produceren door _Server-aan-Server geloofsbrieven_ voor gebruik in een actie van App Builder te gebruiken.

>[!IMPORTANT]
>
> De geloofsbrieven van de Rekening van de Dienst (JWT) zijn afgekeurd ten gunste van de geloofsbrieven van de Server-aan-Server OAuth. Er zijn echter nog steeds enkele Adobe API&#39;s die alleen JWT-referenties (Service Account) ondersteunen en de migratie naar OAuth Server-to-Server wordt uitgevoerd. Raadpleeg de Adobe API-documentatie om te begrijpen welke referenties worden ondersteund.

## Adobe Developer Console-projectconfiguraties

Terwijl het toevoegen van de gewenste Adobe API aan het project van Adobe Developer Console, in _vorm API_ stap, selecteer het **Server-aan-Server** authentificatietype OAuth.

![ Adobe Developer Console - OAuth server-aan-Server ](./assets/s2s-auth/oauth-server-to-server.png)

Selecteer het gewenste productprofiel om het bovenstaande automatisch gemaakte serviceaccount voor integratie toe te wijzen. De machtigingen voor serviceaccounts worden dus via het productprofiel beheerd.

![ Adobe Developer Console - het Profiel van het Product ](./assets/s2s-auth/select-product-profile.png)

## .env-bestand

Voeg in het bestand `.env` van het App Builder-project aangepaste sleutels toe voor de OAuth Server-to-Server-referenties van het Adobe Developer Console-project. De server-aan-Server van OAuth geloofswaarden kunnen van de geloofsbrieven van het project van Adobe Developer Console __worden verkregen >__ Server-aan-Server __voor een bepaalde werkruimte.__

![ Adobe Developer Console OAuth Server-aan-Server Credentials ](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

De waarden voor `OAUTHS2S_CLIENT_ID` , `OAUTHS2S_CLIENT_SECRET` , `OAUTHS2S_CECREDENTIALS_METASCOPES` kunnen rechtstreeks worden gekopieerd uit het OAuth Server-to-Server-aanmeldingsscherm van het Adobe Developer Console-project.

## Invoer toewijzen

Als de referentie-waarde van OAuth Server-aan-Server in het `.env` dossier wordt geplaatst, moeten zij aan actiinput worden in kaart gebracht AppBuilder zodat kunnen zij in de actie zelf worden gelezen. Hiervoor voegt u items toe voor elke variabele in de `ext.config.yaml` handeling `inputs` in de notatie: `PARAMS_INPUT_NAME: $ENV_KEY` .

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
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

De sleutels die onder `inputs` worden gedefinieerd, zijn beschikbaar voor het `params` -object dat aan de App Builder-handeling wordt geleverd.

## OAuth Server-aan-Server geloofsbrieven aan toegangstoken

In de App Builder-actie zijn de OAuth Server-to-Server-referenties beschikbaar in het `params` -object. Gebruikend deze geloofsbrieven kan het toegangstoken worden geproduceerd gebruikend [ OAuth 2.0 bibliotheken ](https://oauth.net/code/). Of u kunt de [ bibliotheek van de Vetch van de Knoop ](https://www.npmjs.com/package/node-fetch) gebruiken om een POST- verzoek aan het symbolische eindpunt van Adobe te maken IMS om het toegangstoken te krijgen.

In het volgende voorbeeld wordt getoond hoe u de `node-fetch` -bibliotheek kunt gebruiken om een POST-aanvraag in te dienen bij het Adobe IMS-tokeneindpunt om het toegangstoken op te halen.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const tokenResponse = await fetch(adobeIMSV3TokenEndpointURL, options);
    const tokenResponseJSON = await tokenResponse.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = tokenResponseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const aemDataResponse = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!aemDataResponse.ok) { throw new Error("Request to API failed with status code " + aemDataResponse.status);}

    // API data
    let data = await aemDataResponse.json();

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
