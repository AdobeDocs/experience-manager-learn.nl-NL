---
title: Aangepaste knop toevoegen aan Rich Text Editor (RTE)-werkbalk
description: Leer hoe u een aangepaste knop toevoegt aan de Rich Text Editor (RTE)-werkbalk in de AEM Content Fragment Editor
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 2b72c282-bce8-4f2a-bce6-f2f31e96ec88
source-git-commit: 6f537a0c7605b96f6c6b43ff8c5bf634369171cc
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Aangepaste knop toevoegen aan Rich Text Editor (RTE)-werkbalk

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

U kunt aangepaste knoppen toevoegen aan de **RTE, werkbalk** in de Inhoudsfragmenteditor met de opdracht `rte` extensiepunt. In dit voorbeeld wordt getoond hoe u een aangepaste knop met de naam _Tip toevoegen_ aan de toolbar van RTE en wijzig de inhoud binnen RTE.

Gebruiken `rte` extensiepunt `getCustomButtons()` methode één of vele douaneknopen kan aan worden toegevoegd **RTE, werkbalk**. Het is ook mogelijk om standaardRTE knopen zoals toe te voegen of te verwijderen _Kopiëren, Plakken, Vet en Cursief_ gebruiken `getCoreButtons()` en `removeButtons)` methoden.

In dit voorbeeld ziet u hoe u een gemarkeerde notitie of tip invoegt met behulp van aangepaste _Tip toevoegen_ werkbalkknop. Voor de gemarkeerde notitie of de inhoud van het uiteinde wordt een speciale opmaak toegepast via HTML-elementen en de bijbehorende CSS-klassen. De inhoud en de HTML-code van de tijdelijke aanduiding worden ingevoegd met de `onClick()` callback-methode van de `getCustomButtons()`.

## Extensiepunt

In dit voorbeeld wordt het uitbreidingspunt uitgebreid `rte` om een aangepaste knop toe te voegen aan de RTE-werkbalk in de Inhoudsfragmenteditor.

| AEM UI uitgebreid | Extensiepunt |
| ------------------------ | --------------------- | 
| [Inhoudsfragmenteditor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Werkbalk RTF-editor](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Voorbeeldextensie

In het volgende voorbeeld wordt een _Tip toevoegen_ douaneknoop in de toolbar van RTE. De klikactie neemt de placeholder tekst bij de huidige inlasteken positie in RTE op.

De code toont hoe te om de douaneknoop met een pictogram toe te voegen en de functie van de klikmanager te registreren.

### Registratie van extensies

`ExtensionRegistration.js`, toegewezen aan de route index.html, is het ingangspunt voor de AEM uitbreiding en bepaalt:

+ De definitie van de RTE-werkbalkknop in `getCustomButtons()` functie met `id, tooltip and icon` kenmerken.
+ De klikmanager voor de knoop, in `onClick()` functie.
+ De functie van de klikmanager ontvangt `state` object als een argument om de inhoud van de RTE op te halen in HTML- of tekstindeling. In dit voorbeeld wordt het echter niet gebruikt.
+ De functie van de klikmanager keert een instructieserie terug. Deze array bevat een object met `type` en `value` kenmerken. Als u de inhoud wilt invoegen, `value` attributen HTML code fragment, the `type` het attribuut gebruikt `insertContent`. Als er een gebruiksgeval is om de inhoud te vervangen, gebruikt u de optie `replaceContent` instructietype.

De `insertContent` value is a HTML string, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. De CSS-klassen `cmp-contentfragment__element-tip` gebruikt om de waarde weer te geven, worden niet gedefinieerd in de widget, maar geïmplementeerd op het web. Dit veld Inhoudsfragment wordt weergegeven.


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

          // RTE Toolbar custom button
          getCustomButtons: () => ([
            {
              id: "wknd-cf-tip",       // Provide a unique ID for the custom button
              tooltip: "Add Tip",      // Provide a label for the custom button
              icon: 'Note',            // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {    // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [{
                  type: "insertContent",
                  value: "<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>"
                }];
              },
            },
          ]),
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```
