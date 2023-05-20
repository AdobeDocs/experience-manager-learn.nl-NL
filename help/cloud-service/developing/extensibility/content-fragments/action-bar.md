---
title: Handelingenbalk AEM extensies van de inhoudsfragmentconsole
description: Leer hoe u een AEM ActionScript-knopextensie voor inhoudsfragmenten maakt.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: 97d26a1f-f9a7-4e57-a5ef-8bb2f3611088
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Uitbreiding van actiebalk

![Uitbreiding van actiebalk](./assets/action-bar/action-bar.png){align="center"}

Extensies die een actiebalk bevatten, introduceren een knop voor de handeling van de AEM Content Fragment Console die wordt weergegeven wanneer __1 of meer__ Inhoudsfragmenten worden geselecteerd. Omdat de knoppen voor het uitbreiden van de actiebalk alleen worden weergegeven wanneer ten minste één inhoudsfragment is geselecteerd, worden de knoppen meestal toegepast op de geselecteerde inhoudsfragmenten. Voorbeelden zijn:

+ Een bedrijfsproces of workflow voor de geselecteerde inhoudsfragmenten aanroepen.
+ De gegevens van de geselecteerde inhoudsfragmenten bijwerken of wijzigen.

## Registratie van extensies

`ExtensionRegistration.js` is het ingangspunt voor de AEM uitbreiding en bepaalt:

1. het extensietype; in het geval van een knop op de actiebalk.
1. De definitie van de extensieknop, in `getButton()` functie.
1. De klikmanager voor de knoop, in `onClick()` functie.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

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
              'id': 'example.my-action-bar-button',     // Unique ID for the button
              'label': 'My action bar button',          // Button label 
              'icon': 'Edit'                            // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },
          // Click handler for the Action Bar extension button
          onClick(selections) {
            // Action Bar buttons require the selection of at least 1 Content Fragment, 
            // so we can assume the extension will do something with these selections

            // Collect the selected content fragment paths from the selections parameter
            const selectionIds = selections.map(selection => selection.id);
            
            // Do some work with the selected content fragments
            doWork(selectionIds);          
        }
      }
    })
  }
  init().catch(console.error)
```

## Modal

![Modal](./assets/modal/modal.png)

AEM de actiebalkextensies van de Content Fragment Console zijn mogelijk vereist:

+ Extra invoer van de gebruiker om de gewenste actie uit te voeren.
+ De mogelijkheid om de gebruiker gedetailleerde informatie over het resultaat van de actie te verstrekken.

Ter ondersteuning van deze vereisten staat de extensie AEM Content Fragment Console een aangepast modaal model toe dat wordt weergegeven als een React-toepassing.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick(selections) {
    // Collect the selected content fragment paths 
    const contentFragmentPaths = selections.map(selection => selection.id);

    // Create a URL that maps to the React route to be rendered in the modal 
    const modalURL = "/index.html#" + generatePath(
      "/content-fragment/:selection/my-extension",
      {
        // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
        selection: encodeURIComponent(contentFragmentPaths.join('|'))
      }
    );

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  } ...     
} ...
```

<div class="column is-8-desktop is-full-mobile is-half-tablet" style="
    border: solid 1px #ccc;
    border-radius: 10px;
    margin: 4rem auto;
">
  <div class="is-flex is-padded-small is-padded-big-mobile">
    <div>
      <p class="has-text-weight-bold is-size-36 is-size-27-touch is-margin-bottom-big has-text-blackest">Overslaan naar het maken van een modaal</p>
      <p class="has-text-blackest">Leer hoe u een modaal object maakt dat wordt weergegeven wanneer u op de knop voor extensie op de actiebalk klikt.</p>
      <div class="has-align-start is-margin-top-big">
        <a href="./modal.md" target="_blank" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
          <span class="spectrum-Button-label has-no-wrap has-text-weight-bold" title="Leer een modaal model te maken">Leer een modaal model te maken</span>
        </a>
      </div>
    </div>
  </div>
</div>

## Geen modaal

Soms is voor AEM Action Bar-extensies voor inhoudsfragmentconsole geen verdere interactie met de gebruiker vereist, bijvoorbeeld:

+ Een back-end proces aanroepen waarvoor geen gebruikersinvoer nodig is, zoals importeren of exporteren.

In deze gevallen is voor de consolelevering AEM Content Fragment geen [modaal](#modal)en voer het werk rechtstreeks uit in de knoppen van de actiebalk `onClick` handler.

Met de AEM extensie van de Content Fragment-console kan een voortgangsindicator de AEM Content Fragment-console bedekken terwijl het werk wordt uitgevoerd, zodat de gebruiker geen verdere handelingen meer kan uitvoeren. Het gebruik van de voortgangsindicator is optioneel, maar nuttig om de voortgang van synchroon werk aan de gebruiker mee te delen.

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  guestConnection: { ...
    methods: { ...
      actionBar: { ...
        onClick(selections) {
          // Collect the selected content fragment paths 
          const contentFragmentPaths = selections.map(selection => selection.id);

          // Optionally, show the progress indicator overlay on the AEM Content Fragment console
          guestConnection.host.progressCircle.start();
          // Perform work on the selected Content Fragments
          doWork(contentFragmentPaths);
          // Hide the progress indicator overlay on the AEM Content Fragment console when the work is done
          guestConnection.host.progressCircle.stop();
        }
      }
    }
  }
}
```
