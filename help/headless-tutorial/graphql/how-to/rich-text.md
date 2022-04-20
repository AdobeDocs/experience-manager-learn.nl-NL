---
title: RTF-tekst gebruiken met AEM zonder kop
description: Leer aan auteursinhoud en bedt referenced inhoud gebruikend een multi-line rijke tekstredacteur met de Fragments van de Inhoud van Adobe Experience Manager, en hoe de rijke tekst door AEM GraphQL APIs als JSON wordt geleverd om door koploze toepassingen te worden verbruikt.
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 0%

---

# RTF-tekst met AEM zonder kop

Het tekstveld Meerdere regels is een gegevenstype van inhoudsfragmenten waarmee auteurs RTF-inhoud kunnen maken. Verwijzingen naar andere inhoud, zoals afbeeldingen of andere Content Fragments, kunnen dynamisch in regel worden ingevoegd in de tekstflow. AEM GraphQL API biedt een robuuste mogelijkheid om RTF-tekst te retourneren als HTML, platte tekst of als pure JSON. De vertegenwoordiging JSON is krachtig aangezien het de cliënttoepassing volledige controle over geeft hoe te om de inhoud terug te geven.

## Meerdere regels bewerken

>[!VIDEO](https://video.tv.adobe.com/v/342104/?quality=12&learn=on)

In de Inhoudsfragmenteditor biedt de menubalk van het tekstveld Meerdere regels auteurs standaard rijke tekstopmaakmogelijkheden, zoals **vet**, *cursief* en onderstrepen. Als u het veld Meerdere regels opent in de modus Volledig scherm, wordt [aanvullende opmaakgereedschappen, zoals Alineatekst, zoeken en vervangen, spellingcontrole en meer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

## Gegevenstype meerdere regels {#multi-line-data-type}

Gebruik de **Tekst met meerdere regels** gegevenstype bij het definiëren van het inhoudsfragmentmodel om RTF-bewerkingen mogelijk te maken.

![Gegevenstype Meerdere regels met tekstopmaak](assets/rich-text/multi-line-rich-text.png)

Als u het tekstgegevenstype Meerdere regels gebruikt, kunt u het **Standaardtype** tot:

* RTF
* Markering
* Onbewerkte tekst

De **Standaardtype** Deze optie heeft rechtstreeks invloed op de bewerkingservaring en bepaalt of de tekstopties opgebouwd zijn.

U kunt ook [inline-verwijzingen inschakelen](#insert-fragment-references) aan andere Inhoudsfragmenten controleren **Fragmentverwijzing toestaan** en het vormen van **Modellen voor toegestane inhoudsfragmenten**.

## RTF-reactie met GraphQL API

Wanneer het creëren van een vraag GraphQL, kunnen de ontwikkelaars verschillende reactietypes kiezen van `html`, `plaintext`, `markdown`, en `json` uit een veld Meerdere regels.

Ontwikkelaars kunnen de [JSON-voorvertoning](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) in de Inhoudsfragmenteditor om alle waarden weer te geven van het huidige inhoudsfragment dat kan worden geretourneerd met de GraphQL-API.

### JSON-voorbeeld

De `json` biedt de meeste flexibiliteit voor front-end ontwikkelaars wanneer het werken met rijke tekstinhoud. De rijke tekstinhoud wordt geleverd als een serie van JSON knooptypes die uniek op het cliëntplatform kunnen worden verwerkt.

Hieronder ziet u een JSON-reactietype van een veld met meerdere regels met de naam `main` die een alinea bevat: &quot;*Dit is een alinea die **belangrijk**inhoud.*&quot; waarbij &quot;belangrijk&quot; wordt aangeduid als **vet**.

**GraphQL-query:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

**GraphQL respons:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### Andere voorbeelden

Hieronder staan verschillende voorbeelden van reactietypen van een veld met meerdere regels met de naam `main` die een alinea bevat: &quot;Dit is een alinea die **belangrijk** inhoud.&quot; waarbij &quot;belangrijk&quot; wordt gemarkeerd als **vet**.

+++HTML, voorbeeld

**GraphQL-query:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**GraphQL respons:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

+++voorbeeld Markering

**GraphQL-query:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**GraphQL respons:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++Voorbeeld van onbewerkte tekst

**GraphQL-query:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**GraphQL respons:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

De `plaintext` met de renderoptie wordt elke opmaak verwijderd.

+++


## Een JSON-reactie met RTF-opmaak renderen {#render-multiline-json-richtext}

De JSON-reactie van het veld Meerdere regels is gestructureerd als een hiërarchische structuur. Elk object of knooppunt vertegenwoordigt een ander HTML-blok van de RTF-tekst.

Hieronder ziet u een voorbeeld van een JSON-reactie van een tekstveld met meerdere regels. Merk op dat elk object, of knooppunt, een `nodeType` die staat voor het blok HTML van opgemaakte tekst `paragraph`, `link`, en `text`. Elk knooppunt bevat optioneel `content` Dit is een subarray met onderliggende items van het huidige knooppunt.

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

De eenvoudigste manier om de meerdere regels te renderen `json` de reactie moet elk voorwerp of knoop in de reactie verwerken en dan om het even welke kinderen van de huidige knoop verwerken. Een recursieve functie kan worden gebruikt om de JSON-boom te doorlopen.

Hieronder ziet u voorbeeldcode die een recursieve traversale aanpak illustreert. De voorbeelden zijn gebaseerd op JavaScript en gebruiken React [JSX](https://reactjs.org/docs/introducing-jsx.html)De programmeerconcepten kunnen echter op elke taal worden toegepast.

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

De `renderNodeList` function is the entrypoint into the recursive algorithm. De `renderNodeList` function verwacht een array van `childNodes`. Elk knooppunt in de array wordt vervolgens doorgegeven aan een functie `renderNode`.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

De `renderNode` functie verwacht één genoemd voorwerp `node`. Een knooppunt heeft mogelijk onderliggende items die recursief worden verwerkt met de `renderNodeList` hierboven beschreven functie. Tot slot `nodeMap` wordt gebruikt om de inhoud van de knoop terug te geven die op zijn wordt gebaseerd `nodeType`.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

De `nodeMap` is een letterlijke JavaScript-object dat wordt gebruikt als een kaart. Elk van de &quot;toetsen&quot; vertegenwoordigt een ander `nodeType`. Parameters van `node` en `children` kan worden doorgegeven aan de resulterende functies die het knooppunt renderen. Het retourneringstype dat in dit voorbeeld wordt gebruikt, is JSX, maar de benadering kan worden aangepast om een letterlijke tekenreeks op te bouwen die HTML-inhoud vertegenwoordigt.

### Voorbeeld van volledige code

Een herbruikbaar RTF-hulpprogramma is te vinden in het dialoogvenster [WKND GraphQL React, voorbeeld](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app/src/utils/renderRichText.js) - herbruikbaar hulpprogramma dat een functie toegankelijk maakt `mapJsonRichText`. Dit nut kan door componenten worden gebruikt die een rijke tekstJSON reactie als React JSX willen teruggeven.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - Voorbeeldcomponent die een GraphQL-aanvraag doet die RTF-tekst bevat. De component gebruikt de `mapJsonRichText` gebruiken om de tekst met opmaak en eventuele verwijzingen te renderen.


## In-line verwijzingen toevoegen aan RTF-tekst {#insert-fragment-references}

Met het veld Mutli-regel kunnen auteurs afbeeldingen of andere digitale elementen uit AEM Assets invoegen in de tekststroom met tekstopmaak.

![afbeelding invoegen](assets/rich-text/insert-image.png)

De bovenstaande schermafbeelding geeft een afbeelding weer die in het veld Meerdere regels is ingevoegd met behulp van de **Element invoegen** knop.

Verwijzingen naar andere inhoudsfragmenten kunnen ook worden gekoppeld aan of ingevoegd in het veld Meerdere regels met de opdracht **Inhoudsfragment invoegen** knop.

![Content fragment-verwijzing invoegen](assets/rich-text/insert-contentfragment.png)

Bovenstaande schermafbeelding toont een ander Content Fragment, Ultimate Guide to LA Skate Parks, dat wordt ingevoegd in het veld met meerdere regels. De typen inhoudsfragmenten die in het veld kunnen worden ingevoegd, worden beheerd door de **Modellen voor toegestane inhoudsfragmenten** in de [Gegevenstype meerdere regels](#multi-line-data-type) in het inhoudsfragmentmodel.

## In-line verwijzingen van de vraag met GraphQL

Met de GraphQL API kunnen ontwikkelaars een query maken die aanvullende eigenschappen bevat over verwijzingen die in een veld met meerdere regels zijn ingevoegd. De JSON-reactie bevat een aparte `_references` object dat deze extra eigenschappen opsomt. Het JSON-antwoord geeft ontwikkelaars de volledige controle over de manier waarop ze de referenties of koppelingen moeten weergeven in plaats van dat ze moeten omgaan met geadviseerde HTML.

U kunt bijvoorbeeld het volgende doen:

* Omvat douane verpletterende logica voor het beheren van verbindingen aan andere Fragmenten van de Inhoud wanneer het uitvoeren van Één enkele Toepassing van de Pagina, zoals het gebruiken van React Router of Next.js
* Een inline afbeelding renderen met het absolute pad naar een AEM-publicatie-omgeving als de `src` waarde.
* Bepaal hoe u een ingesloten verwijzing naar een ander inhoudsfragment rendert met extra aangepaste eigenschappen.

Gebruik de `json` retourneringstype en de `_references` object bij het samenstellen van een GraphQL-query:

**GraphQL-query:**

```graphql
{
  articleByPath(_path: "/content/dam/wknd/en/magazine/sample-article")
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _path
        _publishUrl
        width
      }
      ...on ArticleModel {
        _path
        author
      }
      
    }
  }
}
```

In de bovenstaande vraag, `main` wordt geretourneerd als JSON. De `_references` object bevat fragmenten voor de afhandeling van alle verwijzingen van het type `ImageRef` of van het type `ArticleModel`.

**JSON-antwoord:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "_publishUrl": "http://localhost:4503/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "width": 1920
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
        }
      ]
    }
  }
}
```

Het JSON-antwoord bevat de plaats waar de verwijzing in de RTF-tekst is ingevoegd `"nodeType": "reference"`. De `_references` bevat vervolgens elke verwijzing met de gevraagde aanvullende eigenschappen. De `ImageRef` retourneert de `width` van de afbeelding waarnaar in het artikel wordt verwezen.

## Inline-verwijzingen renderen in RTF-tekst

Voor het renderen van in-line verwijzingen wordt de recursieve benadering beschreven in [Een JSON-respons met meerdere regels renderen](#render-multiline-json-richtext) kan worden uitgebreid.

Wanneer `nodeMap` Dit is de kaart die de JSON-knooppunten rendert.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if(node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if(node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

De aanpak op hoog niveau bestaat erin elke `nodeType` equals `reference` in de Mutli Line JSON-respons. Een aangepaste renderfunctie kan vervolgens worden aangeroepen die het `_references` -object dat wordt geretourneerd in het GraphQL-antwoord.

Het in-line verwijzingspad kan vervolgens worden vergeleken met het overeenkomstige item in het dialoogvenster `_references` object en een andere aangepaste kaart `renderReference` kan worden geroepen.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._path} alt={'in-line reference'} /> 
    },
    'AdventureModel': (node) => {
        // when __typename === AdventureModel
        return <Link to={`/adventure:${node._path}`}>{`${node.adventureTitle}: ${node.adventurePrice}`}</Link>;
    }
    ...
}
```

De `__typename` van de `_references` -object kan worden gebruikt om verschillende referentietypen toe te wijzen aan verschillende renderfuncties.

### Voorbeeld van volledige code

Een volledig voorbeeld van het schrijven van een renderer van douaneverwijzingen kan in worden gevonden [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) als onderdeel van de [WKND GraphQL React, voorbeeld](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## Voorbeeld van begin tot einde

>[!VIDEO](https://video.tv.adobe.com/v/342105/?quality=12&learn=on)

De voorgaande video toont een voorbeeld van begin tot eind:

1. Het tekstveld Meerdere regels van een inhoudsfragmentmodel bijwerken om fragmentverwijzingen toe te staan
1. Met de Inhoudsfragmenteditor kunt u een afbeelding en een verwijzing naar een ander fragment opnemen in een tekstveld met meerdere regels.
1. Creating a GraphQL query that includes the Multi line text response as JSON and any `_references` gebruikt.
1. Het schrijven van React SPA dat de in-line verwijzingen van de rijke tekstreactie teruggeeft.
