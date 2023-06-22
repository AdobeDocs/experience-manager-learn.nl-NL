---
title: Widgets toevoegen aan Rich Text Editor (RTE)
description: Leer hoe u widgets kunt toevoegen aan de Rich Text Editor (RTE) in de AEM Content Fragment Editor
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: be4c0a6a-5c1f-4408-9ac6-56b8f0653d42
source-git-commit: 9c8c03df7c510ab697d5222f9dffd5111519b712
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Widgets toevoegen aan Rich Text Editor (RTE)

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

Om de dynamische inhoud in de Rich Text Editor (RTE) toe te voegen, **widgets** kan worden gebruikt. De widgets helpen om eenvoudige of complexe UI in RTE te integreren en UI kan worden gecreeerd gebruikend het kader JS van uw keus. Ze kunnen worden beschouwd als dialoogvensters die worden geopend door op `{` speciale sleutel in RTE.

De widgets worden doorgaans gebruikt om de dynamische inhoud in te voegen die afhankelijk is van een extern systeem of die kan veranderen op basis van de huidige context.

De **widgets** worden toegevoegd aan de **RTE** in de Inhoudsfragmenteditor met de opdracht `rte` extensiepunt. Gebruiken `rte` extensiepunt `getWidgets()` methode één of vele widgets worden toegevoegd. Ze worden geactiveerd door op de knop `{` Gebruik een speciale toets om de optie contextmenu te openen en selecteer vervolgens de gewenste widget om de aangepaste interface van het dialoogvenster te laden.

In dit voorbeeld wordt getoond hoe u een widget met de naam _Lijst met kortingscodes_ om, de adventure-specific de disconteringscode van WKND binnen een inhoud van RTE te vinden te selecteren en toe te voegen. Deze kortingscodes kunnen worden beheerd in een extern systeem, zoals Order Management System (OMS), Product Information Management (PIM), een toepassing die op het thuisniveau wordt ontwikkeld of een Adobe AppBuilder-actie.

In dit voorbeeld wordt het volgende gebruikt om de [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) -framework voor het ontwikkelen van de interface van de widget of dialoog en de hardgecodeerde WKND-avontuurnaam, kortingscodegegevens.

## Extensiepunt

In dit voorbeeld wordt het uitbreidingspunt uitgebreid `rte` om een widget aan RTE in de Redacteur van het Fragment van de Inhoud toe te voegen.

| AEM UI uitgebreid | Extensiepunt |
| ------------------------ | --------------------- | 
| [Inhoudsfragmenteditor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Widgets Rich Text Editor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## Voorbeeldextensie

In het volgende voorbeeld wordt een _Lijst met kortingscodes_ widget. Door op de knop `{` speciale sleutel binnen RTE, wordt het contextmenu geopend, dan door te selecteren _Lijst met kortingscodes_ in het contextmenu wordt de dialoogvenster-interface geopend.

De auteurs van de inhoud WKND kunnen, huidige Adventure-specifieke kortingscode vinden selecteren en toevoegen, als beschikbaar.

### Registratie van extensies

`ExtensionRegistration.js`, toegewezen aan de route index.html, is het ingangspunt voor de AEM uitbreiding en bepaalt:

+ De widgetdefinitie in `getWidgets()` functie met `id, label and url` kenmerken.
+ De `url` kenmerkwaarde, een relatief URL-pad (`/index.html#/discountCodes`) om de interface van het dialoogvenster te laden.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
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
              id: "discountcode-list-widget",       // Provide a unique ID for the widget
              label: "Discount Code List",          // Provide a label for the widget
              url: "/index.html#/discountCodes",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### Toevoegen `discountCodes` route in `App.js`{#add-widgets-route}

In de hoofdcomponent React `App.js`, voegt u de `discountCodes` route om UI voor de bovengenoemde relatieve weg terug te geven URL.

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

### Maken `DiscountCodes` Reageren, component{#create-widget-react-component}

De widget- of dialoogvenster-UI wordt gemaakt met de [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) kader. De `DiscountCodes` de componentencode is zoals hieronder, hier zijn zeer belangrijke hoogtepunten:

+ UI wordt teruggegeven gebruikend React de componenten van het Spectrum, zoals [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Knop](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ De `adventureDiscountCodes` array heeft hardcoded mapping van avontuurnaam en kortingscode. In echt scenario, kunnen deze gegevens van Adobe AppBuilder actie of externe systemen zoals PIM, OMS of huis worden teruggewonnen of op wolkenleverancier-gebaseerde API gateway.
+ De `guestConnection` is geïnitialiseerd met de `useEffect` [Reagehaak](https://react.dev/reference/react/useEffect) en beheerd als componentstatus. Het wordt gebruikt om met de AEM gastheer te communiceren.
+ De `handleDiscountCodeChange` De functie krijgt de kortingscode voor de geselecteerde avontuurnaam en werkt de staatsvariabele bij.
+ De `addDiscountCode` functie gebruiken `guestConnection` -object bevat RTE-instructie die moet worden uitgevoerd. In dit geval `insertContent` Instructie en codefragment van HTML van de daadwerkelijke disconteringscode die in RTE moet worden opgenomen.

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
