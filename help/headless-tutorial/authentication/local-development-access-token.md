---
title: Toegangstoken lokale ontwikkeling
description: AEM Local Development Access Tokens worden gebruikt om de ontwikkeling te versnellen van integraties met AEM as a Cloud Service die programmatisch communiceren met AEM Author of Publish services via HTTP.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 572
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# Toegangstoken lokale ontwikkeling

Ontwikkelaars die integraties bouwen die programmatische toegang tot AEM as a Cloud Service vereisen, hebben een eenvoudige, snelle manier nodig om tijdelijke toegangstokens voor AEM te verkrijgen om lokale ontwikkelingsactiviteiten te vergemakkelijken. Om aan deze behoefte te voldoen, staat AEM Developer Console ontwikkelaars toe om tijdelijke toegangstokens zelf te produceren die kunnen worden gebruikt om tot AEM programmatically toegang te hebben.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## Een token voor lokale ontwikkelingstoegang genereren

![ het krijgen van een Lokaal Token van de Toegang van de Ontwikkeling ](assets/local-development-access-token/getting-a-local-development-access-token.png)

Het token Local Development Access biedt toegang tot de services AEM Author and Publish als de gebruiker die het token heeft gegenereerd, samen met zijn machtigingen. Ondanks dat dit een ontwikkelingstoken is, deel dit teken niet, of opslag in broncontrole.

