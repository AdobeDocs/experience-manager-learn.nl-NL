---
title: AEM Assets-gebeurtenissen voor PIM-integratie
description: Leer hoe u AEM Assets en PIM-systemen (Product Information Management) kunt integreren voor updates van metagegevens van bedrijfsmiddelen.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 761
last-substantial-update: 2024-02-13T00:00:00Z
jira: KT-14901
thumbnail: KT-14901.jpeg
exl-id: 070cbe54-2379-448b-bb7d-3756a60b65f0
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1116'
ht-degree: 0%

---

# AEM Assets-gebeurtenissen voor PIM-integratie

>[!IMPORTANT]
>
>Deze zelfstudie maakt gebruik van experimentele AEM as a Cloud Service API&#39;s. Om toegang te krijgen tot deze API&#39;s, moet u een pre-release softwareovereenkomst accepteren en deze API&#39;s handmatig voor uw omgeving laten inschakelen door Adobe engineering. Om toegang te verzoeken bereiken tot de steun van de Adobe.

Leer hoe u AEM Assets kunt integreren met een systeem van derden, zoals een PIM-systeem (Product Information Management) of een PLM-systeem (Product Line Management), om metagegevens van bedrijfsmiddelen bij te werken **native AEM-IO-gebeurtenissen gebruiken**. Als u een AEM Assets-gebeurtenis ontvangt, kunnen de metagegevens van de elementen op basis van de zakelijke vereisten worden bijgewerkt in AEM, de PIM of beide systemen. In dit voorbeeld wordt echter getoond hoe de metagegevens van de elementen in AEM worden bijgewerkt.

