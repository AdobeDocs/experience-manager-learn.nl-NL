---
title: Hoe AEM React Editable Components v2 te gebruiken
description: Leer hoe u AEM React Editable Components v2 kunt gebruiken om een React-app te activeren.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# Hoe AEM React Editable Components v2 te gebruiken

{{edge-delivery-services}}

AEM biedt [AEM bewerkbare componenten React v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), een op Node.js-Gebaseerde SDK die de verwezenlijking van componenten van het Reageren toestaat, die in-context component het uitgeven gebruikend AEM Redacteur steunen SPA.

+ [npm-module](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Github-project](https://github.com/adobe/aem-react-editable-components)
+ [Documentatie Adobe](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Raadpleeg de technische documentatie voor meer informatie en codevoorbeelden voor AEM React Editable Components v2:

+ [Integratie met AEM documentatie](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [Bewerkbare componentdocumentatie](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [Helpers, documentatie](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM pagina&#39;s

AEM bewerkbare componenten Reageren werken met zowel de SPA Editor als de externe SPA React-apps. Inhoud die de bewerkbare React-componenten vult, moet zichtbaar zijn via AEM pagina&#39;s die de [SPA](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). AEM componenten, die aan editable React componenten in kaart brengen, moeten AEM uitvoeren [Component Exporter-framework](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) - zoals [AEM Core WCM-componenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html).


## Afhankelijkheden

Zorg ervoor dat de React-app actief is op Node.js 14+.

De minimale set afhankelijkheden die de React-app kan gebruiken AEM React Editable Components v2 zijn: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`, en  `@adobe/aem-spa-page-model-manager`.


+ `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [AEM Reageren op basis van WCM-componenten](https://github.com/adobe/aem-react-core-wcm-components-base) en [AEM Reageren op WCM-SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) zijn niet compatibel met AEM React Editable Components v2.

## SPA Editor

Als u de AEM Bewerkbare componenten Reageren gebruikt met een React-app die is gebaseerd op SPA Editor, AEM `ModelManager` SDK, als SDK:

1. Hiermee wordt inhoud opgehaald van AEM
1. Hiermee vult u de bewerkbare componenten React met AEM inhoud in

Plaats de React-app met een geïnitialiseerde ModelManager en geef de React-app weer. De React-app moet één instantie van de `<Page>` component geëxporteerd uit `@adobe/aem-react-editable-components`. De `<Page>` component heeft logica om React componenten dynamisch te creëren die op `.model.json` verstrekt door AEM.

+ `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

De `<Page>` wordt doorgegeven als de weergave van de AEM pagina als JSON, via de `pageModel` door de `ModelManager`. De `<Page>` component maakt dynamisch React-componenten voor objecten in de `pageModel` door de `resourceType` met een component React die zich bij het middeltype via registreert `MapTo(..)`.

## Bewerkbare componenten

De `<Page>` wordt de weergave van de AEM pagina als JSON doorgegeven via de `ModelManager`. De `<Page>` maakt vervolgens dynamisch React-componenten voor elk object in de JSON door de JS-objecten op elkaar af te stemmen `resourceType` waarde met een component React die zich aan het middeltype via de component registreert `MapTo(..)` oproepen. Het volgende wordt bijvoorbeeld gebruikt om een instantie te instantiëren

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

De bovenstaande JSON die door AEM wordt geboden, kan worden gebruikt om een bewerkbare React-component dynamisch te instantiëren en te vullen.

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## Componenten insluiten

Bewerkbare componenten kunnen opnieuw worden gebruikt en in elkaar worden ingesloten. Er zijn twee belangrijke overwegingen wanneer het inbedden van één editable component in een andere:

1. De JSON-inhoud van AEM voor de insluitingscomponent moet de inhoud bevatten om aan de ingesloten componenten te voldoen. Dit wordt gedaan door een dialoog voor de AEM component te creëren die de vereiste gegevens verzamelt.
1. De instantie &quot;niet-bewerkbaar&quot; van de component React moet worden ingesloten in plaats van de instantie &quot;bewerkbaar&quot; die is ingesloten met `<EditableComponent>`. De reden is dat de ingesloten component de `<EditableComponent>` verpakt, probeert de SPA Editor de binnencomponent te kleden met de bewerkingsinterface (blauwe aanwijsdoos) in plaats van met de buitenste insluitcomponent.

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

Bovenstaande JSON die door AEM wordt verstrekt zou kunnen worden gebruikt om een editable component van de Reactie dynamisch te concretiseren en te bevolken die een andere component van de React inbedt.


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```
