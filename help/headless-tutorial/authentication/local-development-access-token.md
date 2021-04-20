---
title: Toegangstoken lokale ontwikkeling
description: AEM de Tokens van de Toegang van de Lokale Ontwikkeling worden gebruikt om de ontwikkeling van integratie met AEM als Cloud Service te versnellen die programmatically met de Auteur van AEM of de Publish diensten over HTTP in wisselwerking staan.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1076'
ht-degree: 0%

---


# Toegangstoken lokale ontwikkeling

Ontwikkelaars die integraties bouwen die programmatische toegang tot AEM als Cloud Service vereisen, hebben een eenvoudige, snelle manier nodig om tijdelijke toegangstokens voor AEM te verkrijgen om lokale ontwikkelingsactiviteiten te vergemakkelijken. Om aan deze behoefte te voldoen, staat AEM Console van de Ontwikkelaar ontwikkelaars toe om tijdelijke toegangstokens zelf-te produceren die aan programmatically tot AEM kunnen worden gebruikt toegang te hebben.

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## Een token voor lokale ontwikkelingstoegang genereren

![Token voor lokale ontwikkelingstoegang ophalen](assets/local-development-access-token/getting-a-local-development-access-token.png)

Het token Local Development Access biedt toegang tot de services AEM Author and Publish als de gebruiker die het token heeft gegenereerd, samen met zijn machtigingen. Ondanks dat dit een ontwikkelingstoken is, deel dit teken niet, of opslag in broncontrole.

