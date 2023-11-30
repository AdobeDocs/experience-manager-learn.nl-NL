---
title: Toegangstoken lokale ontwikkeling
description: AEM de Tokens van de Toegang van de Lokale Ontwikkeling worden gebruikt om de ontwikkeling van integratie met AEM as a Cloud Service te versnellen die programmatically met AEM de Diensten van de Auteur of van de Publicatie over HTTP in wisselwerking staan.
version: Cloud Service
topics: Development, Security
feature: APIs
activity: develop
audience: developer
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# Toegangstoken lokale ontwikkeling

Ontwikkelaars die integraties bouwen die programmatische toegang tot AEM as a Cloud Service vereisen hebben een eenvoudige, snelle manier nodig om tijdelijke toegangstokens voor AEM te verkrijgen om lokale ontwikkelingsactiviteiten te vergemakkelijken. Om aan deze behoefte te voldoen, staat AEM Console van de Ontwikkelaar ontwikkelaars toe om tijdelijke toegangstokens zelf-te produceren die aan programmatically tot AEM kunnen worden gebruikt toegang te hebben.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## Een token voor lokale ontwikkelingstoegang genereren

![Token voor lokale ontwikkelingstoegang ophalen](assets/local-development-access-token/getting-a-local-development-access-token.png)

Het token Local Development Access biedt toegang tot AEM services Auteur en Publiceren als de gebruiker die het token heeft gegenereerd, samen met zijn machtigingen. Ondanks dat dit een ontwikkelingstoken is, deel dit teken niet, of opslag in broncontrole.

