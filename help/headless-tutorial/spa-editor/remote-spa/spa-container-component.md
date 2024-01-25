---
title: Voeg bewerkbare React containercomponenten aan een Verre SPA toe
description: Leer hoe u bewerkbare containercomponenten aan een externe SPA kunt toevoegen waarmee AEM auteurs componenten naar deze componenten kunnen slepen en neerzetten.
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
duration: 399
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 0%

---

# Bewerkbare containercomponenten

[Vaste componenten](./spa-fixed-component.md) biedt enige flexibiliteit voor het ontwerpen SPA inhoud, maar deze aanpak is star en vereist dat ontwikkelaars de exacte samenstelling van de bewerkbare inhoud definiëren. Om het creëren van uitzonderlijke ervaringen door auteurs te steunen, steunt SPA Redacteur het gebruik van containercomponenten in de SPA. Met containercomponenten kunnen auteurs toegestane componenten naar de container slepen en neerzetten, en ze ontwerpen, net als bij traditionele AEM Sites-ontwerpen!

![Bewerkbare containercomponenten](./assets/spa-container-component/intro.png)

In dit hoofdstuk, voegen wij een editable container aan de huismening toe die auteurs toestaat om rijke inhoudservaringen samen te stellen en te lay-out gebruikend de Bewerkbare componenten van het Reageren direct in de SPA.

## De WKND-app bijwerken

Een containercomponent toevoegen aan de weergave Home:

+ De AEM bewerkbare component React importeren `ResponsiveGrid` component
+ Aangepaste bewerkbare reactiecomponenten (tekst en afbeelding) importeren en registreren voor gebruik in de component ResponsiveGrid

### De component ResponsiveGrid gebruiken

Een bewerkbaar gebied toevoegen aan de weergave Home:

1. Openen en bewerken `react-app/src/components/Home.js`
1. Het dialoogvenster Importeren `ResponsiveGrid` component van `@adobe/aem-react-editable-components` en voeg het toe aan `Home` component.
1. Stel de volgende kenmerken in op de knop `<ResponsiveGrid...>` component
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   Dit instrueert het `ResponsiveGrid` component om zijn inhoud van het AEM middel terug te winnen:

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   De `itemPath` worden toegewezen aan `responsivegrid` knooppunt gedefinieerd in `Remote SPA Page` AEM Sjabloon en wordt automatisch gemaakt op nieuwe AEM pagina&#39;s die zijn gemaakt op basis van de `Remote SPA Page` AEM sjabloon.

   Bijwerken `Home.js` om de `<ResponsiveGrid...>` component.

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

De `Home.js` bestand moet er als volgt uitzien:

![Home.js](./assets/spa-container-component/home-js.png)

## Bewerkbare componenten maken

Om het volledige effect van de flexibele auteurservaringscontainers in SPA Redacteur te krijgen. We hebben al een bewerkbare component Titel gemaakt, maar laten we er nog een paar maken waarmee auteurs bewerkbare componenten Tekst en Afbeelding kunnen gebruiken in de zojuist toegevoegde component ResponsiveGrid.

De nieuwe bewerkbare componenten voor tekst en reageren op afbeeldingen worden gemaakt met het bewerkbare componentdefinitiepatroon dat in [bewerkbare vaste componenten](./spa-fixed-component.md).

### Bewerkbare tekstcomponent

1. Open het SPA project in uw winde
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

![Bewerkbare tekstcomponent](./assets/spa-container-component/text-js.png)

### Afbeeldingscomponent

1. Open het SPA project in uw winde
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


1. Een SCSS-bestand maken `src/components/editable/EditableImage.scss` die aangepaste stijlen voor de `EditableImage.scss`. Deze stijlen zijn gericht op de CSS-klassen van de bewerkbare component React.
1. Voeg volgende SCSS toe aan `EditableImage.scss`

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

![Bewerkbare afbeeldingscomponent](./assets/spa-container-component/image-js.png)


### De bewerkbare componenten importeren

De nieuwe `EditableText` en `EditableImage` In de SPA wordt naar React-componenten verwezen en deze worden dynamisch geïnstantieerd op basis van de JSON die door AEM is geretourneerd. Als u ervoor wilt zorgen dat deze componenten beschikbaar zijn voor de SPA, maakt u voor deze componenten importinstructies in `Home.js`

1. Open het SPA project in uw winde
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

![Home.js](./assets/spa-container-component/home-js-imports.png)

Indien deze invoer _niet_ toegevoegd, `EditableText` en `EditableImage` de code wordt niet aangehaald door SPA, en zo, worden de componenten niet in kaart gebracht aan de verstrekte middeltypes.

## De container configureren in AEM

AEM containercomponenten gebruiken beleid om hun toegestane componenten te dicteren. Dit is een kritieke configuratie wanneer het gebruiken van SPA Redacteur, aangezien slechts AEM Componenten die SPA componententegenhangers in kaart hebben gebracht door de SPA renderbaar zijn. Zorg ervoor dat alleen de onderdelen waarvoor we SPA implementaties hebben geleverd, zijn toegestaan:

+ `EditableTitle` toegewezen aan `wknd-app/components/title`
+ `EditableText` toegewezen aan `wknd-app/components/text`
+ `EditableImage` toegewezen aan `wknd-app/components/image`

