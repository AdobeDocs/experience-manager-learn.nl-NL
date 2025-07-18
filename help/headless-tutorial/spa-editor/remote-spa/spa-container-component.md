---
title: Voeg editable React containercomponenten aan een Verre SPA toe
description: Leer hoe te om editable containercomponenten aan verSPA toe te voegen die de auteurs van AEM toestaan om componenten in hen te slepen en te laten vallen.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
duration: 306
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 0%

---

# Bewerkbare containercomponenten

{{spa-editor-deprecation}}

[ Vaste componenten ](./spa-fixed-component.md) verstrekken wat flexibiliteit voor het ontwerpen van de inhoud van het KUUROORD, nochtans is deze benadering stijf en vereist ontwikkelaars om de nauwkeurige samenstelling van de editable inhoud te bepalen. Om de verwezenlijking van uitzonderlijke ervaringen door auteurs te steunen, steunt de Redacteur van het KUUROORD het gebruik van containercomponenten in het KUUROORD. Met containercomponenten kunnen auteurs toegestane componenten naar de container slepen en neerzetten, en ze ontwerpen, net als bij traditionele AEM Sites-ontwerpen!

![ Bewerkbare containercomponenten ](./assets/spa-container-component/intro.png)

In dit hoofdstuk, voegen wij een editable container aan de huismening toe die auteurs toestaat om rijke inhoudservaringen samen te stellen en te lay-out gebruikend de Bewerkbare componenten van het Reageren direct in het KUUROORD.

## De WKND-app bijwerken

Een containercomponent toevoegen aan de weergave Home:

* De component `ResponsiveGrid` van de AEM React Editable-component importeren
* Aangepaste bewerkbare reactiecomponenten (tekst en afbeelding) importeren en registreren voor gebruik in de component ResponsiveGrid

### De component ResponsiveGrid gebruiken

Een bewerkbaar gebied toevoegen aan de weergave Home:

1. Openen en bewerken `react-app/src/components/Home.js`
1. Importeer de `ResponsiveGrid` -component vanuit `@adobe/aem-react-editable-components` en voeg deze toe aan de `Home` -component.
1. De volgende kenmerken instellen voor de component `<ResponsiveGrid...>`
   1. `pagePath = '/content/wknd-app/us/en/home'`
   1. `itemPath = 'root/responsivegrid'`

   Hierdoor krijgt de component `ResponsiveGrid` de opdracht om de inhoud ervan op te halen uit de AEM-bron:

   1. `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   De `itemPath` verwijst naar het knooppunt `responsivegrid` dat is gedefinieerd in de `Remote SPA Page` AEM-sjabloon en wordt automatisch gemaakt op nieuwe AEM-pagina&#39;s die zijn gemaakt op basis van de `Remote SPA Page` AEM-sjabloon.

   Werk `Home.js` bij om de component `<ResponsiveGrid...>` toe te voegen.

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

Het bestand `Home.js` moet er als volgt uitzien:

![ Home.js ](./assets/spa-container-component/home-js.png)

## Bewerkbare componenten maken

Om het volledige effect van de flexibele auteurservaringscontainers te krijgen verstrekt in de Redacteur van het KUUROORD. We hebben al een bewerkbare component Titel gemaakt, maar laten we er nog een paar maken waarmee auteurs bewerkbare componenten Tekst en Afbeelding kunnen gebruiken in de zojuist toegevoegde component ResponsiveGrid.

De nieuwe editable componenten van het Reageren van de Tekst en van het Beeld worden gecreeerd gebruikend het editable patroon van de componentendefinitie die in [ wordt uitgevoerd editable vaste componenten ](./spa-fixed-component.md).

### Bewerkbare tekstcomponent

1. Open het project van het KUUROORD in uw winde
1. Een component React maken op `src/components/editable/core/Text.js`
1. De volgende code toevoegen aan `Text.js`

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. Een bewerkbare React-component maken op `src/components/editable/EditableText.js`
1. De volgende code toevoegen aan `EditableText.js`

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

De bewerkbare implementatie van de component Text moet er als volgt uitzien:

![ Editable tekstcomponent ](./assets/spa-container-component/text-js.png)

### Afbeeldingscomponent

1. Open het project van het KUUROORD in uw winde
1. Een component React maken op `src/components/editable/core/Image.js`
1. De volgende code toevoegen aan `Image.js`

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. Een bewerkbare React-component maken op `src/components/editable/EditableImage.js`
1. De volgende code toevoegen aan `EditableImage.js`

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. Maak een SCSS-bestand `src/components/editable/EditableImage.scss` dat aangepaste stijlen voor de `EditableImage.scss` bevat. Deze stijlen zijn gericht op de CSS-klassen van de bewerkbare component React.
1. Voeg de volgende SCSS toe aan `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. Importeren `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

De bewerkbare implementatie van de component Image moet er als volgt uitzien:

![ Bewerkbare beeldcomponent ](./assets/spa-container-component/image-js.png)


### De bewerkbare componenten importeren

De nieuwe `EditableText` - en `EditableImage` React-componenten worden vermeld in de SPA en worden dynamisch geïnstantieerd op basis van de JSON die door AEM wordt geretourneerd. Om ervoor te zorgen dat deze componenten aan SPA beschikbaar zijn, creeer de invoerverklaringen voor hen in `Home.js`

1. Open het project van het KUUROORD in uw winde
1. Het bestand openen `src/Home.js`
1. Importeerinstructies toevoegen voor `AEMText` en `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

