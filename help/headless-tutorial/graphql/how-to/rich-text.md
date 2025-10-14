---
title: RTF-tekst gebruiken met AEM Headless
description: Leer om inhoud te schrijven en referenced inhoud in te bedden gebruikend een multi-line rijke tekstredacteur met de Fragments van de Inhoud van Adobe Experience Manager, en hoe de rijke tekst door AEM GraphQL APIs als JSON wordt geleverd om door koploze toepassingen te worden verbruikt.
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
duration: 785
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1409'
ht-degree: 0%

---

# RTF-tekst met AEM Headless

Het tekstveld met meerdere regels is een gegevenstype van inhoudsfragmenten waarmee auteurs RTF-inhoud kunnen maken. Verwijzingen naar andere inhoud, zoals afbeeldingen of andere Content Fragments, kunnen dynamisch in regel worden ingevoegd in de tekstflow. Het tekstveld Eén regel is een ander gegevenstype van inhoudsfragmenten dat moet worden gebruikt voor eenvoudige tekstelementen.

AEM GraphQL API biedt een robuuste mogelijkheid om RTF-tekst te retourneren als HTML, normale tekst of pure JSON. De vertegenwoordiging JSON is krachtig aangezien het de cliënttoepassing volledige controle over geeft hoe te om de inhoud terug te geven.

## Meerdere regels bewerken

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