>[!VIDEO](https://video.tv.adobe.com/v/3427592?quality=12&learn=on)

De update voor metagegevens van elementen uitvoeren **code buiten AEM** de [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/), wordt een serverloos platform gebruikt.

De stroom voor gebeurtenisverwerking is als volgt:

![AEM Assets-gebeurtenissen voor PIM-integratie](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. De AEM-auteur activeert een _Middelverwerking voltooid_ gebeurtenis wanneer een asset upload is voltooid en alle activiteiten op het gebied van de verwerking van activa zijn voltooid. Wachten tot de verwerking is voltooid, zorgt ervoor dat alle verwerking buiten de verpakking, zoals het extraheren van metagegevens, is voltooid.
1. De gebeurtenis wordt verzonden naar de [Adobe I/O Events](https://developer.adobe.com/events/) service.
1. De dienst van de Gebeurtenissen van de Adobe I/O gaat de gebeurtenis tot over [Adobe I/O Runtime-actie](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) voor verwerking.
1. De actie van Adobe I/O Runtime roept API van het PIM systeem om extra meta-gegevens zoals SKU, leveranciersinformatie, of andere details terug te winnen.
1. De aanvullende metagegevens die uit de PIM zijn opgehaald, worden vervolgens in AEM Assets bijgewerkt met de functie [API voor middelenauteur](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

## Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

- as a Cloud Service omgeving AEM met [AEM Event ingeschakeld](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Ook het voorbeeld [WKND-sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) het project moet worden uitgevoerd .

- Toegang tot [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ADOBE DEVELOPER CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) op uw lokale computer is geïnstalleerd.

## Ontwikkelingsstappen

De ontwikkelingsstappen op hoog niveau zijn:

1. [Een project maken in de Adobe Developer Console (ADC)](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [Initialiseer het project voor lokale ontwikkeling](./runtime-action.md#initialize-project-for-local-development)
1. Vorm het project in ADC
1. Vorm de AEM dienst van de Auteur om ADC projectmededeling toe te laten
1. Een runtimeactie ontwikkelen die het ophalen en bijwerken van metagegevens ordent
1. Middelen uploaden naar de AEM Auteur-service en controleren of de metagegevens zijn bijgewerkt

Raadpleeg voor meer informatie over de stappen 1-2 de [Adobe I/O Runtime Action and AEM Events](./runtime-action.md#) en voor stap 3-6 verwijst u naar de volgende secties.

### Het project configureren in Adobe Developer Console (ADC)

Als u AEM Assets Events wilt ontvangen en de Adobe I/O Runtime-actie wilt uitvoeren die in de vorige stap is gemaakt, configureert u het project in ADC.

- Ga in ADC naar de [project](https://developer.adobe.com/console/projects). Selecteer de `Stage` werkruimte, dit is waar de runtime actie werd opgesteld.

- Klik op de knop **Service toevoegen** en selecteert u de **Gebeurtenis** -optie. In de **Gebeurtenissen toevoegen** dialoogvenster, selecteren **Experience Cloud** > **AEM Assets** en klik op **Volgende**. Voer aanvullende configuratiestappen uit en selecteer AEMCS-instantie. _Middelverwerking voltooid_ gebeurtenis, OAuth Server-aan-Server authentificatietype, en andere details.

  ![AEM Assets Event - add event](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- Tot slot, in het **Hoe kan ik gebeurtenissen ontvangen** stap, uitbreiden **Runtime, actie** en selecteert u de _algemeen_ handeling die in de vorige stap is gemaakt. Klikken **geconfigureerde gebeurtenissen opslaan**.

  ![AEM Assets Event - receive event](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- Op dezelfde manier klikt u op de knop **Service toevoegen** en selecteert u de **API** -optie. In de **Een API toevoegen** modal, selecteer **Experience Cloud** > **AS A CLOUD SERVICE API AEM** en klik op **Volgende**.

  ![AEM as a Cloud Service API toevoegen - Project configureren](../assets/examples/assets-pim-integration/add-aem-api.png)

- Selecteer vervolgens **OAuth Server-to-Server** voor verificatietype en klik op **Volgende**.

- Selecteer vervolgens de optie **Beheerders AEM-XXX** productprofiel en klik op **geconfigureerde API opslaan**. Als u het desbetreffende element wilt bijwerken, moet het geselecteerde productprofiel zijn gekoppeld aan de AEM Assets-omgeving waaruit de gebeurtenis wordt geproduceerd en moet het voldoende toegang hebben om elementen in die omgeving bij te werken.

  ![AEM as a Cloud Service API toevoegen - Project configureren](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

### Vorm AEM de dienst van de Auteur om ADC projectmededeling toe te laten

Om de activa meta-gegevens in AEM van het bovengenoemde project van ADC bij te werken, vorm AEM de dienst van de Auteur met cliënt identiteitskaart van het project ADC. De _client-id_ wordt toegevoegd als omgevingsvariabele met behulp van [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html#add-variables) UI.

- Aanmelden bij [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/), selecteert u **Programma** > **Omgeving** > **Weglatingsteken** > **Details weergeven** > **Configuratie** tab.

  ![Adobe Cloud Manager - Omgevingsconfiguratie](../assets/examples/assets-pim-integration/cloud-manager-environment-configuration.png)

- Vervolgens **Configuratie toevoegen** en voert u de variabeledetails in als

  | Naam | Waarde | AEM | Type |
  | ----------- | ----------- | ----------- | ----------- |
  | ADOBE_PROVIDED_CLIENT_ID | &lt;COPY_FROM_ADC_PROJECT_CREDENTIALS> | Auteur | Variabele |

  ![Adobe Cloud Manager - Omgevingsconfiguratie](../assets/examples/assets-pim-integration/add-environment-variable.png)

- Klikken **Toevoegen** en **Opslaan** de configuratie.

### Handeling bij uitvoering ontwikkelen

Als u de metagegevens wilt ophalen en bijwerken, begint u met het bijwerken van de automatisch gemaakte _algemeen_ actiecode in `src/dx-excshell-1/actions/generic` map.

Zie de bijgevoegde [WKND-Assets-PIM-Integration.zip](../assets/examples/assets-pim-integration/WKND-Assets-PIM-Integration.zip) voor de volledige code, en onder sectie benadrukt de belangrijkste dossiers.

- De `src/dx-excshell-1/actions/generic/mockPIMCommunicator.js` het dossier controleert de vraag PIM API om extra meta-gegevens zoals SKU en leveranciersnaam terug te winnen. Dit bestand wordt gebruikt voor demo-doeleinden. Zodra u de stroom van begin tot eind het werken hebt, vervang deze functie met een vraag aan uw echt PIM systeem om meta-gegevens voor de activa terug te winnen.

  ```javascript
  /**
   * Mock PIM API to get the product data such as SKU, Supplier, etc.
   *
   * In a real-world scenario, this function would call the PIM API to get the product data.
   * For this example, we are returning mock data.
   *
   * @param {string} assetId - The assetId to get the product data.
   */
  module.exports = {
      async getPIMData(assetId) {
          if (!assetId) {
          throw new Error('Invalid assetId');
          }
          // Mock response data for demo purposes
          const data = {
          SKUID: 'MockSKU 123',
          SupplierName: 'mock-supplier',
          // ... other product data
          };
          return data;
      },
  };
  ```

- De `src/dx-excshell-1/actions/generic/aemCommunicator.js` het bestand werkt de metagegevens van het element bij in AEM met behulp van de [API voor middelenauteur](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/).

  ```javascript
  const fetch = require('node-fetch');
  
  ...
  
  /**
  *  Get IMS Access Token using Client Credentials Flow
  *
  * @param {*} clientId - IMS Client ID from ADC project's OAuth Server-to-Server Integration
  * @param {*} clientSecret - IMS Client Secret from ADC project's OAuth Server-to-Server Integration
  * @param {*} scopes - IMS Meta Scopes from ADC project's OAuth Server-to-Server Integration as comma separated strings
  * @returns {string} - Returns the IMS Access Token
  */
  async function getIMSAccessToken(clientId, clientSecret, scopes) {
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';
  
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
    };
  
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();
  
    return responseJSON.access_token;
  }    
  
  async function updateAEMAssetMetadata(metadataDetails, aemAssetEvent, params) {
    ...
    // Transform the metadata details to JSON Patch format,
    // see https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/patchAssetMetadata
    const transformedMetadata = Object.keys(metadataDetails).map((key) => ({
      op: 'add',
      path: `wknd-${key.toLowerCase()}`,
      value: metadataDetails[key],
    }));
  
    ...
  
    // Get ADC project's OAuth Server-to-Server Integration credentials
    const clientId = params.ADC_CECREDENTIALS_CLIENTID;
    const clientSecret = params.ADC_CECREDENTIALS_CLIENTSECRET;
    const scopes = params.ADC_CECREDENTIALS_METASCOPES;
  
    // Get IMS Access Token using Client Credentials Flow
    const access_token = await getIMSAccessToken(clientId, clientSecret, scopes);
  
    // Call AEM Author service to update the metadata using Assets Author API
    // See https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/
    const res = await fetch(`${aemAuthorHost}/adobe/assets/${assetId}/metadata`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json-patch+json',
        'If-Match': '*',
        'X-Adobe-Accept-Experimental': '1',
        'X-Api-Key': 'aem-assets-management-api', // temporary value
        Authorization: `Bearer ${access_token}`,
      },
      body: JSON.stringify(transformedMetadata),
    });
  
    ...
  }
  
  module.exports = { updateAEMAssetMetadata };
  ```

  De `.env` het dossier slaat de Server-aan-Server van het ADC-project geloofsbrieven details op, en zij worden overgegaan als parameters tot de actie gebruikend `ext.config.yaml` bestand. Zie de [Configuratiebestanden van App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) voor het beheren van geheimen en actieparameters.

- De `src/dx-excshell-1/actions/model` map bevat `aemAssetEvent.js` en `errors.js` bestanden, die door de handeling worden gebruikt om respectievelijk de ontvangen gebeurtenis te parseren en fouten te verwerken.

- De `src/dx-excshell-1/actions/generic/index.js` het dossier gebruikt de eerder vermelde modules om de meta-gegevens terug te winnen en bij te werken.

  ```javascript
  ...
  
  let responseMsg;
  // handle the challenge probe request, they are sent by I/O to verify the action is valid
  if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
  } else {
    logger.info('AEM Asset Event request received');
  
    // create AEM Asset Event object from request parameters
    const aemAssetEvent = new AEMAssetEvent(params);
  
    // Call mock PIM API to get the product data such as SKU, Supplier, etc.
    const mockPIMData = await mockPIMAPI.getPIMData(
      aemAssetEvent.getAssetName(),
    );
    logger.info('Mock PIM API response', mockPIMData);
  
    // Update PIM received data in AEM as Asset metadata
    const aemUpdateStatus = await updateAEMAssetMetadata(
      mockPIMData,
      aemAssetEvent,
      params,
    );
    logger.info('AEM Asset metadata update status', aemUpdateStatus);
  
    if (aemUpdateStatus) {
      // create response message
      responseMsg = JSON.stringify({
        message:
          'AEM Asset Event processed successfully, updated the asset metadata with PIM data.',
        assetdata: {
          assetName: aemAssetEvent.getAssetName(),
          assetPath: aemAssetEvent.getAssetPath(),
          assetId: aemAssetEvent.getAssetId(),
          aemHost: aemAssetEvent.getAEMHost(),
          pimdata: mockPIMData,
        },
      });
    } 
  
    // response object
    const response = {
      statusCode: 200,
      body: responseMsg,
    };
  
    // Return the response to the caller
    return response;
  
    ...
  }
  ```

Voer de bijgewerkte handeling uit naar Adobe I/O Runtime met de volgende opdracht:

```bash
$ aio app deploy
```

### Uploaden van middelen en verificatie van metagegevens

Voer de volgende stappen uit om de integratie met AEM Assets en PIM te controleren:

- Als u de door PIM verschafte metagegevens zoals SKU en Naam leverancier wilt bekijken, maakt u een metagegevensschema in AEM Assets. Zie [Metagegevensschema](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/configuring/metadata-schemas.html) die de eigenschappen van de metagegevens van de SKU- en leveranciersnaam weergeeft.

- Upload een element in AEM Auteur-service en controleer de update van de metagegevens.

  ![Update voor AEM Assets-metagegevens](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## Concept en toetsaanslagen

De synchronisatie van de metagegevens van bedrijfsmiddelen tussen AEM en andere systemen, zoals PIM, is vaak vereist in de onderneming. Met AEM Eventing kunnen dergelijke vereisten worden bereikt.

- De code voor het ophalen van metagegevens van elementen wordt buiten AEM uitgevoerd, waarbij het laden op AEM service Auteur wordt vermeden. Hierdoor wordt gebeurtenisgestuurde architectuur die onafhankelijk van elkaar wordt geschaald, geschaald.
- De nieuwe API voor middelenauteur wordt gebruikt om de metagegevens van elementen in AEM bij te werken.
- De API-verificatie gebruikt OAuth server-to-server (ook wel clientverificatiestroom genoemd), zie [OAuth Server-to-Server referentie implementatiegids](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/).
- In plaats van Adobe I/O Runtime-handelingen kunnen andere websites of Amazon EventBridge worden gebruikt om de AEM Assets-gebeurtenis te ontvangen en de update van de metagegevens te verwerken.
- Asset Events via AEM Event stellen bedrijven in staat om kritieke processen te automatiseren en te stroomlijnen, waardoor de efficiëntie en de coherentie tussen het ecosysteem van de inhoud worden bevorderd.