1. In [ Adobe Admin Console ](https://adminconsole.adobe.com/) verzekert u, de ontwikkelaar, een lid van zijn:
   + __Cloud Manager - het Profiel van het Product IMS van de Ontwikkelaar__ (verleent toegang tot AEM Developer Console)
   + Of de __Beheerders van AEM__ of __Gebruikers van AEM__ IMS Profiel van het Product voor de dienst van het milieu van AEM de toegangstoken integreert met
   + Het milieu van zandbak AEM as a Cloud Service vereist slechts lidmaatschap in of __de Beheerders van AEM__ of __de Gebruikers van AEM__ Profiel van het Product
1. Login aan [ Adobe Cloud Manager ](https://my.cloudmanager.adobe.com)
1. Open het programma dat de AEM as a Cloud Service-omgeving bevat om mee te integreren
1. Tik de __ellips__ naast het milieu in de __sectie van Milieu__, en selecteer __Developer Console__
1. Tik in het __lusje van de Integraties__
1. Tik het __Lokale teken__ lusje
1. Teken van de Ontwikkeling van het Teken van de Tik __van het Tik__ krijgen Lokale
1. Tik op de __downloadknoop__ in de hoogste-linkerhoek om het JSON dossier te downloaden dat `accessToken` waarde bevat, en sparen het JSON dossier aan een veilige plaats op uw ontwikkelingsmachine.
   + Dit is uw 24-uurs toegangstoken voor ontwikkelaars tot de AEM as a Cloud Service-omgeving.

![ AEM Developer Console - Integraties - krijg de Token van de Lokale Ontwikkeling ](./assets/local-development-access-token/developer-console.png)

## Gebruikt het Lokale Token van de Toegang van de Ontwikkeling{#use-local-development-access-token}

![ Token van de Toegang van de Lokale Ontwikkeling - Externe Toepassing ](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Download het tijdelijke Local Development Access Token van AEM Developer Console
   + De lokale Token van de Toegang van de Ontwikkeling verloopt om de 24 uur, zodat moeten de ontwikkelaars nieuwe toegangstokens dagelijks downloaden
1. Er wordt een externe toepassing ontwikkeld die programmatisch communiceert met AEM as a Cloud Service
1. De externe toepassing leest in het Token van de Toegang van de Lokale Ontwikkeling
1. De externe toepassing construeert HTTP- verzoeken aan AEM as a Cloud Service, die het Lokale Token van de Toegang van de Ontwikkeling als teken van de Drager aan de kopbal van de Vergunning van HTTP- verzoeken toevoegen
1. AEM as a Cloud Service ontvangt de HTTP-aanvraag, verifieert de aanvraag en voert het werk uit dat door de HTTP-aanvraag wordt aangevraagd en retourneert een HTTP-reactie terug naar de externe toepassing

### De externe voorbeeldtoepassing

Wij zullen een eenvoudige externe toepassing van JavaScript tot stand brengen om te illustreren hoe te om tot AEM as a Cloud Service over HTTPS programmatically toegang te hebben gebruikend het lokale toegangstoken van de ontwikkelaar. Dit illustreert hoe _om het even welke_ toepassing of systeem dat buiten AEM, ongeacht kader of taal loopt, het toegangstoken kan gebruiken programmatically voor authentiek te verklaren aan, en toegang te hebben, AEM as a Cloud Service. In de [ volgende sectie ](./service-credentials.md), zullen wij deze toepassingscode bijwerken om de benadering te steunen voor het produceren van een teken voor productiegebruik.

Deze voorbeeldtoepassing wordt uitgevoerd vanaf de opdrachtregel en werkt de metagegevens van AEM-elementen bij met behulp van AEM Assets HTTP API&#39;s. Hierbij wordt de volgende stroom gebruikt:

1. Leest in parameters van de bevellijn (`getCommandLineParams()`)
1. Verkrijgt het toegangstoken dat wordt gebruikt om aan AEM as a Cloud Service (`getAccessToken(...)`) voor authentiek te verklaren
1. Hiermee geeft u alle elementen in een AEM-elementmap weer die zijn opgegeven in een opdrachtregelparameter (`listAssetsByFolder(...)`)
1. De meta-gegevens van de vermelde activa met waarden bijwerken die in bevel-lijn parameters worden gespecificeerd (`updateMetadata(...)`)

Het belangrijkste element in programmatically het voor authentiek verklaren aan AEM gebruikend het toegangstoken is toevoegend een de verzoekkopbal van HTTP van de Vergunning aan alle HTTP- verzoeken die aan AEM worden gemaakt, in het volgende formaat:

+ `Authorization: Bearer ACCESS_TOKEN`

## De externe toepassing uitvoeren

1. Zorg ervoor dat [ Node.js ](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) op uw lokale ontwikkelingsmachine geïnstalleerd is, die wordt gebruikt om de externe toepassing in werking te stellen
1. De download en unzip de [ steekproef externe toepassing ](./assets/aem-guides_token-authentication-external-application.zip)
1. Voer vanuit de opdrachtregel in de map van dit project `npm install` uit
1. Kopieer [ het Lokale Token van de Toegang van de Ontwikkeling ](#download-local-development-access-token) aan een dossier genoemd `local_development_token.json` in de wortel van het project
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
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
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
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
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

   Controleer de `fetch(..)` aanroepen in de `listAssetsByFolder(...)` en `updateMetadata(...)` en `headers` definieer de `Authorization` HTTP-aanvraagheader met de waarde `Bearer ACCESS_TOKEN` . Zo wordt de HTTP-aanvraag van de externe toepassing geverifieerd bij AEM as a Cloud Service.

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

   Om het even welke HTTP- verzoeken aan AEM as a Cloud Service, moeten het toegangstoken van de Drager in de kopbal van de Vergunning plaatsen. Herinner me, vereist elke milieu van AEM as a Cloud Service zijn eigen toegangstoken. De toegangstoken van de ontwikkeling werkt niet op Stadium of Productie, het Stadium werkt niet aan Ontwikkeling of Productie, en Productie werkt niet op Ontwikkeling of Stadium!

1. Gebruikend de bevellijn, van de wortel van het project voert de toepassing uit, die in de volgende parameters overgaat:

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   De volgende parameters worden doorgegeven:

   + `aem`: Het schema en de hostnaam van de AEM as a Cloud Service-omgeving waarmee de toepassing werkt (bijvoorbeeld `https://author-p1234-e5678.adobeaemcloud.com` ).
   + `folder`: Het pad naar de elementenmap waarvan de elementen met `propertyValue` worden bijgewerkt; voeg het voorvoegsel `/content/dam` NIET toe (bijv. `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: De naam van de elementeigenschap die moet worden bijgewerkt, in verhouding tot `[dam:Asset]/jcr:content` (bijvoorbeeld `metadata/dc:rights` ).
   + `propertyValue`: De waarde waarop de `propertyName` moet worden ingesteld; waarden met spaties moeten worden ingekapseld met `"` (bijv. `"WKND Limited Use"`)
   + `file`: Het relatieve bestandspad naar het JSON-bestand dat is gedownload van AEM Developer Console.

   Een geslaagde uitvoering van de resultaten van de toepassing voor elk bijgewerkt element:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Update van metagegevens in AEM controleren

Controleer of de metagegevens zijn bijgewerkt door u aan te melden bij de AEM as a Cloud Service-omgeving (zorg ervoor dat dezelfde host die is doorgegeven aan de opdrachtregelparameter `aem` wordt benaderd).

1. Meld u aan bij de AEM as a Cloud Service-omgeving waarmee de externe toepassing interactie heeft gehad (gebruik dezelfde host als in de opdrachtregelparameter van `aem` )
1. Ga aan __Assets__ > __Dossiers__
1. Navigeer het de activaomslag die door de `folder` bevel-lijn parameter, bijvoorbeeld __wordt gespecificeerd WKND__ > __Engels__ > __avonturen__ > __het Tasting van de Wijn van Napa__
1. Open __Eigenschappen__ voor om het even welk (niet-Inhoud Fragment) activa in de omslag
1. Tik aan het __Geavanceerde__ lusje
1. Herzie de waarde van het bijgewerkte bezit, bijvoorbeeld __Copyright__ dat aan het bijgewerkte `metadata/dc:rights` bezit JCR in kaart wordt gebracht, dat op de waarde wijst die in de `propertyValue` parameter wordt verstrekt, bijvoorbeeld __Beperkt Gebruik WKND__

![ WKND de Beperkte Update van Metagegevens van het Gebruik ](./assets/local-development-access-token/asset-metadata.png)

## Volgende stappen

Nu we AEM as a Cloud Service via programmacode hebben benaderd met het token voor lokale ontwikkeling. Vervolgens moet de toepassing worden bijgewerkt zodat deze kan worden gebruikt in een productiecontext.

+ [Hoe te om de Referenties van de Dienst te gebruiken](./service-credentials.md)
