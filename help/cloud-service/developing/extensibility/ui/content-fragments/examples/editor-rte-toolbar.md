---
title: Aangepaste knop toevoegen aan Rich Text Editor (RTE)-werkbalk
description: Leer hoe u een aangepaste knop toevoegt aan de Rich Text Editor (RTE)-werkbalk in de AEM Content Fragment Editor
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 345
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Aangepaste knop toevoegen aan Rich Text Editor (RTE)-werkbalk

Leer hoe u een aangepaste knop toevoegt aan de Rich Text Editor (RTE)-werkbalk in de AEM Content Fragment Editor.

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

De knopen van de douane kunnen aan de **toolbar van RTE** in de Redacteur van het Fragment van de Inhoud worden toegevoegd gebruikend het `rte` uitbreidingspunt. Dit voorbeeld toont hoe te om een douaneknoop toe te voegen genoemd _Uiteinde_ aan de toolbar van RTE en de inhoud binnen RTE te wijzigen.

Het gebruiken van `getCustomButtons()` methode van het 0} uitbreidingspunt van de uitbreiding één of vele douaneknopen kan aan de **toolbar van RTE** worden toegevoegd. `rte` Het is ook mogelijk om standaardRTE knopen zoals _Kopiëren, Deeg, Vet, en Cursief_ toe te voegen of te verwijderen gebruikend `getCoreButtons()` en `removeButtons)` methodes respectievelijk.

Dit voorbeeld toont hoe te om een benadrukte nota of uiteinde op te nemen gebruikend douane _voeg de toolbarknoop van het Uiteinde_ toe. Voor de gemarkeerde notitie of de inhoud van het uiteinde is een speciale opmaak van toepassing via HTML-elementen en de bijbehorende CSS-klassen. De inhoud van de plaatsaanduiding en de HTML-code worden ingevoegd met de callbackmethode `onClick()` van de `getCustomButtons()` .

## Extensiepunt

Dit voorbeeld breidt zich tot uitbreidingspunt `rte` uit om douaneknoop aan RTE toolbar in de Redacteur van het Fragment van de Inhoud toe te voegen.

| AEM-gebruikersinterface uitgebreid | Extensiepunt |
| ------------------------ | --------------------- | 
| [ de Redacteur van het Fragment van de Inhoud ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [ rijke Toolbar van de Redacteur van de Tekst ](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Voorbeeldextensie

Het volgende voorbeeld leidt tot _voegt de douaneknoop van het Uiteinde_ in RTE toolbar toe. De klikactie neemt de placeholder tekst bij de huidige inlasteken positie in RTE op.

De code toont hoe te om de douaneknoop met een pictogram toe te voegen en de functie van de klikmanager te registreren.

### Registratie van extensies

`ExtensionRegistration.js` , toegewezen aan de route index.html, is het ingangspunt voor de uitbreiding van AEM en bepaalt:

+ De definitie van de RTE-werkbalkknop in de functie `getCustomButtons()` met `id, tooltip and icon` -kenmerken.
+ De klikmanager voor de knoop, in de `onClick()` functie.
+ De functie clickHandler ontvangt het `state` -object als een argument om de RTE-inhoud op te halen in HTML- of tekstindeling. In dit voorbeeld wordt het echter niet gebruikt.
+ De functie van de klikmanager keert een instructieserie terug. Deze array heeft een object met `type` - en `value` -kenmerken. Als u de inhoud wilt invoegen, gebruikt het `value` -kenmerk HTML-codefragment, het `type` -kenmerk `insertContent` . Als de inhoud door gebruik kan worden vervangen, gebruikt u het instructietype `replaceContent` .

De `insertContent` -waarde is een HTML-tekenreeks, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>` . De CSS-klassen `cmp-contentfragment__element-tip` die worden gebruikt om de waarde weer te geven, zijn niet gedefinieerd in de widget, maar geïmplementeerd op het web. Dit veld Inhoudsfragment wordt weergegeven.


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
          // RTE Toolbar custom button
          getCustomButtons: () => [
            {
              id: "wknd-cf-tip", // Provide a unique ID for the custom button
              tooltip: "Add Tip", // Provide a label for the custom button
              icon: "Note", // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {
                // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [
                  {
                    type: "insertContent",
                    value:
                      '<div class="cmp-contentfragment__element-tip"><div>TIP</div><div>Add your tip text here...</div></div>',
                  },
                ];
              },
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
