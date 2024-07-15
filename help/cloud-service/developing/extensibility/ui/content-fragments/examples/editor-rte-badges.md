---
title: Badges toevoegen aan Rich Text Editor (RTE)
description: Leer hoe u badges aan de Rich Text Editor (RTE) toevoegt in de AEM Content Fragment Editor
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 538
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---

# Badges toevoegen aan Rich Text Editor (RTE)

Leer hoe u badges aan de Rich Text Editor (RTE) toevoegt in de AEM Content Fragment Editor.

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

{het symbool van de Redacteur van de Tekst van 0} Rich ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) is uitbreidingen die tekst in de Rich Text Editor (RTE) niet-editable maken. [ Dit betekent dat een als zodanig gedeclareerde badge alleen volledig kan worden verwijderd en niet gedeeltelijk kan worden bewerkt. Deze badges bieden ook ondersteuning voor speciale kleuren in de RTE, waarbij de auteur van de inhoud duidelijk wordt aangegeven dat de tekst een badge is en dus niet bewerkbaar. Bovendien geven ze visuele aanwijzingen over de betekenis van de badge-tekst.

Het gemeenschappelijkste gebruiksgeval voor RTE badges is hen samen met [ te gebruiken widgets RTE ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). Hierdoor kan inhoud die door de RTE-widget in de RTE wordt geïnjecteerd, niet-bewerkbaar zijn.

Typisch, worden de badges in verband met widgets gebruikt om de dynamische inhoud toe te voegen die een externe systeemgebiedsafhankelijkheid heeft maar _inhoudsauteurs kunnen_ de opgenomen dynamische inhoud niet wijzigen om de integriteit te handhaven. Ze kunnen alleen als een geheel item worden verwijderd.

De **badges** worden toegevoegd aan **RTE** in de Redacteur van het Fragment van de Inhoud gebruikend het `rte` uitbreidingspunt. Met de methode `getBadges()` van het `rte` extensiepunt worden een of meer badges toegevoegd.

Dit voorbeeld toont hoe te om een widget toe te voegen genoemd _de Grote Dienst van de Klant van de Boekingen van de Groep **om te vinden, te selecteren en, de adventure-specifieke details van de klantendienst van WKND zoals**Naam van de Vertegenwoordiger en **Aantal van de Telefoon**binnen een inhoud van RTE toe te voegen._ Gebruikend de bandenfunctionaliteit wordt het **Aantal van de Telefoon** gemaakt **niet-editable** maar WKND inhoudsauteurs kunnen de Naam van de Vertegenwoordiger uitgeven.

Ook, wordt het **Aantal van de Telefoon** gestileerd verschillend (blauw) dat een extra gebruiksgeval van de bandenfunctionaliteit is.

Om dingen eenvoudig te houden, gebruikt dit voorbeeld het ](https://react-spectrum.adobe.com/react-spectrum/index.html) kader van het Spectrum van het Reageren van de Adobe [ om widget of dialoog UI en hard-gecodeerde de telefoonaantallen van de Klantendienst van WKND te ontwikkelen. Als u het niet-bewerken en verschillende stijlkenmerken van de inhoud wilt bepalen, wordt het teken `#` gebruikt in het kenmerk `prefix` en `suffix` van de definitie van badges.

## Extensiepunten

Dit voorbeeld breidt zich tot uitbreidingspunt `rte` uit om een badge aan RTE in de Redacteur van het Fragment van de Inhoud toe te voegen.

| AEM UI uitgebreid | Extensiepunten |
| ------------------------ | --------------------- | 
| [ de Redacteur van het Fragment van de Inhoud ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) en [ Rich Text Editor Widgets ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) van de Redacteur van de Tekst[ |

## Voorbeeldextensie

Het volgende voorbeeld leidt tot de Dienst van de Klant van de Boekjes van de a _Grote Groep_ widget. Door op de `{` -toets in de RTE te drukken, wordt het contextmenu van de RTE-widgets geopend. Door de _Grote optie van de Dienst van de Klant van de Boekingen van de Groep_ van het contextmenu te selecteren wordt de douane modaal geopend.

Zodra het gewenste aantal van de klantendienst van modaal wordt toegevoegd, maken de badges het _Aantal van de Telefoon niet-editable_ en stijlen het in blauwe kleur.

### Registratie van extensies

`ExtensionRegistration.js` , toegewezen aan de `index.html` -route, is het ingangspunt voor de AEM extensie en definieert:

+ De definitie van de badge wordt in `getBadges()` gedefinieerd met de configuratiekenmerken `id` , `prefix` , `suffix` , `backgroundColor` en `textColor` .
+ In dit voorbeeld wordt het teken `#` gebruikt om de grenzen van deze badge te definiëren. Dit houdt in dat elke tekenreeks in de RTE die door `#` wordt omringd, wordt behandeld als een instantie van deze badge.

Zie ook de belangrijkste details van de RTE-widget:

+ De widgetdefinitie in `getWidgets()` functie met `id` -, `label` - en `url` kenmerken.
+ De waarde van het kenmerk `url` , een relatief URL-pad ( `/index.html#/largeBookingsCustomerService` ) om het modaal te laden.


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
          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",
              prefix: "#",
              suffix: "#",
              backgroundColor: "",
              textColor: "#071DF8",
            },
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",
              label: "Large Group Bookings Customer Service",
              url: "/index.html#/largeBookingsCustomerService",
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### `largeBookingsCustomerService` route toevoegen in `App.js`{#add-widgets-route}

Voeg in de hoofdcomponent React `App.js` de `largeBookingsCustomerService` -route toe om de gebruikersinterface voor het bovenstaande relatieve URL-pad te renderen.

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### Component `LargeBookingsCustomerService` React maken{#create-widget-react-component}

De widget of de dialoog UI wordt gecreeerd gebruikend het [ React Spectrum van de Adobe ](https://react-spectrum.adobe.com/react-spectrum/index.html) kader.

De componentencode React wanneer het toevoegen van de details van de klantendienst, omring de variabele van het telefoonaantal met het `#` geregistreerde badges karakter om het in badges, zoals `#${phoneNumber}#` om te zetten, zo maakt het niet-editable.

Hier volgen enkele belangrijke markeringen van de code van `LargeBookingsCustomerService` :

+ UI wordt teruggegeven gebruikend React de componenten van het Spectrum, als [ ComboBox ](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ ButtonGroup ](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [ Knoop ](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ De array `largeGroupCustomerServiceList` heeft een hardcoded toewijzing van een representatieve naam en telefoonnummer. In echt scenario, kunnen deze gegevens uit Adobe AppBuilder actie of externe systemen of huis worden teruggewonnen uitgegroeid of op wolk leverancier-gebaseerde API gateway.
+ `guestConnection` wordt geïnitialiseerd gebruikend `useEffect` [ Reageer haak ](https://react.dev/reference/react/useEffect) en geleid als componentenstaat. Het wordt gebruikt om met de AEM gastheer te communiceren.
+ De functie `handleCustomerServiceChange` krijgt representatieve naam en telefoonaantal en werkt de variabelen van de componentenstaat bij.
+ De functie `addCustomerServiceDetails` die `guestConnection` gebruikt, biedt RTE-instructie die moet worden uitgevoerd. In dit geval `insertContent` instructiecodefragment en HTML-codefragment.
+ Om het **telefoonaantal niet-editable** te maken gebruikend badges, wordt het `#` speciale karakter toegevoegd vóór en na de `phoneNumber` variabele, als `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