Het resultaat moet er als volgt uitzien:

![ Home.js ](./assets/spa-container-component/home-js-imports.png)

Als deze invoer _niet_ wordt toegevoegd, wordt de `EditableText` en `EditableImage` code niet aangehaald door SPA, en zo, worden de componenten niet in kaart gebracht aan de verstrekte middeltypes.

## De container configureren in AEM

AEM-containercomponenten gebruiken beleid om toegestane componenten voor te schrijven. Dit is een kritieke configuratie wanneer het gebruiken van de Redacteur van het KUUROORD, aangezien slechts de Componenten van AEM die de componententegenhangers van het KUUROORD in kaart hebben gebracht door het KUUROORD kunnen worden teruggegeven. Verzeker slechts de componenten waarvoor wij implementaties van het KUUROORD hebben verstrekt worden toegestaan:

* `EditableTitle` toegewezen aan `wknd-app/components/title`
* `EditableText` toegewezen aan `wknd-app/components/text`
* `EditableImage` toegewezen aan `wknd-app/components/image`

Om de Verre container van het malplaatje van de Pagina van het KUUROORD te vormen reponsivegrid:

1. Aanmelden bij AEM-auteur
1. Navigeer aan __Hulpmiddelen > Algemeen > Malplaatjes > app WKND__
1. Bewerk __Pagina van het Rapport SPA__

   ![ Responsief beleid van het Net ](./assets/spa-container-component/templates-remote-spa-page.png)

1. Selecteer __Structuur__ op de wijzeschakelaar in het hoogste recht
1. Tik om de __Container van de Lay-out__ te selecteren
1. Tik het __pictogram van het Beleid__ in popup bar

   ![ Responsief beleid van het Net ](./assets/spa-container-component/templates-policies-action.png)

1. Op het recht, onder __Toegestane Componenten__ lusje, breid __APP WKND uit - INHOUD__
1. Zorg ervoor dat alleen het volgende is geselecteerd:
   1. Afbeelding
   1. Tekst
   1. Titel

   ![ Verre Pagina van het KUUROORD ](./assets/spa-container-component/templates-allowed-components.png)

1. Tik __Gereed__

## De container ontwerpen in AEM

Nadat SPA wordt bijgewerkt om `<ResponsiveGrid...>` in te bedden, verpakt voor drie editable componenten van het Reageren (`EditableTitle`, `EditableText`, en `EditableImage`), en AEM met een passend beleid van het Malplaatje wordt bijgewerkt, kunnen wij beginnen inhoud in de containercomponent te ontwerpen.

