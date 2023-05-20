---
title: Voorvertoning van inhoudsfragment
description: Leer hoe u de voorvertoning van een inhoudsfragment aan alle auteurs kunt gebruiken om snel te zien hoe de wijzigingen in de inhoud van invloed zijn op de ervaringen AEM Koploos.
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
exl-id: 247d40a3-ff67-4c1f-86bf-3794d7ce3e32
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Voorvertoning van inhoudsfragment

AEM toepassingen zonder koppen ondersteunen geïntegreerde ontwerpvoorvertoning. De ervaring in de voorvertoning koppelt de Inhoudsfragmenteditor van de AEM-auteur aan uw aangepaste app (adresseerbaar via HTTP), zodat er een uitgebreide koppeling kan plaatsvinden in de app die de voorvertoning van het Inhoudsfragment weergeeft.

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

Aan verschillende voorwaarden moet zijn voldaan om een voorbeeld van een inhoudsfragment te kunnen gebruiken:

1. De app moet worden geïmplementeerd op een URL die toegankelijk is voor auteurs
1. De app moet zo zijn geconfigureerd dat deze verbinding maakt met de AEM-auteurservice (in plaats van met de AEM-publicatieservice)
1. De app moet zijn ontworpen met URL&#39;s of routes die kunnen worden gebruikt [Pad of id van inhoudsfragment](#url-expressions) om de Content Fragments te selecteren om voor voorproef in app ervaring te tonen.

## Voorvertoning van URL&#39;s

URL&#39;s voorvertonen met [URL-expressies](#url-expressions), worden ingesteld in de eigenschappen van het inhoudsfragmentmodel.

![Voorvertoning-URL van inhoudsfragmentmodel](./assets/preview/cf-model-preview-url.png)

1. Aanmelden bij de AEM Author-service als beheerder
1. Navigeren naar __Gereedschappen > Algemeen > Modellen van inhoudsfragmenten__
1. Selecteer __Inhoudsfragmentmodel__ en selecteert u __Eigenschappen__ van de bovenste actiebalk.
1. Geef de URL van de voorvertoning voor het inhoudsfragmentmodel op met [URL-expressies](#url-expressions)
   + De URL van de voorvertoning moet verwijzen naar een implementatie van de app die verbinding maakt met de AEM-auteurservice.

### URL-expressies

Elk inhoudsfragmentmodel kan een voorbeeld-URL hebben. De URL van de voorvertoning kan per inhoudsfragment worden geparametriseerd met behulp van de URL-expressies die in de onderstaande tabel worden vermeld. Meerdere URL-expressies kunnen worden gebruikt in één voorbeeld-URL.

|  | URL-uitdrukking | Waarde |
| --------------------------------------- | ----------------------------------- | ----------- |
| Pad inhoudfragment | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| Inhoudsfragment-id | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Variatie van inhoudsfragment | `${contentFragment.variation}` | `main` |
| Pad inhoudsfragmentmodel | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Naam van inhoudsfragmentmodel | `${contentFragment.model.name}` | `adventure` |

Voorbeeld-URL&#39;s:

+ Een voorbeeld-URL op het tabblad __Adventure__ model kan er zo uitzien `https://preview.app.wknd.site/adventure${contentFragment.path}` die wordt omgezet in `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ Een voorbeeld-URL op het tabblad __Artikel__ model kan er zo uitzien `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` de oplossingen `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## Voorvertoning in app

Voor elk inhoudsfragment dat gebruikmaakt van het geconfigureerde inhoudsfragmentmodel is een knop Voorvertoning beschikbaar. Met de knop Voorvertoning opent u de URL van de voorvertoning van het inhoudsfragmentmodel en injecteert u de waarden van het geopende inhoudsfragment in de [URL-expressies](#url-expressions).

![Knop Voorvertoning](./assets/preview/preview-button.png)

Voer hard vernieuwen uit (wissen van de lokale cache van de browser) wanneer u een voorvertoning weergeeft van wijzigingen in inhoudsfragmenten in de app.

## Voorbeeld Reageren

Laten we eens kijken naar de WKND-app, een eenvoudige React-toepassing die avonturen van AEM weergeeft met behulp van AEM Headless GraphQL API&#39;s.

De voorbeeldcode is beschikbaar op [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL&#39;s en routes

De URL&#39;s of routes die worden gebruikt om een voorbeeld van een inhoudsfragment weer te geven, moeten kunnen worden samengesteld met [URL-expressies](#url-expressions). In deze voorvertoningsversie van de WKND-app worden de fragmenten voor avontuur-inhoud weergegeven via de `AdventureDetail` component gebonden aan de route `/adventure<CONTENT FRAGMENT PATH>`. De URL van de voorvertoning van het WKND-avontuurmodel moet daarom worden ingesteld op `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` om aan deze route op te lossen.

Voorvertoning van contentfragment werkt alleen als de app een adresseerbare route heeft, die kan worden gevuld [URL-expressies](#url-expressions) die dat inhoudsfragment in de app op een voor een voorvertoning geschikte manier renderen.

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

De `AdventureDetail` parseert eenvoudig het pad van het inhoudsfragment en injecteert u dit via de URL van de voorvertoning `${contentFragment.path}` [URL-expressie](#url-expressions), van de route URL, en gebruikt het om het Adventure WKND te verzamelen en terug te geven.

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
