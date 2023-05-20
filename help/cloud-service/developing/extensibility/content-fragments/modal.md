---
title: AEM extensie van Content Fragment, modaal
description: Leer hoe u een modaal extensiemodel voor de AEM Content Fragment maakt.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# Extensie, modaal

![AEM extensie voor inhoudsfragment, modaal](./assets/modal/modal.png){align="center"}

AEM de extensiemodel van Content Fragment kunt u een aangepaste UI koppelen aan de extensies van AEM inhoudsfragment. Dit kan zijn [Actiebalk](./action-bar.md) of [Menu Koptekst](./header-menu.md) knoppen.

Modals zijn React toepassingen, die op worden gebaseerd [Spectrum reageren](https://react-spectrum.adobe.com/react-spectrum/)en kan elke aangepaste UI maken die door de extensie wordt vereist, inclusief maar niet beperkt tot:

+ Bevestigingsdialoogvensters
+ [Invoerformulieren](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [Voortgangsindicatoren](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [Overzicht van resultaten](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ Foutberichten
+ ... of zelfs een volledige, multiview React toepassing!

## Modale routes

De modale ervaring wordt gedefinieerd door de extensie App Builder React-app die is gedefinieerd onder de `web-src` map. Net als bij elke React-app wordt de volledige ervaring georkestreerd met [Reageerroutes](https://reactrouter.com/en/main/components/routes) die renderen [Reageren op componenten](https://reactjs.org/docs/components-and-props.html).

Ten minste één route is vereist om de eerste modale weergave te genereren. Deze aanvankelijke route wordt aangehaald in [extensieverichting](#extension-registration)s `onClick(..)` functie, zoals hieronder getoond.


+ `./src/aem-cf-console-admin-1/web-src/src/components/App.js`

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

            For modals opened from Action Bar extensions, typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="content-fragment/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="content-fragment/my-modal"
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

Om een modaal te openen, roept een vraag aan `guestConnection.host.modal.showUrl(..)` is gemaakt op basis van de `onClick(..)` functie. `showUrl(..)` wordt doorgegeven aan een JavaScript-object met sleutel/waarden:

+ `title` verstrekt de naam van titel van modaal die aan de gebruiker wordt getoond
+ `url` is de URL die het [Reageerroute](#modal-routes) verantwoordelijk voor de eerste visie van het modaal.

Het is absoluut noodzakelijk dat de `url` doorgegeven aan `guestConnection.host.modal.showUrl(..)` wordt omgezet in route in de extensie, anders wordt er niets weergegeven in het modale.

Controleer de [kopmenu](./header-menu.md#modal) en [actiebalk](./action-bar.md#modal) documentatie voor het maken van modale URL&#39;s.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/content-fragment/my-modal";

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

Elke route van de extensie [dat is niet `index` route](./extension-registration.md#app-routes), wordt toegewezen aan een component React die kan worden gerenderd in de modale extensie.

Een modaal kan uit om het even welk aantal routes van React, van eenvoudig één-route modaal aan een complex, multi-routemodel worden samengesteld.

In het volgende voorbeeld ziet u een eenvoudig model met één route, maar deze modale weergave kan React-koppelingen bevatten die andere routes of gedragingen aanroepen.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

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

![Mogelijke sluitknop voor extensie AEM inhoudfragment](./assets/modal/close.png){align="center"}

Modals moeten hun eigen dichte controle verstrekken. Dit gebeurt door een beroep te doen op `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
