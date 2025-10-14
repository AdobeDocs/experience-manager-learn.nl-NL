---
title: AEM UI-extensie modal
description: Leer hoe u een modaal extensiemodel voor de AEM UI maakt.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
duration: 127
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Extensie, modaal

![&#x200B; de extensie van AEM UI modaal &#x200B;](./assets/modal/modal.png){align="center"}

Met de uitbreidingsmodus van AEM UI kunt u een aangepaste UI koppelen aan AEM UI-extensies.

De modules zijn React toepassingen, die op [&#x200B; worden gebaseerd Reageer Spectrum &#x200B;](https://react-spectrum.adobe.com/react-spectrum/), en kunnen om het even welke douaneUI tot stand brengen die door de uitbreiding wordt vereist, die, maar niet beperkt tot:

+ Bevestigingsdialoogvensters
+ [&#x200B; de vormen van de Input &#x200B;](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [&#x200B; de indicatoren van de Voortgang &#x200B;](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [&#x200B; Overzicht van Resultaten &#x200B;](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Foutberichten
+ ... of zelfs een volledige, multiview React toepassing!

## Modale routes

De modale ervaring wordt gedefinieerd door de extensie App Builder React die is gedefinieerd in de map `web-src` . Zoals met om het even welke Reageren app, wordt de volledige ervaring georkestreerd gebruikend [&#x200B; Reageer routes &#x200B;](https://reactrouter.com/en/main/components/routes) die [&#x200B; Reageren componenten &#x200B;](https://reactjs.org/docs/components-and-props.html) teruggeven.

Ten minste één route is vereist om de eerste modale weergave te genereren. Deze aanvankelijke route wordt aangehaald in de [&#x200B; functie van de uitbreidingsregistratie &#x200B;](#extension-registration) `onClick(..)`, zoals hieronder getoond.


+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions.
            Depending on the extension point, different parameters are passed to the modal.
            This example illustrates a modal for the AEM Content Fragment Console (list view), where typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Where as Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="aem-ui-extension/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="aem-ui-extension/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## Registratie van extensies

Als u een modaal object wilt openen, wordt `guestConnection.host.modal.showUrl(..)` aangeroepen vanuit de functie `onClick(..)` van de extensie. `showUrl(..)` wordt doorgegeven aan een JavaScript-object met sleutel/waarden:

+ `title` bevat de naam van de titel van het modaal dat aan de gebruiker wordt weergegeven
+ `url` is URL die de [&#x200B; Reageer route &#x200B;](#modal-routes) verantwoordelijk voor de aanvankelijke mening van modal aanhaalt.

Het is absoluut noodzakelijk dat de `url` die aan `guestConnection.host.modal.showUrl(..)` wordt doorgegeven, wordt omgezet in routebeschrijving in de extensie, anders wordt er niets weergegeven in het modaal.

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/aem-ui-extension/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## Modal component

Elke route van de uitbreiding, [&#x200B; die niet de `index` route &#x200B;](./extension-registration.md#app-routes) is, kaarten aan een React component die in de modaal van de uitbreiding kan teruggeven.

Een modaal kan uit om het even welk aantal routes van React, van eenvoudig één-route modaal aan een complex, multi-routemodel worden samengesteld.

In het volgende voorbeeld ziet u een eenvoudig model met één route, maar deze modale weergave kan React-koppelingen bevatten die andere routes of gedragingen aanroepen.

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## Het modaal sluiten

![&#x200B; de extensie van AEM UI modaal sluiten knoop &#x200B;](./assets/modal/close.png){align="center"}

Modals moeten hun eigen dichte controle verstrekken. Dit gebeurt door `guestConnection.host.modal.close()` aan te roepen.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
