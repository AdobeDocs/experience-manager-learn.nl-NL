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
duration: 190
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 0%

---

# Hoe AEM React Editable Components v2 te gebruiken

{{edge-delivery-services}}

AEM verstrekt [ AEM React Editable Componenten v2 ](https://www.npmjs.com/package/@adobe/aem-react-editable-components), een op Node.js-Gebaseerde SDK die de verwezenlijking van componenten van het Reageren toestaat, die in-context component het uitgeven gebruikend AEM Redacteur steunen SPA.

+ [ npm module ](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [ het project van Github ](https://github.com/adobe/aem-react-editable-components)
+ [ documentatie van de Adobe ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


Raadpleeg de technische documentatie voor meer informatie en codevoorbeelden voor AEM React Editable Components v2:

+ [ Integratie met AEM documentatie ](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [ Bewerkbare componentendocumentatie ](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [ Helpers documentatie ](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM pagina&#39;s

AEM bewerkbare componenten Reageren werken met zowel de SPA Editor als de externe SPA React-apps. De inhoud die de editable componenten bevolkt van de Reactie, moet via AEM pagina&#39;s worden blootgesteld die de [ SPA component van de Pagina ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html) uitbreiden. AEM componenten, die kaarten aan editable componenten van het Reageren, moeten AEM [ kader van de Exporteur van de Component ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) uitvoeren - zoals [ AEM de Componenten van WCM van de Kern ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html).


## Afhankelijkheden

Zorg ervoor dat de React-app actief is op Node.js 14+.

De minimale set afhankelijkheden die de React-app gebruikt AEM React Editable Components v2 zijn: `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping` en `@adobe/aem-spa-page-model-manager` .


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
> [ AEM Reageer de Basis van de Componenten WCM van de Reageren Kern ](https://github.com/adobe/aem-react-core-wcm-components-base) en [ AEM Reageren de Componenten van WCM SPA ](https://github.com/adobe/aem-react-core-wcm-components-spa) zijn niet compatibel met AEM React Editable Componenten v2.

## SPA Editor

Wanneer u de AEM bewerkbare componenten React gebruikt met een SPA React-app op basis van de Editor, de AEM `ModelManager` SDK, als SDK:

1. Hiermee wordt inhoud opgehaald van AEM
1. De React Eetbare componenten met AEM inhoud vullen

Plaats de React-app met een geïnitialiseerde ModelManager en geef de React-app weer. De React-app moet één instantie van de `<Page>` -component bevatten die is geëxporteerd uit `@adobe/aem-react-editable-components` . De component `<Page>` heeft logica voor het dynamisch maken van React-componenten op basis van de `.model.json` die door AEM wordt verschaft.

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

`<Page>` wordt doorgegeven als de weergave van de AEM pagina als JSON, via de lus `pageModel` die wordt geleverd door de `ModelManager` . De component `<Page>` maakt dynamisch React-componenten voor objecten in de `pageModel` door de `resourceType` aan te passen aan een component React die zich via `MapTo(..)` registreert bij het middeltype.

## Bewerkbare componenten

De `<Page>` wordt via de `ModelManager` doorgegeven aan de weergave van de AEM pagina als JSON. De component `<Page>` maakt vervolgens dynamisch React-componenten voor elk object in de JSON door de waarde `resourceType` van het JS-object te koppelen aan een React-component die zich via de `MapTo(..)` -aanroep van de component registreert bij het brontype. Het volgende wordt bijvoorbeeld gebruikt om een instantie te instantiëren

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
1. De instantie &#39;niet-bewerkbaar&#39; van de component React moet worden ingesloten in plaats van de instantie &#39;bewerkbaar&#39; die met `<EditableComponent>` is ingesloten. Als de ingesloten component de `<EditableComponent>` -omslag heeft, probeert de SPA Editor de binnencomponent met de bewerkingsinterface (blauw aanwijsvak) aan te passen in plaats van met de buitenste insluitcomponent.

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