In de Redacteur van het Fragment van de Inhoud, verstrekt de het menubar van het multi-line tekstgebied auteurs van standaard rijke tekst het formatteren mogelijkheden, zoals **gewaagd**, *cursief*, en onderstreept. Het openen van het multi-lijngebied op het volledige schermwijze laat [&#x200B; extra het formatteren hulpmiddelen zoals het type van Paragraaf toe, vinden en vervangen, spellingcontrole, en meer &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html?lang=nl-NL).

>[!NOTE]
>
> De rijke tekststoppen in de multi-line redacteur kunnen niet worden aangepast.

## Tekstgegevenstype van meerdere regels {#multi-line-data-type}

Gebruik het **multi-line tekst** gegevenstype wanneer het bepalen van uw Model van het Fragment van de Inhoud om rijke tekst toe te laten creeert.

![&#x200B; Meerdere Lijn het type van rijke tekstgegevens &#x200B;](assets/rich-text/multi-line-rich-text.png)

Verschillende eigenschappen van het veld met meerdere regels kunnen worden geconfigureerd.

**teruggeeft als** bezit kan aan worden geplaatst:

* Tekstgebied - geeft één veld met meerdere regels weer
* Meerdere velden - geeft meerdere velden voor Mutli-lijnen weer


Het **StandaardType** kan aan worden geplaatst:

* RTF
* Markering
* Onbewerkte tekst

De **optie StandaardType** beïnvloedt direct de het uitgeven ervaring en bepaalt als de rijke teksthulpmiddelen aanwezig zijn.

U kunt [&#x200B; ook toelaten in-line verwijzingen &#x200B;](#insert-fragment-references) aan andere Fragmenten van de Inhoud door **te controleren toestaan Verwijzing van het Fragment** en het vormen van **Toegestane Modellen van het Fragment van de Inhoud**.

Controleer het **Vertaalbare** vakje, als de inhoud moet worden gelokaliseerd. Alleen RTF en normale tekst kunnen worden gelokaliseerd. Zie [&#x200B; werkend met gelokaliseerde inhoud voor meer details &#x200B;](./localized-content.md).

## Rijke tekstreactie met GraphQL API

Wanneer ontwikkelaars een GraphQL-query maken, kunnen ze verschillende antwoordtypen kiezen in `html` , `plaintext` , `markdown` en `json` in een veld met meerdere regels.

De ontwikkelaars kunnen de [&#x200B; Voorproef JSON &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html?lang=nl-NL) in de redacteur van het Fragment van de Inhoud gebruiken om alle waarden van het huidige Fragment van de Inhoud te tonen dat kan worden teruggekeerd gebruikend GraphQL API.

## GraphQL-query voortgezet

Als u de responsindeling `json` voor het veld met meerdere regels selecteert, hebt u de meeste flexibiliteit bij het werken met RTF-inhoud. De rijke tekstinhoud wordt geleverd als een serie van JSON knooptypes die uniek op het cliëntplatform kunnen worden verwerkt.

Hieronder is een JSON reactietype van een multi-line genoemd gebied `main` dat een paragraaf bevat: &quot;*Dit is een paragraaf die **belangrijke**&#x200B;inhoud omvat.*&quot;waar &quot;belangrijk&quot;als **gewaagd** duidelijk is.

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

Voor de variabele `$path` die in het filter `_path` wordt gebruikt, is het volledige pad naar het inhoudsfragment vereist (bijvoorbeeld `/content/dam/wknd/en/magazine/sample-article` ).

**reactie van GraphQL:**

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

Hieronder zijn verscheidene voorbeelden van reactietypen van een multi-line genoemd gebied `main` dat een paragraaf bevat: &quot;Dit is een paragraaf die **belangrijke** inhoud omvat.&quot; waar &quot;belangrijk&quot;als **gewaagd** duidelijk is.

+++HTML, voorbeeld

**GraphQL bleef vraag:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

**reactie van GraphQL:**

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

**GraphQL bleef vraag:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

**reactie van GraphQL:**

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

**GraphQL bleef vraag:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
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

**reactie van GraphQL:**

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

Met de renderoptie `plaintext` wordt elke opmaak verwijderd.

+++


## Een JSON-reactie met RTF-opmaak renderen {#render-multiline-json-richtext}

De JSON-reactie van het veld met meerdere regels is gestructureerd als een hiërarchische structuur. Elk object of knooppunt vertegenwoordigt een ander HTML-blok met opgemaakte tekst.

Hieronder ziet u een voorbeeld van een JSON-reactie van een tekstveld met meerdere regels. Elk object, of knooppunt, bevat een `nodeType` die het HTML-blok van de opgemaakte tekst zoals `paragraph` , `link` en `text` vertegenwoordigt. Elk knooppunt bevat optioneel `content` dat een subarray is met onderliggende knooppunten van het huidige knooppunt.

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

De eenvoudigste manier om de multiline `json` reactie te renderen is om elk object, of knooppunt, in de reactie te verwerken en vervolgens onderliggende knooppunten van het huidige knooppunt te verwerken. Een recursieve functie kan worden gebruikt om de JSON-boom te doorlopen.

Hieronder ziet u voorbeeldcode die een recursieve traversale aanpak illustreert. De steekproeven zijn JavaScript gebaseerd en gebruiken React [&#x200B; JSX &#x200B;](https://reactjs.org/docs/introducing-jsx.html), nochtans kunnen de programmeringsconcepten op om het even welke taal worden toegepast.

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

`renderNodeList` is een recursieve functie die een array van `childNodes` neemt. Elk knooppunt in de array wordt vervolgens doorgegeven aan een functie `renderNode` , die op zijn beurt `renderNodeList` aanroept wanneer het knooppunt onderliggende knooppunten heeft.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

De functie `renderNode` verwacht één object met de naam `node` . Een knooppunt kan onderliggende knooppunten hebben die recursief worden verwerkt met de hierboven beschreven functie `renderNodeList` . Ten slotte wordt een `nodeMap` gebruikt om de inhoud van het knooppunt te renderen op basis van de `nodeType` ervan.

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

`nodeMap` is een letterlijke JavaScript Object die wordt gebruikt als een kaart. Elk van de &quot;toetsen&quot; vertegenwoordigt een andere `nodeType` . Parameters van `node` en `children` kunnen worden doorgegeven aan de resulterende functies die het knooppunt renderen. Het retourneringstype dat in dit voorbeeld wordt gebruikt, is JSX, maar de benadering kan worden aangepast om een letterlijke tekenreeks samen te stellen die HTML-inhoud vertegenwoordigt.

### Voorbeeld van volledige code

Een herbruikbaar rijke tekst-teruggevend nut kan in het [&#x200B; WKND GraphQL React voorbeeld &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) worden gevonden.

* [&#x200B; renderRichText.js &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) - herbruikbaar nut dat een functie `mapJsonRichText` blootstelt. Dit nut kan door componenten worden gebruikt die een rijke tekstJSON reactie als React JSX willen teruggeven.
* [&#x200B; AdventureDetail.js &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - de component van het Voorbeeld die een verzoek van GraphQL doet dat rijke tekst omvat. De component gebruikt het hulpprogramma `mapJsonRichText` om de tekst met opmaak en eventuele verwijzingen te renderen.


## In-line verwijzingen toevoegen aan RTF-tekst {#insert-fragment-references}

In het veld Mutliline kunnen auteurs afbeeldingen of andere digitale elementen uit AEM Assets invoegen in de tekststroom met tekstopmaak.

![&#x200B; neem beeld &#x200B;](assets/rich-text/insert-image.png) op

Het bovenstaande schermschot toont een beeld dat op het multi-lijngebied wordt opgenomen gebruikend de **activa van het Tussenvoegsel** knoop.

De verwijzingen naar andere Fragmenten van de Inhoud kunnen ook op het multi-line gebied worden verbonden of worden opgenomen gebruikend de **knoop van het Fragment van de Inhoud van het Tussenvoegsel**.

![&#x200B; de verwijzing van het inhoudsfragment van het Tussenvoegsel &#x200B;](assets/rich-text/insert-contentfragment.png)

Bovenstaande schermafbeelding toont een ander Content Fragment, Ultimate Guide to LA Skate Parks, dat wordt ingevoegd in het veld met meerdere regels. De types van de Fragmenten van de Inhoud die in gebied kunnen worden opgenomen worden gecontroleerd door **Toegestane Modellen van het Fragment van de Inhoud** configuratie in het [&#x200B; multi-line gegevenstype &#x200B;](#multi-line-data-type) in het Model van het Fragment van de Inhoud.

## In-line zoekopdrachten uitvoeren met GraphQL

Met de GraphQL API kunnen ontwikkelaars een query maken die aanvullende eigenschappen bevat over verwijzingen die in een veld met meerdere regels zijn ingevoegd. De JSON-reactie bevat een afzonderlijk `_references` -object waarin deze extra eigenschappen worden vermeld. Het JSON-antwoord geeft ontwikkelaars de volledige controle over hoe ze de referenties of koppelingen moeten weergeven in plaats van dat ze moeten omgaan met gecommuniceerde HTML.

U kunt bijvoorbeeld het volgende doen:

* Omvat douane verpletterende logica voor het beheren van verbindingen aan andere Fragmenten van de Inhoud wanneer het uitvoeren van Één enkele Toepassing van de Pagina, zoals het gebruiken van React Router of Next.js
* Een inline afbeelding renderen met het absolute pad naar een AEM-publicatie-omgeving als de `src` -waarde.
* Bepaal hoe u een ingesloten verwijzing naar een ander inhoudsfragment rendert met extra aangepaste eigenschappen.

Gebruik het retourneringstype `json` en neem het object `_references` op wanneer u een GraphQL-query samenstelt:

**GraphQL bleef vraag:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }  
    }
  }
}
```

In de bovenstaande query wordt het veld `main` geretourneerd als JSON. Het `_references` -object bevat fragmenten voor de afhandeling van alle verwijzingen van het type `ImageRef` of `ArticleModel` .

**JSON reactie:**

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
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

De JSON-reactie omvat de locatie waar de verwijzing is ingevoegd in de RTF-tekst met de `"nodeType": "reference"` . Het `_references` -object bevat vervolgens elke referentie.

## Inline-verwijzingen renderen in RTF-tekst

Om in-line verwijzingen terug te geven, kan de recursieve benadering die in [&#x200B; wordt verklaard die een multi-lijn reactie JSON &#x200B;](#render-multiline-json-richtext) teruggeven worden uitgebreid.

Waar `nodeMap` de kaart is die de JSON-knooppunten rendert.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

De aanpak op hoog niveau is om te inspecteren wanneer een `nodeType` gelijk is aan `reference` in de Mutli Line JSON-respons. Vervolgens kan een aangepaste renderfunctie worden aangeroepen die het object `_references` bevat dat in het GraphQL-antwoord wordt geretourneerd.

Het inline verwijzingspad kan vervolgens worden vergeleken met de corresponderende vermelding in het `_references` -object en een andere aangepaste kaart `renderReference` kan worden aangeroepen.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

Met `__typename` van het `_references` -object kunt u verschillende referentietypen toewijzen aan verschillende renderfuncties.

### Voorbeeld van volledige code

Een volledig voorbeeld van het schrijven van een renderer van douaneverwijzingen kan in [&#x200B; AdventureDetail.js &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) als deel van [&#x200B; GraphQL React voorbeeld van WKND &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app) worden gevonden.

## Voorbeeld van begin tot einde

>[!VIDEO](https://video.tv.adobe.com/v/3449706?quality=12&learn=on&captions=dut)

>[!NOTE]
>
> De bovenstaande video gebruikt `_publishUrl` om de afbeeldingsverwijzing te renderen. In plaats daarvan, verkies `_dynamicUrl` zoals die in [&#x200B; wordt verklaard web-geoptimaliseerde beelden hoe te &#x200B;](./images.md);


De voorgaande video toont een voorbeeld van begin tot eind:

1. Het tekstveld met meerdere regels van een inhoudsfragmentmodel bijwerken om fragmentverwijzingen toe te staan
2. Met de Inhoudsfragmenteditor kunt u een afbeelding en een verwijzing naar een ander fragment opnemen in een tekstveld met meerdere regels.
3. Een GraphQL-query maken die de multiline tekstreactie bevat als JSON en alle `_references` die wordt gebruikt.
4. Schrijvend React SPA die de in-line verwijzingen van de rijke tekstreactie teruggeeft.