1. In [Adobe AdminConsole](https://adminconsole.adobe.com/) zorg ervoor u, de ontwikkelaar, een lid van bent:
   + __Cloud Manager -__ DeveloperIMS-productprofiel (verleent toegang tot AEM Developer Console)
   + Of de __AEM Beheerders__ of __AEM Gebruikers__ IMS Productprofiel voor de dienst van het AEM milieu zal het toegangstoken met integreren
   + Voor AEM als Cloud Service is alleen lidmaatschap vereist in de __AEM Beheerders__ of __AEM Gebruikers__ Productprofiel
1. Aanmelden bij [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Open het programma dat de AEM bevat als een Cloud Service-omgeving om te integreren met
1. Tik op de __ovaal__ naast de omgeving in de sectie __Omgevingen__ en selecteer __Developer Console__
1. Tik op de tab __Integraties__
1. Tik __Token voor lokale ontwikkeling ophalen__-knop
1. Tik op de __downloadknop__ in de linkerbovenhoek om het JSON-bestand met de waarde `accessToken` te downloaden en sla het JSON-bestand op een veilige locatie op uw ontwikkelcomputer op.
   + Dit is uw 24 uur, het toegangstoken van de ontwikkelaar aan het AEM als milieu van de Cloud Service.

![AEM Developer Console - Integratie - Token voor lokale ontwikkeling ophalen](./assets/local-development-access-token/developer-console.png)

## Gebruikt het Lokale Token van de Toegang van de Ontwikkeling{#use-local-development-access-token}

![Token voor toegang tot lokale ontwikkeling - externe toepassing](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Download het tijdelijke Local Development Access Token van AEM Developer Console
   + Het token Local Development Access verloopt elke 24 uur, zodat ontwikkelaars dagelijks nieuwe toegangstokens moeten downloaden
1. Een externe toepassing wordt ontwikkeld die programmatically met AEM als Cloud Service in wisselwerking staat
1. De externe toepassing leest in het Token van de Toegang van de Lokale Ontwikkeling
1. De externe toepassing construeert HTTP- verzoeken om als Cloud Service te AEM, toevoegend het Lokale Token van de Toegang van de Ontwikkeling als teken van de Drager aan de kopbal van de Vergunning van HTTP- verzoeken
1. AEM als Cloud Service ontvangt het HTTP- verzoek, verklaart het verzoek voor authentiek, en voert het werk uit dat door het HTTP- verzoek wordt gevraagd, en keert een reactie van HTTP terug naar de Externe Toepassing

### De externe voorbeeldtoepassing

We maken een eenvoudige externe JavaScript-toepassing om te illustreren hoe AEM via HTTPS via programmacode via HTTPS via het toegangstoken voor lokale ontwikkelaars kan worden benaderd. Dit illustreert hoe _om het even welk_ toepassing of systeem dat buiten AEM, ongeacht kader of taal loopt, het toegangstoken kan gebruiken programmatically voor authentiek verklaren aan, en toegang, AEM als Cloud Service. In [volgende sectie](./service-credentials.md) zullen wij deze toepassingscode bijwerken om de benadering te steunen voor het produceren van een teken voor productiegebruik.

Deze voorbeeldtoepassing wordt uitgevoerd vanaf de opdrachtregel en werkt metagegevens AEM elementen bij met behulp van AEM Assets HTTP-API&#39;s. Hierbij wordt de volgende stroom gebruikt:

1. Leest in parameters van de bevellijn (`getCommandLineParams()`)
1. Verkrijgt het toegangstoken wordt gebruikt om aan AEM als Cloud Service voor authentiek te verklaren (`getAccessToken(...)`)
1. Hiermee geeft u alle elementen in een map met AEM elementen weer die zijn opgegeven in parameters voor de opdrachtregel (`listAssetsByFolder(...)`)
1. De metagegevens van de weergegeven elementen bijwerken met de waarden die zijn opgegeven in de opdrachtregelparameters (`updateMetadata(...)`)

Het belangrijkste element in programmatically het voor authentiek verklaren aan AEM gebruikend het toegangstoken voegt een de verzoekkopbal van HTTP van de Vergunning aan alle HTTP- verzoeken toe die aan AEM worden gemaakt, in het volgende formaat:

+ `Authorization: Bearer ACCESS_TOKEN`

## De externe toepassing uitvoeren

1. Zorg ervoor dat [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) is geïnstalleerd op uw lokale ontwikkelcomputer, die zal worden gebruikt om de externe toepassing in werking te stellen
1. Download en decomprimeer de [voorbeeldtoepassing voor externe toepassingen](./assets/aem-guides_token-authentication-external-application.zip)
1. Van de bevellijn, in de omslag van dit project, looppas `npm install`
1. Kopieer [de Lokale Token van de Toegang van de Ontwikkeling ](#download-local-development-access-token) aan een dossier genoemd `local_development_token.json` in de wortel van het project
   + Maar vergeet niet dat u nooit aanmeldgegevens aan Git wilt toewijzen!
1. Open `index.js` en bekijk de code en opmerkingen van de externe toepassing.

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   Controleer de `fetch(..)` aanroepen in `listAssetsByFolder(...)` en `updateMetadata(...)`, en merk `headers` de `Authorization` HTTP- verzoekkopbal met een waarde van `Bearer ACCESS_TOKEN` bepalen. Zo wordt de HTTP-aanvraag die afkomstig is van de externe toepassing geverifieerd voor AEM als Cloud Service.

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   Om het even welke HTTP- verzoeken om als Cloud Service te AEM, moet het toegangstoken van de Drager in de kopbal van de Vergunning plaatsen. Herinner me, vereist elke AEM als milieu van de Cloud Service het eigen toegangstoken is. De toegangstoken van de ontwikkeling werkt niet op Stadium of Productie, zal het Stadium niet aan Ontwikkeling of Productie werken, en Productie zal niet op Ontwikkeling of Stadium werken!

1. Gebruikend de bevellijn, van de wortel van het project voert de toepassing uit, die in de volgende parameters overgaat:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   De volgende parameters worden doorgegeven:

   + `aem`: Het schema en de hostnaam van de AEM als een Cloud Service-omgeving waarmee de toepassing communiceert (bijvoorbeeld  `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: Het pad naar de map met middelen waarvan de elementen worden bijgewerkt met de  `propertyValue`bron; voeg het  `/content/dam` voorvoegsel NIET toe (bijv.  `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`: De elementeigenschapnaam die moet worden bijgewerkt, relatief ten opzichte van  `[dam:Asset]/jcr:content` (bijvoorbeeld  `metadata/dc:rights`).
   + `propertyValue`: De waarde waarop  `propertyName` moet worden ingesteld; Waarden met spaties moeten worden ingekapseld  `"` (bijv.  `"WKND Limited Use"`)
   + `file`: Het relatieve bestandspad naar het JSON-bestand dat is gedownload van AEM Developer Console.

   Een geslaagde uitvoering van de resultaten van de toepassing voor elk bijgewerkt element:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Metagegevens bijwerken in AEM controleren

Controleer of de metagegevens zijn bijgewerkt door u aan te melden bij de AEM als een Cloud Service-omgeving (zorg ervoor dat dezelfde host die wordt doorgegeven aan de opdrachtregelparameter `aem` wordt geopend).

1. Log in de AEM als een Cloud Service-omgeving waarmee de externe toepassing interactie heeft gehad (gebruik dezelfde host als in de opdrachtregelparameter `aem`)
1. Navigeer naar __Middelen__ > __Bestanden__
1. Navigeer in de elementenmap die wordt aangegeven door de opdrachtregelparameter `folder`, bijvoorbeeld __WKND__ > __English__ > __Adventures__ > __Napa Wine Tasting__
1. Open __Eigenschappen__ voor elk element (niet-inhoudsfragment) in de map
1. Tik op de tab __Geavanceerd__
1. Controleer de waarde van de bijgewerkte eigenschap, bijvoorbeeld __Copyright__ die is toegewezen aan de bijgewerkte JCR-eigenschap `metadata/dc:rights`, die de waarde weerspiegelt die wordt opgegeven in de parameter `propertyValue`, bijvoorbeeld __WKND Limited Use__

![WKND Update metagegevens beperkt gebruik](./assets/local-development-access-token/asset-metadata.png)

## Volgende stappen

Nu wij programmatically AEM als Cloud Service gebruikend het lokale ontwikkelingstoken hebben betreden, moeten wij de toepassing bijwerken om het gebruiken van de Verantwoordelijkheden van de Dienst te behandelen, zodat kan deze toepassing in een productiecontext worden gebruikt.

+ [Hoe te om de Referenties van de Dienst te gebruiken](./service-credentials.md)
