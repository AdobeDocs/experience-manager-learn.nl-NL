---
title: Op OpenAI gebaseerde afbeelding genereren via de extensie van de Content Fragment Console
description: Een voorbeeld AEM de uitbreiding van de Console van de Fragmenten van de Inhoud die digitale beeld van natuurlijke taalbeschrijvingen gebruikend OpenAI of DALL-E 2 produceert en geproduceerde beeld uploadt aan AEM, en het associeert aan het Inhoudsfragment.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
kt: 11649
thumbnail: KT-11649.png
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: d902eb9a8d497a43c8d4ca63767f81a35eadf139
workflow-type: tm+mt
source-wordcount: '1314'
ht-degree: 0%

---


# Op OpenAI gebaseerde digitale afbeelding genereren, uploaden naar AEM voorbeeldextensie

![Genereren van digitale afbeeldingen](./assets/digital-image-generation/screenshot.png){align="center"}

Dit voorbeeld AEM de extensie Content Fragment Console is een [actiebalk](../action-bar.md) extensie die digitale afbeeldingen genereert op basis van natuurlijke taalinvoer met [OpenAI API](https://openai.com/api/) of [DALL.E 2](https://openai.com/dall-e-2/). De gegenereerde afbeelding wordt geüpload naar de AEM DAM en de afbeeldingseigenschap van het geselecteerde inhoudsfragment wordt bijgewerkt om deze nieuw gegenereerde, geüploade afbeelding van DAM te gebruiken.

In dit voorbeeld leert u:

1. Afbeelding genereren met [OpenAI API](https://beta.openai.com/docs/guides/images/image-generation-beta) of [DALL.E 2](https://openai.com/dall-e-2/)
1. Afbeeldingen uploaden naar AEM
1. Update van eigenschap Content Fragment

De functionele stroom van de voorbeeldextensie is als volgt:

![Adobe I/O Runtime-actiestroom voor het genereren van digitale afbeeldingen](./assets/digital-image-generation/flow.png){align="center"}

1. Selecteer Inhoudsfragment en klik op de extensie `Generate Image` in de [actiebalk](#extension-registration) opent de [modaal](#modal).
1. De [modaal](#modal) geeft een aangepast invoerformulier weer dat is gebouwd met [Spectrum reageren](https://react-spectrum.adobe.com/react-spectrum/).
1. De gebruiker die het formulier verzendt, verzendt het formulier `Image Description` tekst, het geselecteerde inhoudsfragment en de AEM host naar de [aangepaste Adobe I/O Runtime-actie](#adobe-io-runtime-action).
1. De [Adobe I/O Runtime-actie](#adobe-io-runtime-action) valideert de invoer.
1. Vervolgens worden de OpenAI&#39;s [Afbeelding genereren](https://beta.openai.com/docs/guides/images/image-generation-beta) API en gebruikt `Image Description` tekst om aan te geven welke afbeelding moet worden gegenereerd.
1. De [image genereren](https://beta.openai.com/docs/guides/images/image-generation-beta) eindpunt leidt tot een origineel beeld van grootte _1024x1024_ pixels die de parameterwaarde van de prompt request gebruiken en retourneert de gegenereerde afbeeldings-URL als reactie.
1. De [Adobe I/O Runtime-actie](#adobe-io-runtime-action) Hiermee downloadt u de gegenereerde afbeelding naar de App Builder-runtime.
1. Vervolgens wordt het uploaden van de afbeelding vanuit de App Builder-runtime naar AEM DAM gestart onder een vooraf gedefinieerd pad.
1. De AEM as a Cloud Service slaat afbeelding op naar de DAM en retourneert een geslaagde of mislukte reactie op de Adobe I/O Runtime-actie. De geslaagde upload reactie werkt de geselecteerde de bezitswaarde van het Beeld van het Fragment van de Inhoud bij gebruikend een andere HTTP- verzoek aan AEM van de actie van Adobe I/O Runtime.
1. Het modaal ontvangt de reactie van de actie van Adobe I/O Runtime, en verstrekt AEM verbinding van activa details van het onlangs geproduceerde, geüploade beeld.

In deze video wordt het genereren van voorbeeldafbeeldingen met de extensie OpenAI of DALL.E 2 bekeken, evenals de werking en de ontwikkeling van de afbeelding. De video bevat hoofdstukmarkeringen, zoals __Functionele demo, Opstelling, en Tech-Code__ om snel naar het relevante stuk te kijken.

>[!VIDEO](https://video.tv.adobe.com/v/3413093/?quality=12&learn=on)


## De app App Builder-extensie

In het voorbeeld wordt een bestaand Adobe Developer Console-project gebruikt en worden de volgende opties gebruikt bij het initialiseren van de App Builder-app via `aio app init`.

+ Welke sjablonen wilt u zoeken?: `All Extension Points`
+ Kies de sjabloon die u wilt installeren:` @adobe/aem-cf-admin-ui-ext-tpl`
+ Wat wilt u de extensie een naam geven?: `Image generation`
+ Geef een korte beschrijving van uw extensie op: `An example action bar extension that generates an image using OpenAI and uploads it to AEM DAM.`
+ Met welke versie wilt u beginnen?: `0.0.1`
+ Wat wilt u nu doen?
   + `Add a custom button to Action Bar`
      + Geef een labelnaam op voor de knop: `Generate Image`
      + Moet u een modaal voor de knoop tonen? `y`
   + `Add server-side handler`
      + Met Adobe I/O Runtime kunt u naar wens serverloze code aanroepen. Hoe wilt u deze handeling een naam geven?: `generate-image`

De gegenereerde App Builder-extensie-app wordt bijgewerkt zoals hieronder wordt beschreven.

## Aanvullende instellingen

1. Meld u gratis aan [OpenAI API](https://openai.com/api/) een account maken en een [API-sleutel](https://beta.openai.com/account/api-keys)
1. Voeg deze sleutel aan uw project App Builder toe `.env` file

   ```
       # Specify your secrets here
       # This file must not be committed to source control
       ## Adobe I/O Runtime credentials
       ...
       AIO_runtime_apihost=https://adobeioruntime.net
       ...
       # OpenAI secret API key
       OPENAI_API_KEY=my-openai-secrete-key-to-generate-images
       ...
   ```

1. Voldoende `OPENAI_API_KEY` Als param voor de Adobe I/O Runtime-actie, werkt u de `src/aem-cf-console-admin-1/ext.config.yaml`

   ```yaml
       ...
   
       runtimeManifest:
         packages:
           aem-cf-console-admin-1:
             license: Apache-2.0
             actions:
               generate-image:
                 function: actions/generate-image/index.js
                 web: 'yes'
                 runtime: nodejs:16
                 inputs:
                   LOG_LEVEL: debug
                   OPENAI_API_KEY: $OPENAI_API_KEY
       ...
   ```

1. Installeren onder Node.js-bibliotheken
   1. [De OpenAI Node.js-bibliotheek](https://github.com/openai/openai-node#installation) - om de OpenAI API gemakkelijk aan te roepen
   1. [AEM uploaden](https://github.com/adobe/aem-upload#install) - om afbeeldingen te uploaden naar AEM-CS-instanties.

## Toepassingsroutes{#app-routes}

De `src/aem-cf-console-admin-1/web-src/src/components/App.js` bevat de [Reageren router](https://reactrouter.com/en/main).

Er zijn twee logische reeksen routes:

1. De eerste routekaarten verzoeken aan `index.html`, die de component React aanroept die verantwoordelijk is voor de [extensieverichting](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. De tweede reeks routes brengt URLs in kaart om componenten te Reageren die de inhoud van de modaal van de uitbreiding teruggeven. De `:selection` param staat voor een pad met een als scheidingsteken weergegeven inhoudsfragment.

   Als de extensie meerdere knoppen heeft om afzonderlijke handelingen aan te roepen, moet elke knop [extensieverichting](#extension-registration) kaarten aan een hier bepaalde route.

   ```javascript
   <Route
       exact path="content-fragment/:selection/generate-image-modal"
       element={<GenerateImageModal />}
       />
   ```

## Registratie van extensies

`ExtensionRegistration.js`, toegewezen aan de `index.html` route, is het ingangspunt voor de AEM uitbreiding en bepaalt:

1. De locatie van de extensieknop wordt weergegeven in de AEM-ontwerpervaring (`actionBar` of `headerMenu`)
1. De definitie van de extensieknop in `getButton()` function
1. De klikmanager voor de knoop, in `onClick()` function

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'generate-image',     // Unique ID for the button
              'label': 'Generate Image',  // Button label 
              'icon': 'PublishCheck'      // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/generate-image-modal",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|')),
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Generate Image",
              url: modalURL
            })
          }
        },

      }
    })
  }
  init().catch(console.error)
```

## Modal

Elke route van de uitbreiding, zoals bepaald in [`App.js`](#app-routes), wordt toegewezen aan een component React die wordt weergegeven in het modale gedeelte van de extensie.

In deze voorbeeld-app is er een modale React-component (`GenerateImageModal.js`) dat vier staten heeft:

1. Laden, wat aangeeft dat de gebruiker moet wachten
1. Het waarschuwingsbericht dat de gebruikers de suggestie geeft slechts één inhoudsfragment tegelijk te selecteren
1. Het formulier Afbeelding genereren waarmee de gebruiker een beschrijving van de afbeelding in de natuurlijke taal kan opgeven.
1. De reactie van de afbeeldingsgeneratiebewerking die de koppeling bevat met de AEM elementdetails van de nieuw gegenereerde, geüploade afbeelding.

Belangrijk is dat elke interactie met AEM van de extensie wordt gedelegeerd aan een [Handeling AppBuilder Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/), dat een afzonderlijk serverloos proces is dat wordt uitgevoerd in [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
Het gebruik van Adobe I/O Runtime-acties om te communiceren met AEM, is om kwesties met betrekking tot de connectiviteit tussen bronnen van verschillende oorsprong (CORS) te voorkomen.

Wanneer de _Afbeelding genereren_ formulier is verzonden, een aangepaste `onSubmitHandler()` Hiermee wordt de Adobe I/O Runtime-handeling aangeroepen, waarbij de beschrijving van de afbeelding, de huidige AEM (domein) en het toegangstoken van de AEM van de gebruiker worden doorgegeven. De actie roept dan OpenAI [Afbeelding genereren](https://beta.openai.com/docs/guides/images/image-generation-beta) API om een afbeelding te genereren met de ingediende afbeeldingsbeschrijving. Volgende gebruiken [AEM uploaden](https://github.com/adobe/aem-upload) knooppuntmodule&#39;s `DirectBinaryUpload` klasse uploadt het geproduceerde beeld aan AEM en gebruikt definitief [AEM Content Fragment API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) om de inhoudsfragmenten bij te werken.

Wanneer de reactie van de Adobe I/O Runtime-actie wordt ontvangen, wordt het modaal bijgewerkt om de resultaten van de afbeeldingsgeneratiebewerking weer te geven.

+ `src/aem-cf-console-admin-1/web-src/src/components/GenerateImageModal.js`

```javascript
export default function GenerateImageModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the application state
  const [imageDescription, setImageDescription] = useState(null);
  const [validationState, setValidationState] = useState({});

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Get the selected content fragment paths from the route parameter `:selection`
  const { selection } = useParams();
  const fragmentIds = selection?.split('|') || [];

  console.log('Selected Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error('The Content Fragments are not selected, can NOT generate images');
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />;
  } if (actionInvokeInProgress) {
    // If the 'Generate Image' action has been invoked but not completed, display a loading spinner
    return <Spinner />;
  } if (fragmentIds.length > 1) {
    // If more than one CF selected show warning and suggest to select only one CF
    return renderMoreThanOneCFSelectionError();
  } if (fragmentIds.length === 1 && !actionResponse) {
    // Display the 'Generate Image' modal and ask for image description
    return renderImgGenerationForm();
  } if (actionResponse) {
    // If the 'Generate Image' actio has completed, display the response
    return renderActionResponse();
  }

  /**
   * Renders the message suggesting to select only on CF at a time to not lose credits accidentally
   *
   * @returns the suggestion or error message to select one CF at a time
   */
  function renderMoreThanOneCFSelectionError() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">
          <Text>
            As this operation
            <strong> uses credits from Generative AI services</strong>
            {' '}
            such as DALL.E 2 (or Stable Dufusion), we allow only one Generate Image at a time.
            <p />
            <strong>So please select only one Content Fragment at this moment.</strong>
          </Text>

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Renders the form asking for image description in the natural language and
   * displays message this action uses credits from Generative AI services.
   *
   *
   * @returns the image description input field and credit usage message
   */
  function renderImgGenerationForm() {
    return (

      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          <Flex width="100%">
            <Form
              width="100%"
            >
              <TextField
                label="Image Description"
                description="The image description in natural language, for e.g. Alaskan adventure in wilderness, animals, and flowers."
                isRequired
                validationState={validationState?.propertyName}
                onChange={setImageDescription}
                contextualHelp={(
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>
                        The
                        <strong>description of an image</strong>
                        {' '}
                        you are looking for in the natural language, for e.g. &quot;Family vacation on the beach with blue ocean, dolphins, boats and drink&quot;
                      </Text>
                    </Content>
                  </ContextualHelp>
                  )}
              />

              <Text>
                <p />
                Please note this will use credits from Generative AI services such as OpenAI/DALL.E 2. The AI-generated images are saved to this AEM as a Cloud Service Author service using logged user access (IMS) token.
              </Text>

              <ButtonGroup align="end">
                <Button variant="accent" onPress={onSubmitHandler}>Use Credits</Button>
                <Button variant="accent" onPress={() => guestConnection.host.modal.close()}>Close</Button>
              </ButtonGroup>
            </Form>
          </Flex>

        </Content>
      </Provider>

    );
  }

  function buildAssetDetailsURL(aemImgURL) {
    const urlParts = aemImgURL.split('.com');
    const aemAssetdetailsURL = `${urlParts[0]}.com/ui#/aem/assetdetails.html${urlParts[1]}`;

    return aemAssetdetailsURL;
  }

  /**
   * Displays the action response received from the App Builder
   *
   * @returns Displays App Builder action and details
   */
  function renderActionResponse() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          {actionResponse.status === 'success'
            && (
              <>
                <Heading level="4">
                  Successfully generated an image, uploaded it to this AEM-CS Author service, and associated it to the selected Content Fragment.
                </Heading>

                <Text>
                  {' '}
                  Please see generated image in AEM-CS
                  {' '}
                  <Link>
                    <a href={buildAssetDetailsURL(actionResponse.aemImgURL)} target="_blank" rel="noreferrer">
                      here.
                    </a>
                  </Link>
                </Text>
              </>
            )}

          {actionResponse.status === 'failure'
            && (
            <Heading level="4">
              Failed to generate, upload image, please check App Builder logs.
            </Heading>
            )}

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Handle the Generate Image form submission.
   * This function calls the supporting Adobe I/O Runtime actions such as
   * - Call the Generative AI service (DALL.E) with 'image description' to generate an image
   * - Download the AI generated image to App Builder runtime
   * - Save the downloaded image to AEM DAM and update Content Fragement's image reference property to use this new image
   *
   * When invoking the Adobe I/O Runtime actions, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The Content Fragment path to update
   *
   * @returns In case of success the updated content fragment, otherwise failure message
   */
  async function onSubmitHandler() {
    console.log('Started Image Generation orchestration');

    // Validate the form input fields
    if (imageDescription?.length > 1) {
      setValidationState({ imageDescription: 'valid' });
    } else {
      setValidationState({ imageDescription: 'invalid' });
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      Authorization: `Bearer ${guestConnection.sharedContext.get('auth').imsToken}`,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg,
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {

      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentId: fragmentIds[0],
      imageDescription,
    };

    const generateImageAction = 'generate-image';

    try {
      const generateImageActionResponse = await actionWebInvoke(allActions[generateImageAction], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(generateImageActionResponse);

      console.log(`Response from ${generateImageAction}:`, actionResponse);
    } catch (e) {
      // Log and store any errors
      console.error(e);
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```

## Adobe I/O Runtime-actie

Een AEM extensie App Builder-app kan 0 of veel Adobe I/O Runtime-acties definiëren of gebruiken.
Adobe Runtime actie is de oorzaak van het werk dat interactie met AEM of Adobe of derdeWebdiensten vereist.

In deze voorbeeldapp `generate-image` Adobe I/O Runtime-actie is verantwoordelijk voor:

1. Een afbeelding genereren met [OpenAI API Image Generation](https://beta.openai.com/docs/guides/images/image-generation-beta) service
1. De gegenereerde afbeelding uploaden naar AEM-CS-instantie met [AEM uploaden](https://github.com/adobe/aem-upload) bibliotheek
1. Een HTTP-aanvraag indienen bij de AEM Content Fragment-API om de afbeeldingseigenschap van het inhoudsfragment bij te werken.
1. De belangrijkste informatie van succes en mislukking voor vertoning door modal terugkeren (`GenerateImageModal.js`)


### De orkest - `index.js`

De `index.js` organiseert boven 1 tot 3 taken door de respectieve modules JavaScript te gebruiken, namelijk `generate-image-using-openai, upload-generated-image-to-aem, update-content-fragement`. Deze modules en bijbehorende code worden beschreven in de volgende [subsecties](#image-generation-module---generate-image-using-openaijs).

+ `src/aem-cf-console-admin-1/actions/generate-image/index.js`

```javascript
/**
 *
 * This action orchestrates an image generation by calling the OpenAI API (DALL.E 2) and saves generated image to AEM.
 *
 * It leverages following modules
 *  - 'generate-image-using-openai' - To generate an image using OpenAI API
 *  - 'upload-generated-image-to-aem' - To upload the generated image into AEM-CS instance
 *  - 'update-content-fragement' - To update the CF image property with generated image's DAM path
 *
 */

const { Core } = require('@adobe/aio-sdk');
const {
  errorResponse, stringParameters, getBearerToken, checkMissingRequestInputs,
} = require('../utils');

const { generateImageUsingOpenAI } = require('./generate-image-using-openai');

const { uploadGeneratedImageToAEM } = require('./upload-generated-image-to-aem');

const { updateContentFragmentToUseGeneratedImg } = require('./update-content-fragement');

// main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' });

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action');

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params));

    // check for missing request input parameters and headers
    const requiredParams = ['aemHost', 'fragmentId', 'imageDescription'];
    const requiredHeaders = ['Authorization'];
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // extract the user Bearer token from the Authorization header
    const token = getBearerToken(params);

    // Call OpenAI (DALL.E 2) API to generate an image using image description
    const generatedImageURL = await generateImageUsingOpenAI(params);
    logger.info(`Generated image using OpenAI API and url is : ${generatedImageURL}`);

    // Upload the generated image to AEM-CS
    const uploadedImagePath = await uploadGeneratedImageToAEM(params, generatedImageURL, token);
    logger.info(`Uploaded image to AEM, path is: ${uploadedImagePath}`);

    // Update Content Fragment with the newly generated image reference
    const updateContentFragmentPath = await updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, token);
    logger.info(`Updated Content Fragment path is: ${updateContentFragmentPath}`);

    let result;
    if (updateContentFragmentPath) {
      result = {
        status: 'success', message: 'Successfully generated and uploaded image to AEM', genTechServiceImageURL: generatedImageURL, aemImgURL: uploadedImagePath, fragmentPath: updateContentFragmentPath,
      };
    } else {
      result = { status: 'failure', message: 'Failed to generated and uploaded image, please check App Builder logs' };
    }

    const response = {
      statusCode: 200,
      body: result,
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
  } catch (error) {
    // log any server errors
    logger.error(error);
    // return with 500
    return errorResponse(500, 'server error', logger);
  }
}

exports.main = main;
```


### Module voor het genereren van afbeeldingen - `generate-image-using-openai.js`

Deze module is verantwoordelijk voor het aanroepen van OpenAI&#39;s [Afbeelding genereren](https://beta.openai.com/docs/guides/images/image-generation-beta) eindpunt gebruiken [openai](https://github.com/openai/openai-node) bibliotheek. De OpenAI API-geheimhoudingssleutel instellen in het dialoogvenster `.env` bestand, gebruikt `params.OPENAI_API_KEY`.

+ `src/aem-cf-console-admin-1/actions/generate-image/generate-image-using-openai.js`

```javascript
/**
 * This module calls OpenAI API to generate an image based on image description provided to Action
 *
 */

const { Configuration, OpenAIApi } = require('openai');

const { Core } = require('@adobe/aio-sdk');

// Placeholder than actual OpenAI Image
const PLACEHOLDER_IMG_URL = 'https://www.gstatic.com/webp/gallery/2.png';

async function generateImageUsingOpenAI(params) {
  // create a Logger
  const logger = Core.Logger('generateImageUsingOpenAI', { level: params.LOG_LEVEL || 'info' });

  let generatedImageURL = PLACEHOLDER_IMG_URL;

  // create configuration object with the API Key
  const configuration = new Configuration({
    apiKey: params.OPENAI_API_KEY,
  });

  // create OpenAIApi object
  const openai = new OpenAIApi(configuration);

  logger.info(`Generating image for input: ${params.imageDescription}`);

  try {
    // invoke createImage method with details
    const response = await openai.createImage({
      prompt: params.imageDescription,
      n: 1,
      size: '1024x1024',
    });

    generatedImageURL = response.data.data[0].url;

    logger.info(`The OpenAI generate image url is: ${generatedImageURL}`);
  } catch (error) {
    logger.error(`Error while generating image, details are: ${error}`);
  }

  return generatedImageURL;
}

module.exports = {
  generateImageUsingOpenAI,
};
```

### Afbeelding uploaden naar AEM module - `upload-generated-image-to-aem.js`

Deze module is verantwoordelijk voor het uploaden van de OpenAI-afbeelding naar AEM. [AEM uploaden](https://github.com/adobe/aem-upload) bibliotheek. De gegenereerde afbeelding wordt eerst naar de App Builder-runtime gedownload met Node.js [Bestandssysteem](https://nodejs.org/api/fs.html) bibliotheek en zodra het uploaden naar AEM is voltooid, wordt deze verwijderd.

In onderstaande code `uploadGeneratedImageToAEM` Deze functie ordent het genereren van een download naar de runtime, uploadt deze naar AEM en verwijdert deze uit de runtime. De afbeelding wordt geüpload naar de `/content/dam/wknd-shared/en/generated` weg, zorg ervoor alle omslagen in DAM bestaan, zijn eerste vereiste om te gebruiken [AEM uploaden](https://github.com/adobe/aem-upload) bibliotheek.

+ `src/aem-cf-console-admin-1/actions/generate-image/upload-generated-image-to-aem.js`

```javascript
/**
 * This module uploads the generated image to AEM-CS instance using current user's IMS token
 *
 */

const { Core } = require('@adobe/aio-sdk');
const fs = require('fs');

const {
  DirectBinaryUploadErrorCodes,
  DirectBinaryUpload,
  DirectBinaryUploadOptions,
} = require('@adobe/aem-upload');

const codes = DirectBinaryUploadErrorCodes;
const IMG_EXTENSION = '.png';

const GENERATED_IMAGES_DAM_PATH = '/content/dam/wknd-shared/en/generated';

async function downloadImageToRuntime(logger, generatedImageURL) {
  logger.log('Downloading generated image to the runtime');

  // placeholder image name
  let generatedImageName = 'generated.png';

  try {
    // Get the generated image name from the image URL
    const justImgURL = generatedImageURL.substring(0, generatedImageURL.indexOf(IMG_EXTENSION) + 4);
    generatedImageName = justImgURL.substring(justImgURL.lastIndexOf('/') + 1);

    // Read image from URL as the buffer
    const response = await fetch(generatedImageURL);
    const buffer = await response.buffer();

    // Write/download image to the runtime
    fs.writeFileSync(generatedImageName, buffer, (err) => {
      if (err) throw err;
      logger.log('Saved the generated image!');
    });
  } catch (error) {
    logger.error(`Error while downloading image on the runtime, details are: ${error}`);
  }

  return generatedImageName;
}

function setupEventHandlers(binaryUpload, logger) {
  binaryUpload.on('filestart', (data) => {
    const { fileName } = data;

    logger.log(`Started file upload ${fileName}`);
  });

  binaryUpload.on('fileprogress', (data) => {
    const { fileName, transferred } = data;

    logger.log(`Fileupload is in progress ${fileName} & ${transferred}`);
  });

  binaryUpload.on('fileend', (data) => {
    const { fileName } = data;

    logger.log(`Finished file upload ${fileName}`);
  });

  binaryUpload.on('fileerror', (data) => {
    const { fileName, errors } = data;

    logger.log(`Error in file upload ${fileName} and ${errors}`);
  });
}

async function getImageSize(downloadedImgName) {
  const stats = fs.statSync(downloadedImgName);
  return stats.size;
}

async function uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken) {
  let aemImageURL;
  try {
    logger.log('Uploading generated image to AEM from the runtime');

    const binaryUpload = new DirectBinaryUpload();

    // setup event handlers to track the progress, success or error
    setupEventHandlers(binaryUpload, logger);

    // get downloaded image size
    const imageSize = await getImageSize(downloadedImgName);
    logger.info(`The image upload size is: ${imageSize}`);

    // The deatils of the file to be uploaded
    const uploadFiles = [
      {
        fileName: downloadedImgName, // name of the file as it will appear in AEM
        fileSize: imageSize, // total size, in bytes, of the file
        filePath: downloadedImgName, // Full path to the local file
      },
    ];

    // Provide AEM URL and DAM Path where images will be uploaded
    const options = new DirectBinaryUploadOptions()
      .withUrl(`${aemURL}${GENERATED_IMAGES_DAM_PATH}`)
      .withUploadFiles(uploadFiles);

    // Add headers like content type and authorization
    options.withHeaders({
      'content-type': 'image/png',
      Authorization: `Bearer ${accessToken}`,
    });

    // Start the upload to AEM
    await binaryUpload.uploadFiles(options)
      .then((result) => {
        // Handle Error
        result.getErrors().forEach((error) => {
          if (error.getCode() === codes.ALREADY_EXISTS) {
            logger.error('The generated image already exists');
          }
        });

        // Handle Upload result and check for errors
        result.getFileUploadResults().forEach((fileResult) => {
          // log file upload result
          logger.info(`File upload result ${JSON.stringify(fileResult)}`);

          fileResult.getErrors().forEach((fileErr) => {
            if (fileErr.getCode() === codes.ALREADY_EXISTS) {
              const fileName = fileResult.getFileName();
              logger.error(`The generated image already exists ${fileName}`);
            }
          });
        });
      })
      .catch((err) => {
        logger.info(`Failed to uploaded generated image to AEM${err}`);
      });

    logger.info('Successfully uploaded generated image to AEM');

    aemImageURL = `${aemURL + GENERATED_IMAGES_DAM_PATH}/${downloadedImgName}`;
  } catch (error) {
    logger.info(`Error while uploading generated image to AEM, see ${error}`);
  }

  return aemImageURL;
}

async function deleteFileFromRuntime(logger, downloadedImgName) {
  try {
    logger.log('Deleting the generated image from the runtime');

    fs.unlinkSync(downloadedImgName);

    logger.log('Successfully deleted the generated image from the runtime');
  } catch (error) {
    logger.error(`Error while deleting generated image from the runtime, details are: ${error}`);
  }
}

async function uploadGeneratedImageToAEM(params, generatedImageURL, accessToken) {
  // create a Logger
  const logger = Core.Logger('uploadGeneratedImageToAEM', { level: params.LOG_LEVEL || 'info' });

  const aemURL = params.aemHost;

  logger.info(`Uploading generated image from ${generatedImageURL} to AEM ${aemURL} by streaming the bytes.`);

  // download image to the App Builder runtime
  const downloadedImgName = await downloadImageToRuntime(logger, generatedImageURL);

  // Upload image to AEM from the App Builder runtime
  const aemImageURL = await uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken);

  // Delete the downloaded image from the App Builder runtime
  await deleteFileFromRuntime(logger, downloadedImgName);

  return aemImageURL;
}

module.exports = {
  uploadGeneratedImageToAEM,
};
```

### Module Inhoudsfragment bijwerken - `update-content-fragement.js`

Deze module is verantwoordelijk voor het bijwerken van de afbeeldingseigenschap van het inhoudsfragment met het DAM-pad van de nieuw geüploade afbeelding met behulp van de API voor AEM inhoudsfragment.

+ `src/aem-cf-console-admin-1/actions/generate-image/update-content-fragement.js`

```javascript
/**
 * This module updates the CF image property with generated image's DAM path
 *
 */
const { Core } = require('@adobe/aio-sdk');

const ADVENTURE_MODEL_IMG_PROPERTY_NAME = 'primaryImage';

const ARTICLE_MODEL_IMG_PROPERTY_NAME = 'featuredImage';

const AUTHOR_MODEL_IMG_PROPERTY_NAME = 'profilePicture';

function findImgPropertyName(fragmenPath) {
  if (fragmenPath && fragmenPath.includes('/adventures')) {
    return ADVENTURE_MODEL_IMG_PROPERTY_NAME;
  } if (fragmenPath && fragmenPath.includes('/magazine')) {
    return ARTICLE_MODEL_IMG_PROPERTY_NAME;
  }
  return AUTHOR_MODEL_IMG_PROPERTY_NAME;
}

async function updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, accessToken) {
  // create a Logger
  const logger = Core.Logger('updateContentFragment', { level: params.LOG_LEVEL || 'info' });

  const fragmenPath = params.fragmentId;
  const imgPropName = findImgPropertyName(fragmenPath);
  const relativeImgPath = uploadedImagePath.substring(uploadedImagePath.indexOf('/content/dam'));

  logger.info(`Update CF ${fragmenPath} to use ${relativeImgPath} image path`);

  const body = {
    properties: {
      elements: {
        [imgPropName]: {
          value: relativeImgPath,
        },
      },
    },
  };

  const res = await fetch(`${params.aemHost}${fragmenPath.replace('/content/dam/', '/api/assets/')}.json`, {
    method: 'put',
    body: JSON.stringify(body),
    headers: {
      Authorization: `Bearer ${accessToken}`,
      'Content-Type': 'application/json',
    },

  });

  if (res.ok) {
    logger.info(`Successfully updated ${fragmenPath}`);
    return fragmenPath;
  }

  logger.info(`Failed to update ${fragmenPath}`);
  return '';
}

module.exports = {
  updateContentFragmentToUseGeneratedImg,
};
```