De reponsivegrid-container van de sjabloon Externe SPA pagina configureren:

1. Aanmelden bij AEM auteur
1. Navigeren naar __Gereedschappen > Algemeen > Sjablonen > WKND App__
1. Bewerken __SPA pagina rapporteren__

   ![Beleid voor responsieve rasters](./assets/spa-container-component/templates-remote-spa-page.png)

1. Selecteren __Structuur__ in de modusschakelaar rechtsboven
1. Tik om de __Layout Container__
1. Tik op de knop __Beleid__ pictogram in de pop-upbalk

   ![Beleid voor responsieve rasters](./assets/spa-container-component/templates-policies-action.png)

1. Rechts, onder de __Toegestane componenten__ tab, uitbreiden __WKND APP - INHOUD__
1. Zorg ervoor dat alleen het volgende is geselecteerd:
   + Afbeelding
   + Tekst
   + Titel

   ![Externe SPA](./assets/spa-container-component/templates-allowed-components.png)

1. Tikken __Gereed__

## De container ontwerpen in AEM

Nadat de SPA is bijgewerkt om de `<ResponsiveGrid...>`, omvat drie bewerkbare React-componenten (`EditableTitle`, `EditableText`, en `EditableImage`), en AEM wordt bijgewerkt met een passend beleid van het Malplaatje, kunnen wij beginnen inhoud in de containercomponent te ontwerpen.

1. Aanmelden bij AEM auteur
1. Navigeren naar __Sites > WKND App__
1. Tikken __Home__ en selecteert u __Bewerken__ van de bovenste actiebalk
   + De componentenvertoningen van de Tekst van de &quot;Wereld van Hello&quot;aangezien dit automatisch werd toegevoegd toen het produceren van het project van het archetype van het AEM Project
1. Selecteren __Bewerken__ in de modus Selecteren rechtsboven in de Pagina-editor
1. Zoek de __Layout Container__ bewerkbaar gebied onder Titel
1. Open de __Zijbalk van de pagina-editor__ en selecteert u de __Weergave Componenten__
1. Sleep de volgende componenten naar de __Layout Container__
   + Afbeelding
   + Titel
1. Sleep de componenten om deze in de volgende volgorde te plaatsen:
   1. Titel
   1. Afbeelding
   1. Tekst
1. __Auteur__ de __Titel__ component
   1. Tik op de component Title en tik op de knop __moersleutel__ pictogram naar __bewerken__ de component Title
   1. Voeg de volgende tekst toe:
      + Titel: __De zomer komt eraan. Laten we er het beste van maken!__
      + Type: __H1__
   1. Tikken __Gereed__
1. __Auteur__ de __Afbeelding__ component
   1. Sleep een afbeelding vanuit de zijbalk (na het schakelen naar de weergave Elementen) naar de component Afbeelding
   1. Tik op de component Image en tik op de __moersleutel__ pictogram dat u wilt bewerken
   1. Controleer de __Afbeelding is decoratief__ selectievakje
   1. Tikken __Gereed__
1. __Auteur__ de __Tekst__ component
   1. Bewerk de component Text door op de component Text te tikken en op de component Text te tikken __moersleutel__ pictogram
   1. Voeg de volgende tekst toe:
      + _Op dit moment kunt u 15% krijgen voor alle avonturen van één week, en 20% voor alle avonturen die 2 weken of langer zijn! Voeg bij de kassa de campagnecode SUMMERISCOMING toe om uw kortingen te krijgen!_
   1. Tikken __Gereed__

1. Uw componenten zijn nu ontworpen, maar worden verticaal gestapeld.

   ![Gemaakte componenten](./assets/spa-container-component/authored-components.png)

Gebruik AEM lay-outmodus om de grootte en lay-out van de componenten aan te passen.

1. Overschakelen op __Lay-outmodus__ met de moduskiezer rechtsboven
1. __Formaat wijzigen__ de componenten Afbeelding en Tekst, zodat deze naast elkaar staan
   + __Afbeelding__ onderdeel moet __8 kolommen breed__
   + __Tekst__ onderdeel moet __3 kolommen breed__

   ![Indelingscomponenten](./assets/spa-container-component/layout-components.png)

1. __Voorvertoning__ uw wijzigingen in AEM paginaeditor
1. De WKND-app vernieuwen die lokaal wordt uitgevoerd [http://localhost:3000](http://localhost:3000) om de gemaakte wijzigingen te zien!

   ![Containercomponent in SPA](./assets/spa-container-component/localhost-final.png)


## Gefeliciteerd!

U hebt een containercomponent toegevoegd waarmee bewerkbare componenten door auteurs aan de WKND App kunnen worden toegevoegd! Nu weet u hoe:

+ De bewerkbare component AEM React gebruiken `ResponsiveGrid` in de SPA
+ Bewerkbare React-componenten (tekst en afbeelding) maken en registreren voor gebruik in de SPA via de containercomponent
+ Vorm het Verre malplaatje van de Pagina van de SPA om de SPA-toegelaten componenten toe te staan
+ Bewerkbare componenten toevoegen aan de containercomponent
+ Auteur- en indelingscomponenten in SPA Editor

## Volgende stappen

In de volgende stap wordt dezelfde techniek gebruikt om [Voeg een editable component aan een route van de Details van het Avontuur toe](./spa-dynamic-routes.md) in de SPA.
