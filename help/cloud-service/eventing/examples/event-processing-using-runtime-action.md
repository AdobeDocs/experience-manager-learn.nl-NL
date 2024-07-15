---
title: AEM Gebeurtenissen verwerken met Adobe I/O Runtime Action
description: Leer hoe u ontvangen AEM Gebeurtenissen verwerkt met Adobe I/O Runtime Action.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 558
last-substantial-update: 2024-01-30T00:00:00Z
jira: KT-14879
thumbnail: KT-14879.jpeg
exl-id: c362011e-89e4-479c-9a6c-2e5caa3b6e02
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---

# AEM Gebeurtenissen verwerken met Adobe I/O Runtime Action

Leer hoe te om ontvangen AEM Gebeurtenissen te verwerken gebruikend [ Adobe I/O Runtime ](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Actie. Dit voorbeeld verbetert het vroegere voorbeeld [ Actie van Adobe I/O Runtime en AEM Gebeurtenissen ](runtime-action.md), zorg ervoor u het alvorens met dit hebt voltooid.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

In dit voorbeeld worden de oorspronkelijke gebeurtenisgegevens en de ontvangen gebeurtenis opgeslagen als een activiteitenbericht in Adobe I/O Runtime-opslag. Nochtans, als de gebeurtenis van _Gewijzigd het Fragment van de Inhoud_ type is, dan roept het ook AEM auteursdienst om de wijzigingsdetails te vinden. Tot slot worden de gebeurtenisdetails weergegeven in één paginatoepassing (SPA).

## Voorwaarden

U hebt het volgende nodig om deze zelfstudie te voltooien:

- Het milieu van AEM as a Cloud Service met [ toegelaten AEM Gebeurtenis ](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Ook, moet het steekproef ](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) project van de Plaatsen WKND [ worden opgesteld aan het.

- Toegang tot [ Adobe Developer Console ](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ Adobe Developer CLI ](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) geïnstalleerd op uw lokale machine.

- Lokaal geïnitialiseerd project van het vroegere voorbeeld [ de Actie van Adobe I/O Runtime en AEM Gebeurtenissen ](./runtime-action.md#initialize-project-for-local-development).

>[!IMPORTANT]
>
>AEM as a Cloud Service Event is alleen beschikbaar voor geregistreerde gebruikers in de pre-releasemodus. Om AEM gebeurtenis op uw milieu van AEM as a Cloud Service toe te laten, contacteer [ AEM-Toevend team ](mailto:grp-aem-events@adobe.com).

## Actie AEM Events-processor

In dit voorbeeld, voert de actie van de gebeurtenisbewerker [ ](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) volgende taken uit:

- Parseert ontvangen gebeurtenis in een activiteitenbericht.
- Als de ontvangen gebeurtenis van _Gewijzigd het Fragment van de Inhoud_ type is, vraag terug naar AEM auteursdienst om de wijzigingsdetails te vinden.
- Hiermee worden de oorspronkelijke gebeurtenisgegevens, het activiteitenbericht en eventuele wijzigingsgegevens in Adobe I/O Runtime-opslag gehandhaafd.

Om bovengenoemde taken uit te voeren, laten wij beginnen door een actie aan het project toe te voegen, JavaScript modules te ontwikkelen om de bovengenoemde taken uit te voeren, en tenslotte de actiecode bij te werken om de ontwikkelde modules te gebruiken.

Verwijs naar het in bijlage [ WKND-AEM-Eventing-Runtime-Action.zip ](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) dossier voor de volledige code, en onder sectie benadrukt de belangrijkste dossiers.

### Handeling toevoegen

- Voer de volgende opdracht uit om een handeling toe te voegen:

  ```bash
  aio app add action
  ```

- Selecteer `@adobe/generator-add-action-generic` als actiesjabloon en geef de handeling de naam `aem-event-processor` .

  ![ voeg actie ](../assets/examples/event-processing-using-runtime-action/add-action-template.png) toe

### JavaScript-modules ontwikkelen

Om de hierboven vermelde taken uit te voeren, ontwikkelen de volgende modules van JavaScript.

- De `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` module bepaalt als de ontvangen gebeurtenis van _Gewijzigd het Fragment van de Inhoud_ type is.

  ```javascript
  async function needsAEMCallback(aemEvent) {
  // create a Logger
  const logger = Core.Logger('eventValidator', {
      level: 'info',
  });
  
  let isValid = false;
  
  // verify the event is a Content Fragment Modified event
  if (
      aemEvent
      && aemEvent.ContentType === 'contentFragment'
      && aemEvent.EventName === 'modified'
  ) {
      logger.info('Processing Content Fragment Modified Event');
      isValid = true;
  }
  
  return isValid;
  }
  
  module.exports = needsAEMCallback;
  ```

- De module `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` roept AEM auteurservice aan om de wijzigingsdetails te vinden.

  ```javascript
  ...
  const auth = require('@adobe/jwt-auth');
  ...
  // Get AEM Service Credentials aka Technical Account details.
  // These are passed to the action as params and added in .env file.
  const clientId = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTID;
  const technicalAccountId = params.AEM_SERVICECREDENTIALS_ID;
  const orgId = params.AEM_SERVICECREDENTIALS_ORG;
  const clientSecret = params.AEM_SERVICECREDENTIALS_TECHNICALACCOUNT_CLIENTSECRET;
  // Private key is passed as a string with \n and \r characters escaped.
  const privateKey = params.AEM_SERVICECREDENTIALS_PRIVATEKEY.replace(
      /\\n/g,
      '\n',
  ).replace(/\\r/g, '\r');
  const metaScopes = params.AEM_SERVICECREDENTIALS_METASCOPES.split(',');
  const ims = `https://${params.AEM_SERVICECREDENTIALS_IMSENDPOINT}`;
  
  // Get the access token from IMS using Adobe I/O SDK
  const { access_token } = await auth({
      clientId,
      technicalAccountId,
      orgId,
      clientSecret,
      privateKey,
      metaScopes,
      ims,
  });
  ...
  // Call AEM Author service to get the CF details using AEM Assets API
  const res = await fetch(
      `${aemAuthorHost}${cfPath.replace('/content/dam/', '/api/assets/')}.json`,
  {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${access_token}`,
    },
  },
  );
  
  let newValuesOfCFPropertiesAddedOrModified = {};
  // If the response is OK, get the values of the CF properties that were added or modified
  if (res.ok) {
      logger.info('AEM Event Details loaded from AEM Author instance');
      const responseJSON = await res.json();
  
      // Get the values of the CF properties that were added or modified
      if (
      responseJSON
      && responseJSON.properties
      && responseJSON.properties.elements
      ) {
      const allCurrentCFProperties = responseJSON.properties.elements;
  
      newValuesOfCFPropertiesAddedOrModified = cfPropertiesAddedOrModified.map(
          (key) => ({
          key,
          value: allCurrentCFProperties[key],
          }),
      );
      }    
  }
  ...
  ```

  Verwijs naar [ AEM de zelfstudie van de Referenties van de Dienst ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en) om meer over het te leren. Ook, de [ Dossiers van de Configuratie van App Builder ](https://developer.adobe.com/app-builder/docs/guides/configuration/) voor het beheren van geheimen en actieparameters.

- In de module `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` worden de oorspronkelijke gebeurtenisgegevens, het activiteitenbericht en eventuele wijzigingsgegevens opgeslagen in Adobe I/O Runtime.

  ```javascript
  ...
  const filesLib = require('@adobe/aio-lib-files');
  ...
  
  const files = await filesLib.init();
  
  const eventDataAsJSON = JSON.stringify({
      activity: activityMessage,
      aemEvent,
      aemEventDetails,
  });
  
  // store details in a folder named YYYY-MM-DD and a file named <eventID>.json
  const bytesWritten = await files.write(
      `${formattedDate}/${aemEvent.getEventID()}.json`,
      eventDataAsJSON,
  );
  ...
  ```

### Handelingscode bijwerken

Werk ten slotte de actiecode bij in `src/dx-excshell-1/actions/aem-event-processor/index.js` om de ontwikkelde modules te gebruiken.

```javascript
...
// handle the challenge probe request, they are sent by I/O to verify the action is valid
if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
} else {
    logger.info('AEM Event request received');

    // create AEM Event object from request parameters
    const aemEvent = new AEMEvent(params);

    // get AEM Event as activity message using the helper method
    const activityMessage = aemEvent.getEventAsActivityMessage();

    // determine if AEM Event requires callback to AEM Author service
    const callbackAEMForEventDetails = await needsAEMCallback(aemEvent);

    let eventDetails = {};
    if (callbackAEMForEventDetails) {
    // call AEM Author service to get specifics about the event
    eventDetails = await loadEventDetailsFromAEMAuthorService(
        aemEvent,
        params,
    );
    }

    // store AEM Event and Event details in the file system
    const storageDetails = await storeEventData(
    activityMessage,
    aemEvent,
    eventDetails || {},
    );
    logger.info(`Storage details: ${JSON.stringify(storageDetails)}`);

    // create response message
    responseMsg = JSON.stringify({
    message: 'AEM Event processed',
    activityMessage,
    });

    // response object
    const response = {
    statusCode: 200,
    body: responseMsg,
    };
    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
}
...
```

## Aanvullende bronnen

- De map `src/dx-excshell-1/actions/model` bevat `aemEvent.js` - en `errors.js` -bestanden, die door de handeling worden gebruikt om respectievelijk de ontvangen gebeurtenis te parseren en fouten af te handelen.
- De map `src/dx-excshell-1/actions/load-processed-aem-events` bevat actiecode. Deze handeling wordt door de SPA gebruikt om de verwerkte AEM Events vanuit de Adobe I/O Runtime-opslag te laden.
- De map `src/dx-excshell-1/web-src` bevat de SPA code waarmee de verwerkte AEM Events wordt weergegeven.
- Het bestand `src/dx-excshell-1/ext.config.yaml` bevat actieconfiguratie en parameters.

## Concept en toetsaanslagen

De vereisten voor gebeurtenisverwerking verschillen van project tot project, maar de belangrijkste taken in dit voorbeeld zijn:

- De gebeurtenisverwerking kan worden uitgevoerd met Adobe I/O Runtime Action.
- De Runtime Actie kan met systemen zoals uw interne toepassingen, derdeoplossingen, en Adobe oplossingen communiceren.
- De runtimeactie dient als ingangspunt voor een bedrijfsproces dat rond een inhoudsverandering wordt ontworpen.
