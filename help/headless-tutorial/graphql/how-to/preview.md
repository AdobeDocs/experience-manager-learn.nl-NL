---
title: Voorvertoning van inhoudsfragment
description: Leer hoe u de voorvertoning van een inhoudsfragment aan alle auteurs kunt gebruiken om snel te zien hoe wijzigingen in de inhoud van invloed zijn op uw AEM Headless-ervaringen.
version: Experience Manager as a Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
exl-id: 247d40a3-ff67-4c1f-86bf-3794d7ce3e32
duration: 463
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---

# Voorvertoning van inhoudsfragment

AEM Headless-toepassingen ondersteunen geïntegreerde ontwerpvoorvertoning. De voorvertoning koppelt de Inhoudsfragmenteditor van de AEM-auteur aan uw aangepaste app (adresseerbaar via HTTP), zodat een uitgebreide koppeling mogelijk is naar de app die de voorvertoning van het Inhoudsfragment weergeeft.

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

Aan verschillende voorwaarden moet zijn voldaan om een voorbeeld van een inhoudsfragment te kunnen gebruiken:

1. De app moet worden geïmplementeerd op een URL die toegankelijk is voor auteurs
1. De app moet zijn geconfigureerd om verbinding te maken met de AEM Author-service (in plaats van de AEM Publish-service)
1. App moet met URLs of routes worden ontworpen die [ weg of identiteitskaart van het Fragment van de Inhoud ](#url-expressions) kunnen gebruiken om de Fragmenten van de Inhoud te selecteren om voor voorproef in de app ervaring te tonen.

## Voorvertoning van URL&#39;s

Voorproef URLs, gebruikend [ uitdrukkingen URL ](#url-expressions), wordt geplaatst op de Eigenschappen van het Model van het Fragment van de Inhoud.

![ het ModelVoorproef URL van het Fragmentmodel van de Inhoud ](./assets/preview/cf-model-preview-url.png)

1. Aanmelden bij de AEM Author-service als beheerder
1. Navigeer aan __Hulpmiddelen > Algemeen > de Modellen van het Fragment van de Inhoud__
1. Selecteer het __Model van het Fragment van de Inhoud__ en selecteer __Eigenschappen__ van de hoogste actiebar.
1. Ga voorproef URL voor het Model van het Fragment van de Inhoud in gebruikend [ uitdrukkingen URL ](#url-expressions)
   + De URL van de voorvertoning moet verwijzen naar een implementatie van de app die verbinding maakt met de AEM Author-service.

### URL-expressies

Elk inhoudsfragmentmodel kan een voorbeeld-URL hebben. De URL van de voorvertoning kan per inhoudsfragment worden geparametriseerd met behulp van de URL-expressies die in de onderstaande tabel worden vermeld. Meerdere URL-expressies kunnen worden gebruikt in één voorbeeld-URL.

|                                         | URL-uitdrukking | Waarde |
| --------------------------------------- | ----------------------------------- | ----------- |
| Pad inhoudfragment | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| Inhoudsfragment-id | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Variatie van inhoudsfragment | `${contentFragment.variation}` | `main` |
| Pad inhoudsfragmentmodel | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Naam van inhoudsfragmentmodel | `${contentFragment.model.name}` | `adventure` |

Voorbeeld-URL&#39;s:

+ Een voorproef URL op het __model van het Adventure__ kon als `https://preview.app.wknd.site/adventure${contentFragment.path}` kijken die aan `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` oplost
+ Een voorproef URL op het __model van het Artikel__ kon als `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` kijken oplossen `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## Voorvertoning in app

Voor elk inhoudsfragment dat gebruikmaakt van het geconfigureerde inhoudsfragmentmodel is een knop Voorvertoning beschikbaar. De knoop van de Voorproef opent de voorproef URL van het Model van het Fragment van de Inhoud en injecteert de open waarden van het Fragment van de Inhoud in de [ uitdrukkingen URL ](#url-expressions).

![ knoop van de Voorproef ](./assets/preview/preview-button.png)

Voer hard vernieuwen uit (wissen van de lokale cache van de browser) wanneer u een voorvertoning weergeeft van wijzigingen in inhoudsfragmenten in de app.

## Voorbeeld Reageren

Laten we eens kijken naar de WKND-app, een eenvoudige React-toepassing die avonturen van AEM weergeeft met AEM Headless GraphQL API&#39;s.

De voorbeeldcode is beschikbaar op [ Github.com ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL&#39;s en routes

URLs of de routes die aan voorproef worden gebruikt moeten een tevreden Fragment composable zijn gebruikend [ uitdrukkingen URL ](#url-expressions). In deze voorvertoningsversie van de WKND-app worden de fragmenten voor avontuurinhoud weergegeven via de `AdventureDetail` -component die aan de route `/adventure<CONTENT FRAGMENT PATH>` is gebonden. De URL van de voorvertoning van het WKND-avontuurmodel moet daarom zijn ingesteld op `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` om deze route te kunnen gebruiken.

De voorproef van het Fragment van de inhoud werkt slechts als app een adresseerbare route heeft, die met [ uitdrukkingen URL ](#url-expressions) kan worden bevolkt die dat Fragment van de Inhoud in app op een voorproefable manier teruggeven.

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### De geschreven inhoud weergeven

De `AdventureDetail` component ontleedt eenvoudig de weg van het Fragment van de Inhoud, die in Voorproef URL via de `${contentFragment.path}` [ wordt ingespoten uitdrukking URL ](#url-expressions), van de route URL, en gebruikt het om het Adventure WKND te verzamelen en terug te geven.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