1. Aanmelden bij AEM-auteur
1. Navigeer aan __Plaatsen > app WKND__
1. Tik __Huis__ en selecteer __uitgeven__ van de hoogste actiebar
   1. De component &quot;Hello World&quot; Tekst wordt weergegeven, aangezien dit automatisch is toegevoegd bij het genereren van het project vanuit het AEM Project archetype
1. Selecteer __uitgeven__ van de wijze-selecteur in het hoogste recht van de Redacteur van de Pagina
1. Bepaal de plaats van de __Container van de Lay-out__ editable gebied onder de Titel
1. Open de __zijbar van de Redacteur van de Pagina__, en selecteer de __mening van Componenten__
1. Sleep de volgende componenten in de __Container van de Lay-out__
   1. Afbeelding
   1. Titel
1. Sleep de componenten om deze in de volgende volgorde te plaatsen:
   1. Titel
   1. Afbeelding
   1. Tekst
1. __Auteur__ de __3 component van de Titel &lbrace;__
   1. Tik de component van de Titel, en tik het __moersleutelpictogram__ ____ de component van de Titel uitgeven
   1. Voeg de volgende tekst toe:
      1. Titel: __Zomer komt, laten we het meesten van het maken!__
      1. Type: __H1__
   1. Tik __Gereed__
1. __Auteur__ de __component van het Beeld__
   1. Sleep een afbeelding vanuit de zijbalk (na het schakelen naar de Assets-weergave) naar de afbeeldingscomponent
   1. Tik de component van het Beeld, en tik het __moersleutel__ pictogram om uit te geven
   1. Controle het __Beeld is decoratieve__ checkbox
   1. Tik __Gereed__
1. __Auteur__ de ____ component van de Tekst
   1. Bewerk de component van de Tekst door de component van de Tekst te tikken, en het tikken van het __moersleutelpictogram__
   1. Voeg de volgende tekst toe:
      1. _Op dit ogenblik, kunt u 15% op alle avonturen van 1 week krijgen, en 20% korting op alle avonturen die 2 weken of langer zijn! Bij controle, voeg de campagnecode toe SUMMERISCOMING om uw kortingen te krijgen!_
   1. Tik __Gereed__

1. Uw componenten zijn nu ontworpen, maar worden verticaal gestapeld.

   ![ Authored componenten ](./assets/spa-container-component/authored-components.png)

   Gebruik de AEM-lay-outmodus om de grootte en lay-out van de componenten aan te passen.

1. Schakelaar aan __Wijze van de Lay-out__ gebruikend de wijze-selecteur in top-right
1. __vergroot__ de componenten van het Beeld en van de Tekst, dusdanig dat zij zij aan zij zijn
   1. __de component van het 1&rbrace; Beeld__ zou 8 kolommen breed __moeten zijn__
   1. __de component van de Tekst__ __3 kolommen breed__ zou moeten zijn

   ![ componenten van de Lay-out ](./assets/spa-container-component/layout-components.png)

1. __Voorproef__ uw veranderingen in de Redacteur van de Pagina van AEM
1. Vernieuw de WKND App die plaatselijk op [ http://localhost:3000 ](http://localhost:3000) loopt om de authored veranderingen te zien!

   ![ component van de Container in KUUROORD ](./assets/spa-container-component/localhost-final.png)


## Gefeliciteerd!

U hebt een containercomponent toegevoegd waarmee bewerkbare componenten door auteurs aan de WKND App kunnen worden toegevoegd! Nu weet u hoe:

* De AEM React Editable-component `ResponsiveGrid` in de SPA gebruiken
* Creeer en registreer editable React componenten (Tekst en Beeld) voor gebruik in SPA via de containercomponent
* Vorm het Verre malplaatje van de Pagina van SPA om de SPA-Toegelaten componenten toe te staan
* Bewerkbare componenten toevoegen aan de containercomponent
* Auteur en lay-outcomponenten in de Redacteur van het KUUROORD

## Volgende stappen

De volgende stap gebruikt deze zelfde techniek om [ een editable component aan een route van de Details van het Avontuur ](./spa-dynamic-routes.md) in het KUUROORD toe te voegen.
