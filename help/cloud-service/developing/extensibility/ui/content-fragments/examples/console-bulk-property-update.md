---
title: Bulkeigenschap bijwerken voorbeeld AEM Content Fragment Console-extensie
description: Een voorbeeld van een AEM Content Fragments Console-extensie die bulksgewijs een eigenschap van Content Fragments bijwerkt.
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11604
thumbnail: KT-11604.png
doc-type: article
last-substantial-update: 2024-01-26T00:00:00Z
exl-id: fbfb5c10-95f8-4875-88dd-9a941d7a16fd
duration: 1362
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 0%

---

# Bulkeigenschap bijwerken, voorbeeldextensie

>[!VIDEO](https://video.tv.adobe.com/v/3454459?captions=dut&quality=12&learn=on)

Dit voorbeeld AEM de uitbreiding van de Console van het Fragment van de Inhoud is een [&#x200B; uitbreiding van de actiebar &#x200B;](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) die bulkupdates een bezit van het Fragment van de Inhoud aan een gemeenschappelijke waarde.

De functionele stroom van de voorbeeldextensie is als volgt:

![&#x200B; de actiestroom van Adobe I/O Runtime &#x200B;](./assets/bulk-property-update/flow.png){align="center"}

1. Selecteer de Fragmenten van de Inhoud en het klikken van de knoop van de uitbreiding in de [&#x200B; actiebar &#x200B;](#extension-registration) opent [&#x200B; modaal &#x200B;](#modal).
2. [&#x200B; modaal &#x200B;](#modal) toont een vorm van de douanetoevoer die met [&#x200B; wordt gebouwd Reageer Spectrum &#x200B;](https://react-spectrum.adobe.com/react-spectrum/).
3. Het voorleggen van de vorm verzendt de lijst van geselecteerde Fragmenten van de Inhoud, en de gastheer van AEM naar de [&#x200B; actie van douaneAdobe I/O Runtime &#x200B;](#adobe-io-runtime-action).
4. De [&#x200B; actie van Adobe I/O Runtime &#x200B;](#adobe-io-runtime-action) bevestigt de input en doet de verzoeken van HTTP PUT aan AEM om de geselecteerde Fragmenten van de Inhoud bij te werken.
5. Een reeks HTTP PUTs voor elk Inhoudsfragment om het gespecificeerde bezit bij te werken.
6. AEM as a Cloud Service gaat door met de eigenschappenupdates voor het inhoudsfragment en retourneert een geslaagde of mislukte reactie op de Adobe I/O Runtime-actie.
7. Het modaal ontving de reactie van de actie van Adobe I/O Runtime, en toont een lijst van succesvolle bulkupdates.

## Extensiepunt

In dit voorbeeld wordt het uitbreidingspunt `actionBar` uitgebreid om een aangepaste knop toe te voegen aan de Content Fragment Console.

| AEM-gebruikersinterface uitgebreid | Extensiepunt |
| ------------------------ | --------------------- |
| [&#x200B; de Console van het Fragment van de Inhoud &#x200B;](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [&#x200B; de Bar van de Actie &#x200B;](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |


## Voorbeeldextensie

In het voorbeeld wordt een bestaand Adobe Developer Console-project gebruikt en worden de volgende opties gebruikt bij het initialiseren van de App Builder-app via `aio app init` .

+ Welke sjablonen wilt u zoeken?: `All Extension Points`
+ Kies de sjabloon of sjablonen die u wilt installeren:` @adobe/aem-cf-admin-ui-ext-tpl`
+ Wat wilt u de extensie een naam geven?: `Bulk property update`
+ Geef een korte beschrijving van uw extensie op: `An example action bar extension that bulk updates a single property one or more content fragments.`
+ Met welke versie wilt u beginnen?: `0.0.1`
+ Wat wilt u nu doen?
   + `Add a custom button to Action Bar`
      + Geef een labelnaam op voor de knop: `Bulk property update`
      + Moet u een modaal voor de knoop tonen? `y`
   + `Add server-side handler`
      + Met Adobe I/O Runtime kunt u naar wens serverloze code aanroepen. Hoe wilt u deze handeling een naam geven?: `generic`

De gegenereerde App Builder-extensie-app wordt bijgewerkt zoals hieronder wordt beschreven.

### Toepassingsroutes{#app-routes}

`src/aem-cf-console-admin-1/web-src/src/components/App.js` bevat de [&#x200B; React router &#x200B;](https://reactrouter.com/en/main).

Er zijn twee logische reeksen routes:

1. De eerste routekaarten verzoeken aan `index.html`, die de component van het Reageren verantwoordelijk voor de [&#x200B; uitbreidingsregistratie &#x200B;](#extension-registration) aanhaalt.

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. De tweede reeks routes brengt URLs in kaart om componenten te Reageren die de inhoud van de modaal van de uitbreiding teruggeven. De `:selection` -param vertegenwoordigt een pad met een als scheidingsteken weergegeven inhoudsfragment.

   Als de uitbreiding veelvoudige knopen heeft om discrete acties aan te halen, elke [&#x200B; uitbreidingsregistratie &#x200B;](#extension-registration) kaarten aan een hier bepaalde route.

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

### Registratie van extensies

`ExtensionRegistration.js` , toegewezen aan de `index.html` -route, is het ingangspunt voor de AEM-extensie en definieert:

1. De locatie van de extensieknop wordt weergegeven in de AEM-ontwerpervaring (`actionBar` of `headerMenu`)
1. De definitie van de extensieknop in de functie `getButtons()`
1. De klikhandler voor de knop, in de functie `onClick()`

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Some unique ID for the extension used to facilitate communication between the extension and Content Fragment Console
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButtons() {
            return [
              {
                id: "examples.action-bar.bulk-property-update", // Unique ID for the button
                label: "Bulk property update", // Button label
                icon: "OpenIn", // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
                // Click handler for the extension button
                onClick(selections) {
                  // Collect the selected content fragment paths
                  const selectionIds = selections.map(
                    (selection) => selection.id
                  );

                  // Create a URL that maps to the
                  const modalURL =
                    "/index.html#" +
                    generatePath(
                      "/content-fragment/:selection/bulk-property-update",
                      {
                        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                        selection: encodeURIComponent(selectionIds.join("|")),
                      }
                    );

                  // Open the route in the extension modal using the constructed URL
                  guestConnection.host.modal.showUrl({
                    // The modal title
                    title: "Bulk property update",
                    url: modalURL,
                  });
                },
              },
            ];
          },
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### Modal

Elke route van de extensie, zoals gedefinieerd in [`App.js`](#app-routes) , wordt toegewezen aan een component React die wordt weergegeven in het modale gebied van de extensie.

In deze voorbeeld-app is er een modale React-component (`BulkPropertyUpdateModal.js`) met drie statussen:

1. Laden. De gebruiker moet wachten
1. Het formulier voor updates van eigenschappen met opsommingstekens waarmee de gebruiker de naam en de waarde van de eigenschap kan opgeven voor bijwerken
1. De reactie van de bulkeigenschapsupdate-bewerking, waarin de inhoudsfragmenten worden vermeld die zijn bijgewerkt en de fragmenten die niet konden worden bijgewerkt

Belangrijk, zou om het even welke interactie met AEM van de uitbreiding aan een [&#x200B; actie van Adobe I/O Runtime AppBuilder &#x200B;](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/) moeten worden gedelegeerd, die een afzonderlijk serverless proces is dat in [&#x200B; Adobe I/O Runtime &#x200B;](https://developer.adobe.com/runtime/docs/) loopt.
Het gebruik van Adobe I/O Runtime-acties om met AEM te communiceren, is om problemen met de connectiviteit van het delen van bronnen van verschillende oorsprong (CORS) te voorkomen.

Wanneer de vorm van de Update van het Bezit van het Bulk wordt voorgelegd, roept een douane `onSubmitHandler()` de actie van Adobe I/O Runtime aan, die de huidige gastheer van AEM (domein) en het toegangstoken van AEM van de gebruiker overgaat, die beurtelings [&#x200B; het Fragment API van de Inhoud van AEM &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html?lang=nl-NL) roept om de inhoudsfragmenten bij te werken.

Wanneer de reactie van de actie van Adobe I/O Runtime wordt ontvangen, wordt modal bijgewerkt om de resultaten van de bulkbezitsupdate verrichting te tonen.

+ `src/aem-cf-console-admin-1/web-src/src/components/BulkPropertyUpdateModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Form,
  Provider,
  Content,
  defaultTheme,
  ContextualHelp,
  Text,
  TextField,
  ButtonGroup,
  Button,
  Heading,
  ListView,
  Item,
} from '@adobe/react-spectrum'

import Spinner from "./Spinner"

import { useParams } from "react-router-dom"

import allActions from '../config.json'
import actionWebInvoke from '../utils'

import { extensionId } from "./Constants"

export default function BulkPropertyUpdateModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState()
  
  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  const [propertyName, setPropertyName] = useState(null);
  const [propertyValue, setPropertyValue] = useState(null);
  const [validationState, setValidationState] = useState({});

  // Get the selected content fragment paths from the route parameter `:selection`
  let { selection } = useParams();
  let fragmentIds = selection?.split('|') || [];
  
  console.log('Content Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error("Unable to locate a list of Content Fragments to update.")
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
       // extensionId is the unique id of this extension (you can make this up as long as its unique) .. in this case its `bulk-property-update` pulled out into Constants.js as it is also referenced in ExtensionRegistration.js
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])


  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (actionInvokeInProgress) {
    // If the bulk property action has been invoked but not completed, display a loading spinner
    return <Spinner />
  } else if (actionResponse) {
    // If the bulk property action has completed, display the response
    return renderResponse();
  } else {
    // Else display the bulk property update form
    return renderForm();
  }

  /**
   * Renders the Bulk Property Update form. 
   * This form has two fields:
   * - Property Name: The name of the Content Fragment property name to update
   * - Property Value: the value the Content Fragment property, specified by the Property Name, will be updated to
   * 
   * @returns the Bulk Property Update form
   */
  function renderForm() {
    return (
      // Use React Spectrum components to render the form
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">
          <Flex width="100%">
            <Form 
              width="100%">
              <TextField label="Property name"
                          isRequired={true}
                          validationState={validationState?.propertyName}
                onChange={setPropertyName}
                contextualHelp={
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>The <strong>Property name</strong> must be a valid for the Content Fragment model(s) the selected Content Fragments implement.</Text>
                    </Content>
                  </ContextualHelp>
                } />

              <TextField
                label="Property value"
                validationState={validationState?.propertyValue}
                onChange={setPropertyValue} />

              <ButtonGroup align="start" marginTop="size-200">
                <Button variant="cta" onPress={onSubmitHandler}>Update {fragmentIds.length} Content Fragments</Button>
              </ButtonGroup>
            </Form>
          </Flex>

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>
    )
  }
  /**
   * Display the response from the Adobe I/O Runtime action in the modal.
   * This includes:
   * - A list of content fragments that were updated successfully
   * - A list a content fragments that failed to update
   * 
   * @returns the response view
   */
  function renderResponse() {
    // Separate the successful and failed content fragments updates
    const successes = actionResponse.filter(item => item.status === 200);
    const failures = actionResponse.filter(item => item.status !== 200);

    return (
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">

          <Text>Bulk updated property <strong>{propertyName}</strong> with value <strong>{propertyValue}</strong></Text>

          {/* Render the list of content fragments that were updated successfully */}
          {successes.length > 0 &&
            <><Heading level="4">{successes.length} Content Fragments successfully updated</Heading>
              <ListView
                items={successes}
                selectionMode="none"
                aria-label="Successful updates"
              >
                {(item) => (
                  <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                    {item.fragmentId.split('/').pop()}
                  </Item>
                )}
              </ListView></>}

          {/* Render the list of content fragments that failed to update */}
          {failures.length > 0 &&
            <><Heading level="4">{failures.length} Content Fragments failed to update</Heading><ListView
              items={failures}
              selectionMode="none"
              aria-label="Failed updates"
            >
              {(item) => (
                <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                  {item.fragmentId.split('/').pop()}
                </Item>
              )}
            </ListView></>}

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>);
  }

  /**
   * Provide a close button for the modal, else it cannot be closed (without refreshing the browser window)
   * 
   * @returns a button that closes the modal.
   */
   function renderCloseButton() {
    return (
      <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
        <ButtonGroup align="end">
          <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
        </ButtonGroup>
      </Flex>
    );
  }

  /**
   * Handle the Bulk Property Update form submission.
   * This function calls the supporting Adobe I/O Runtime action to update the selected Content Fragments, and then returns the response for display in the modal
   * When invoking the Adobe I/O Runtime action, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The list of Content Fragment paths to update
   * - The Content Fragment property name to update
   * - The value to update the Content Fragment property to
   * 
   * @returns a list of content fragment update successes and failures
   */
  async function onSubmitHandler() {
    // Validate the form input fields
    if (propertyName?.length > 1) {
      setValidationState({propertyName: 'valid', propertyValue: 'valid'});
    } else {
      setValidationState({propertyName: 'invalid', propertyValue: 'valid'});
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    console.log('headers', headers);

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentIds: fragmentIds,
      propertyName: propertyName,
      propertyValue: propertyValue
    };

    // Invoke the Adobe I/O Runtime action named `generic`. This name defined in the `ext.config.yaml` file.
    const action = 'generic';

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      const actionResponse = await actionWebInvoke(allActions[action], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);

      console.log(`Response from ${action}:`, actionResponse)
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```


### Adobe I/O Runtime-actie

Een AEM-extensie App Builder-app kan 0 of veel Adobe I/O Runtime-acties definiëren of gebruiken.
Adobe Runtime-acties moeten verantwoordelijk werk zijn dat interactie met AEM of andere Adobe-webservices vereist.

In deze voorbeeldapp is de Adobe I/O Runtime-actie - die de standaardnaam `generic` gebruikt - verantwoordelijk voor:

1. Een reeks HTTP-aanvragen indienen bij de AEM Content Fragment API om de inhoudsfragmenten bij te werken.
1. De antwoorden van deze HTTP-aanvragen verzamelen, deze sorteren in successen en mislukkingen
1. Het terugkeren van de lijst van successen en mislukking voor vertoning door modal (`BulkPropertyUpdateModal.js`)

+ `src/aem-cf-console-admin-1/actions/generic/index.js`


```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

// main function that will be executed by Adobe I/O Runtime
async function main (params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action')

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params))

    // check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'fragmentIds', 'propertyName', 'propertyValue' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    const body = {
      "properties": {
        "elements": {
          [params.propertyName]: {
            "value": params.propertyValue
          }
        }
      }
    };

    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    let results = await Promise.all(params.fragmentIds.map(async (fragmentId) => {

      logger.info(`Updating fragment ${fragmentId} with property ${params.propertyName} and value ${params.propertyValue}`);

      const res = await fetch(`${params.aemHost}${fragmentId.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    const response = {
      statusCode: 200,
      body: results
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the A
     return response;

  } catch (error) {
    // log any server errors
    logger.error(error)
    // return with 500
    return errorResponse(500, 'server error', logger)
  }
}

exports.main = main
```
