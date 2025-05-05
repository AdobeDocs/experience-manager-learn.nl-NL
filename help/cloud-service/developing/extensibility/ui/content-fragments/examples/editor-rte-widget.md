---
title: Widgets toevoegen aan Rich Text Editor (RTE)
description: Leer hoe u widgets kunt toevoegen aan Rich Text Editor (RTE) in de AEM Content Fragment Editor
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 476
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# Widgets toevoegen aan Rich Text Editor (RTE)

Leer hoe u widgets kunt toevoegen aan de Rich Text Editor (RTE) in de AEM Content Fragment Editor.

>[!VIDEO](https://video.tv.adobe.com/v/3447438?quality=12&learn=on&captions=dut)

Om de dynamische inhoud in de Rijke Redacteur van de Tekst (RTE) toe te voegen, kan de **widgets** functionaliteit worden gebruikt. De widgets helpen om eenvoudige of complexe UI in RTE te integreren en UI kan worden gecreeerd gebruikend het kader JS van uw keus. Ze kunnen worden beschouwd als dialoogvensters die worden geopend door op de speciale toets `{` in de RTE te drukken.

De widgets worden doorgaans gebruikt om de dynamische inhoud in te voegen die afhankelijk is van een extern systeem of die kan veranderen op basis van de huidige context.

**widgets** worden toegevoegd aan **RTE** in de Redacteur van het Fragment van de Inhoud gebruikend het `rte` uitbreidingspunt. Met de methode `getWidgets()` van het `rte` extensiepunt worden een of meer widgets toegevoegd. Ze worden geactiveerd door op de speciale toets `{` te drukken om de contextmenuoptie te openen en vervolgens de gewenste widget te selecteren om de aangepaste dialoogvenster-UI te laden.

Dit voorbeeld toont hoe te om een widget toe te voegen genoemd _Lijst van de Code van de Korting_ om, de adventure-specifieke de disconteringscode van WKND binnen een inhoud te vinden te selecteren en toe te voegen RTE. Deze kortingscodes kunnen worden beheerd in een extern systeem, zoals Order Management System (OMS), Product Information Management (PIM), een toepassing die op het thuisniveau wordt ontwikkeld of een Adobe AppBuilder-actie.

Om dingen eenvoudig te houden, gebruikt dit voorbeeld [ Adobe React Spectrum ](https://react-spectrum.adobe.com/react-spectrum/index.html) kader om widget of dialoog UI en hard-gecodeerde WKND adventure naam, kortingscodegegevens te ontwikkelen.

## Extensiepunt

Dit voorbeeld breidt zich tot uitbreidingspunt `rte` uit om een widget aan RTE in de Redacteur van het Fragment van de Inhoud toe te voegen.

| AEM-gebruikersinterface uitgebreid | Extensiepunt |
| ------------------------ | --------------------- | 
| [ de Redacteur van het Fragment van de Inhoud ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [ Rich Text Editor Widgets ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Voorbeeldextensie

Het volgende voorbeeld leidt tot de Lijst van de Code van de a _Korting_ widget. Door op de `{` speciale sleutel binnen RTE te duwen, wordt het contextmenu geopend, dan door de _optie van de Lijst van de Code van de Korting_ van het contextmenu te selecteren wordt de dialoog UI geopend.

De auteurs van de inhoud WKND kunnen, huidige Adventure-specifieke kortingscode vinden selecteren en toevoegen, als beschikbaar.

### Registratie van extensies

`ExtensionRegistration.js` , toegewezen aan de route index.html, is het ingangspunt voor de uitbreiding van AEM en bepaalt:

+ De widgetdefinitie in `getWidgets()` functie met `id, label and url` attributen.
+ De waarde van het kenmerk `url` , een relatief URL-pad ( `/index.html#/discountCodes` ) om de interface van het dialoogvenster te laden.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {
          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget", // Provide a unique ID for the widget
              label: "Discount Code List", // Provide a label for the widget
              url: "/index.html#/discountCodes", // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
        }, // Add a comma here
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### `discountCodes` route toevoegen in `App.js`{#add-widgets-route}

Voeg in de hoofdcomponent React `App.js` de `discountCodes` -route toe om de gebruikersinterface voor het bovenstaande relatieve URL-pad te renderen.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### Component `DiscountCodes` React maken{#create-widget-react-component}

De widget of de dialoog UI wordt gecreeerd gebruikend het [ React Spectrum van Adobe ](https://react-spectrum.adobe.com/react-spectrum/index.html) kader. De componentcode `DiscountCodes` ziet er als volgt uit:

+ UI wordt teruggegeven gebruikend React de componenten van het Spectrum, als [ ComboBox ](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ ButtonGroup ](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [ Knoop ](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ De array `adventureDiscountCodes` heeft een hardcoded mapping van adventure-naam en kortingscode. In werkelijkheid kunnen deze gegevens worden opgehaald uit een Adobe AppBuilder-actie of externe systemen zoals PIM, OMS of home grow of API-gateway op basis van een cloud provider.
+ `guestConnection` wordt geïnitialiseerd gebruikend `useEffect` [ Reageer haak ](https://react.dev/reference/react/useEffect) en geleid als componentenstaat. Deze wordt gebruikt om te communiceren met de AEM-host.
+ De functie `handleDiscountCodeChange` krijgt de kortingscode voor de geselecteerde avontuurnaam en werkt de staatsvariabele bij.
+ De functie `addDiscountCode` die `guestConnection` gebruikt, biedt RTE-instructie die moet worden uitgevoerd. In dit geval moeten `insertContent` instructie en HTML-codefragment met de werkelijke kortingscode die in de RTE moet worden ingevoegd.

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
