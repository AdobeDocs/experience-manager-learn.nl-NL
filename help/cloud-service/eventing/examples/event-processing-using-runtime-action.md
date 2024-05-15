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

Leer hoe u ontvangen AEM gebeurtenissen verwerkt met [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) Actie. In dit voorbeeld wordt het eerdere voorbeeld verbeterd [Adobe I/O Runtime Action and AEM Events](runtime-action.md), moet u controleren of u het document hebt voltooid voordat u verdergaat met deze versie.

>[!VIDEO](https://video.tv.adobe.com/v/3427054?quality=12&learn=on)

In dit voorbeeld worden de oorspronkelijke gebeurtenisgegevens en de ontvangen gebeurtenis opgeslagen als een activiteitenbericht in Adobe I/O Runtime-opslag. Als de gebeurtenis echter _Inhoudsfragment gewijzigd_ type, dan roept het ook AEM auteursdienst om de wijzigingsdetails te vinden. Tot slot worden de gebeurtenisdetails weergegeven in één paginatoepassing (SPA).

## Voorwaarden

U hebt het volgende nodig om deze zelfstudie te voltooien:

- as a Cloud Service omgeving AEM met [AEM Event ingeschakeld](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment). Ook het voorbeeld [WKND-sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) het project moet worden uitgevoerd .

- Toegang tot [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/).

- [ADOBE DEVELOPER CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) op uw lokale computer is geïnstalleerd.

- Lokaal geïnitialiseerd project uit het vorige voorbeeld [Adobe I/O Runtime Action and AEM Events](./runtime-action.md#initialize-project-for-local-development).

>[!IMPORTANT]
>
>AEM as a Cloud Service gebeurtenis is alleen beschikbaar voor geregistreerde gebruikers in de pre-releasemodus. Om AEM gebeurtenis op uw AEM as a Cloud Service milieu toe te laten, contacteer [AEM-team](mailto:grp-aem-events@adobe.com).

## Actie AEM Events-processor

In dit voorbeeld, de gebeurtenisbewerker [action](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) voert de volgende taken uit:

- Parseert ontvangen gebeurtenis in een activiteitenbericht.
- Indien ontvangen gebeurtenis van _Inhoudsfragment gewijzigd_ type, vraag terug aan AEM auteursdienst om de wijzigingsdetails te vinden.
- Hiermee worden de oorspronkelijke gebeurtenisgegevens, het activiteitenbericht en eventuele wijzigingsgegevens in Adobe I/O Runtime-opslag gehandhaafd.

Om bovengenoemde taken uit te voeren, laten wij beginnen door een actie aan het project toe te voegen, modules te ontwikkelen JavaScript om de bovengenoemde taken uit te voeren, en tenslotte de actiecode bij te werken om de ontwikkelde modules te gebruiken.

Zie de bijgevoegde [WKND-AEM-Event-Runtime-Action.zip](../assets/examples/event-processing-using-runtime-action/WKND-AEM-Eventing-Runtime-Action.zip) voor de volledige code, en onder sectie benadrukt de belangrijkste dossiers.

### Handeling toevoegen

- Voer de volgende opdracht uit om een handeling toe te voegen:

  ```bash
  aio app add action
  ```

- Selecteren `@adobe/generator-add-action-generic` als actiesjabloon de handeling een naam geven `aem-event-processor`.

  ![Handeling toevoegen](../assets/examples/event-processing-using-runtime-action/add-action-template.png)

### JavaScript-modules ontwikkelen

Om de hierboven vermelde taken uit te voeren, ontwikkelen de volgende modules JavaScript.

- De `src/dx-excshell-1/actions/aem-event-processor/eventValidator.js` bepaalt als de ontvangen gebeurtenis van is _Inhoudsfragment gewijzigd_ type.

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

- De `src/dx-excshell-1/actions/aem-event-processor/loadEventDetailsFromAEM.js` de modulevraag AEM de auteursdienst om de wijzigingsdetails te vinden.

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

  Zie [Zelfstudie AEM serviceresulentie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en) voor meer informatie. Ook de [Configuratiebestanden van App Builder](https://developer.adobe.com/app-builder/docs/guides/configuration/) voor het beheren van geheimen en actieparameters.

- De `src/dx-excshell-1/actions/aem-event-processor/storeEventData.js` slaat de oorspronkelijke gebeurtenisgegevens, het activiteitenbericht en eventuele wijzigingsdetails (indien van toepassing) op in Adobe I/O Runtime-opslag.

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

Werk ten slotte de actiecode bij op `src/dx-excshell-1/actions/aem-event-processor/index.js` de ontwikkelde modules te gebruiken.

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

- De `src/dx-excshell-1/actions/model` map bevat `aemEvent.js` en `errors.js` bestanden, die door de handeling worden gebruikt om respectievelijk de ontvangen gebeurtenis te parseren en fouten te verwerken.
- De `src/dx-excshell-1/actions/load-processed-aem-events` Deze handeling wordt door de SPA gebruikt om de verwerkte AEM Events vanuit Adobe I/O Runtime-opslag te laden.
- De `src/dx-excshell-1/web-src` Deze map bevat de SPA code waarmee de verwerkte AEM Events worden weergegeven.
- De `src/dx-excshell-1/ext.config.yaml` bestand bevat actieconfiguratie en parameters.

## Concept en toetsaanslagen

De vereisten voor gebeurtenisverwerking verschillen van project tot project, maar de belangrijkste taken in dit voorbeeld zijn:

- De gebeurtenisverwerking kan worden uitgevoerd met Adobe I/O Runtime Action.
- De Runtime Actie kan met systemen zoals uw interne toepassingen, derdeoplossingen, en Adobe oplossingen communiceren.
- De runtimeactie dient als ingangspunt voor een bedrijfsproces dat rond een inhoudsverandering wordt ontworpen.