1. In [Adobe Admin Console](https://adminconsole.adobe.com/) zorgt ervoor dat u, de ontwikkelaar, lid bent van:
   + __Cloud Manager - Ontwikkelaar__ IMS-productprofiel (verleent toegang tot AEM Developer Console)
   + Of de __AEM__ of __AEM__ IMS-productprofiel voor de service van de AEM-omgeving waarin het toegangstoken kan worden geïntegreerd
   + Sandbox AEM as a Cloud Service omgeving vereist alleen lidmaatschap van __AEM__ of __AEM__ Productprofiel
1. Aanmelden bij [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Open het programma dat de AEM as a Cloud Service omgeving bevat om mee te integreren
1. Tik op de knop __weglatingsteken__ naast het milieu in de __Omgevingen__ en selecteert u __Ontwerpconsole__
1. Tik in het dialoogvenster __Integraties__ tab
1. Tik op de knop __Lokaal token__ tab
1. Tikken __Token voor lokale ontwikkeling ophalen__ knop
1. Tik op de knop __downloadknop__ in de linkerbovenhoek om het JSON-bestand met `accessToken` en sla het JSON-bestand op een veilige locatie op uw ontwikkelcomputer op.
   + Dit is uw 24 uur, toegangstoken voor ontwikkelaars tot het AEM as a Cloud Service milieu.

![AEM Developer Console - Integratie - Token voor lokale ontwikkeling ophalen](./assets/local-development-access-token/developer-console.png)

## Gebruikt het Lokale Token van de Toegang van de Ontwikkeling{#use-local-development-access-token}

![Token voor toegang tot lokale ontwikkeling - externe toepassing](assets/local-development-access-token/local-development-access-token-external-application.png)

1. Download het tijdelijke Local Development Access Token van AEM Developer Console
   + De lokale Token van de Toegang van de Ontwikkeling verloopt om de 24 uur, zodat moeten de ontwikkelaars nieuwe toegangstokens dagelijks downloaden
1. Er wordt een externe toepassing ontwikkeld die programmatisch werkt met AEM as a Cloud Service
1. De externe toepassing leest in het Token van de Toegang van de Lokale Ontwikkeling
1. De externe toepassing construeert HTTP- verzoeken om as a Cloud Service te AEM, toevoegend het Lokale Token van de Toegang van de Ontwikkeling als teken van de Drager aan de kopbal van de Vergunning van HTTP- verzoeken
1. AEM as a Cloud Service ontvangt het HTTP- verzoek, verklaart het verzoek voor authentiek, en voert het werk uit dat door het HTTP- verzoek wordt gevraagd en keert een reactie van HTTP terug naar de Externe Toepassing

### De externe voorbeeldtoepassing

We maken een eenvoudige externe JavaScript-toepassing om te illustreren hoe AEM as a Cloud Service via HTTPS via het toegangstoken voor lokale ontwikkelaars programmatisch kan worden benaderd. Dit illustreert hoe _alle_ toepassing of systeem dat buiten AEM, ongeacht kader of taal loopt, kan het toegangstoken gebruiken programmatically voor authentiek verklaren aan, en toegang, AEM as a Cloud Service. In de [volgende sectie](./service-credentials.md), zullen wij deze toepassingscode bijwerken om de benadering te steunen om een token voor productiegebruik te produceren.

Deze voorbeeldtoepassing wordt uitgevoerd vanaf de opdrachtregel en werkt metagegevens AEM elementen bij met behulp van AEM Assets HTTP-API&#39;s. Hierbij wordt de volgende stroom gebruikt:

1. Leest in parameters vanaf de opdrachtregel (`getCommandLineParams()`)
1. Hiermee verkrijgt u het toegangstoken dat wordt gebruikt voor verificatie bij AEM as a Cloud Service (`getAccessToken(...)`)
1. Hiermee worden alle elementen in een AEM elementmap weergegeven die in een opdrachtregelparameter zijn opgegeven (`listAssetsByFolder(...)`)
1. De metagegevens van de weergegeven elementen bijwerken met de waarden die zijn opgegeven in opdrachtregelparameters (`updateMetadata(...)`)

Het belangrijkste element in programmatically het voor authentiek verklaren aan AEM gebruikend het toegangstoken voegt een de verzoekkopbal van HTTP van de Vergunning aan alle HTTP- verzoeken toe die aan AEM worden gemaakt, in het volgende formaat:

+ `Authorization: Bearer ACCESS_TOKEN`

## De externe toepassing uitvoeren

1. Zorg ervoor dat [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) is geïnstalleerd op uw lokale ontwikkelcomputer, die wordt gebruikt om de externe toepassing uit te voeren
1. Download en decomprimeer de [externe voorbeeldtoepassing](./assets/aem-guides_token-authentication-external-application.zip)
1. Vanaf de bevellijn, in de omslag van dit project, looppas `npm install`
1. De [Het token Local Development Access gedownload](#download-local-development-access-token) naar een bestand met de naam `local_development_token.json` in de hoofdmap van het project
   + Maar vergeet niet dat u nooit aanmeldgegevens aan Git wilt toewijzen!
1. Openen `index.js` en bekijk de externe toepassingscode en opmerkingen.

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

   Controleer de `fetch(..)` oproepen in de `listAssetsByFolder(...)` en `updateMetadata(...)`, en `headers` de `Authorization` HTTP-aanvraagheader met een waarde van `Bearer ACCESS_TOKEN`. Op deze manier wordt de HTTP-aanvraag die afkomstig is van de externe toepassing geverifieerd op AEM as a Cloud Service.

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

   Om het even welke HTTP- verzoeken om as a Cloud Service te AEM, moet het toegangstoken van de Drager in de kopbal van de Vergunning plaatsen. Herinner me, vereist elk AEM as a Cloud Service milieu zijn eigen toegangstoken. De toegangstoken van de ontwikkeling werkt niet op Stadium of Productie, het Stadium werkt niet aan Ontwikkeling of Productie, en Productie werkt niet op Ontwikkeling of Stadium!

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

   + `aem`: Het schema en de hostnaam van de AEM as a Cloud Service omgeving waarmee de toepassing communiceert (bijvoorbeeld `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: Het pad naar de elementenmap waarvan de elementen met het `propertyValue`; voeg GEEN `/content/dam` voorvoegsel (bijvoorbeeld `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: De elementeigenschapnaam die moet worden bijgewerkt, relatief ten opzichte van `[dam:Asset]/jcr:content` (bv. `metadata/dc:rights`).
   + `propertyValue`: De waarde die moet worden ingesteld voor de `propertyName` to; waarden met spaties moeten worden ingekapseld `"` (bv. `"WKND Limited Use"`)
   + `file`: Het relatieve bestandspad naar het JSON-bestand dat is gedownload van AEM Developer Console.

   Een geslaagde uitvoering van de resultaten van de toepassing voor elk bijgewerkt element:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### Metagegevens bijwerken in AEM controleren

Controleer of de metagegevens zijn bijgewerkt door u aan te melden bij de AEM as a Cloud Service omgeving (zorg ervoor dat dezelfde host is doorgegeven aan de `aem` opdrachtregelparameter wordt benaderd).

1. Meld u aan bij de AEM as a Cloud Service omgeving waarmee de externe toepassing interactie heeft gehad (gebruik dezelfde host als in het dialoogvenster `aem` opdrachtregelparameter)
1. Ga naar de __Activa__ > __Bestanden__
1. Navigeren in de elementenmap die is opgegeven in het dialoogvenster `folder` opdrachtregelparameter, bijvoorbeeld __WKND__ > __Engels__ > __avonturen__ > __Napa Wine Tasting__
1. Open de __Eigenschappen__ voor elk element (niet-Content Fragment) in de map
1. Tik op de knop __Geavanceerd__ tab
1. Controleer de waarde van de bijgewerkte eigenschap, bijvoorbeeld __Copyright__ die is toegewezen aan de bijgewerkte `metadata/dc:rights` JCR-eigenschap, die de waarde weergeeft die in de `propertyValue` parameter, bijvoorbeeld __Beperkt gebruik WKND__

![WKND Update metagegevens beperkt gebruik](./assets/local-development-access-token/asset-metadata.png)

## Volgende stappen

Nu wij programmatically hebben betreden AEM as a Cloud Service gebruikend het lokale ontwikkelingstoken. Vervolgens moet de toepassing worden bijgewerkt zodat deze kan worden gebruikt in een productiecontext.

+ [Hoe te om de Referenties van de Dienst te gebruiken](./service-credentials.md)
