---
title: Het beheren van AEM gastheren voor AEM GraphQL
description: Leer hoe u AEM hosts configureert in AEM headless-app.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# AEM hosts beheren

Bij het implementeren van een AEM toepassing zonder kop moet u nagaan hoe AEM URL&#39;s zijn samengesteld om ervoor te zorgen dat de juiste AEM host/domein wordt gebruikt. De primaire URL/aanvraagtypen zijn:

+ HTTP-aanvragen voor __[GraphQL API&#39;s AEM](#aem-graphql-api-requests)__
+ __[Afbeeldings-URL&#39;s](#aem-image-urls)__ naar afbeeldingselementen waarnaar wordt verwezen in Inhoudsfragmenten en die worden geleverd door AEM

Doorgaans communiceert een AEM Headless-app met één AEM voor zowel GraphQL API- als afbeeldingsaanvragen. De AEM-service verandert op basis van de implementatie van de AEM Headless-app:

| AEM implementatietype voor headless | AEM | AEM |
|-------------------------------|:---------------------:|:----------------:|
| Productie | Productie | Publicatie |
| Voorvertoning ontwerpen | Productie | Voorvertoning |
| Ontwikkeling | Ontwikkeling | Publicatie |

Om plaatsingstype permutaties te behandelen, wordt elke app plaatsing gebouwd gebruikend een configuratie die de AEM te verbinden dienst specificeert. De gevormde gastheer/het domein van AEM dienst wordt dan gebruikt om AEM GraphQL API URLs en Beeld URLs te construeren. Als u de juiste benadering voor het beheer van build-afhankelijke configuraties wilt bepalen, raadpleegt u de documentatie van de AEM Headless-app (bijvoorbeeld React, iOS, Android™, enzovoort), aangezien de aanpak per framework verschilt.

| Type client | [App van één pagina (SPA)](../spa.md) | [Webcomponent/JS](../web-component.md) | [Mobiel](../mobile.md) | [Server-naar-server](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuratie AEM hosts | ✔ | ✔ | ✔ | ✔ |

Hieronder volgen voorbeelden van mogelijke benaderingen voor het samenstellen van URL&#39;s voor [AEM GraphQL API](#aem-graphql-api-requests) en [afbeeldingsverzoeken](#aem-image-requests), voor verschillende populaire headless frameworks en platforms.

## AEM GraphQL API-aanvragen

De HTTP-GET-aanvragen van de headless-app voor AEM GraphQL-API&#39;s moeten zijn geconfigureerd voor interactie met de juiste AEM, zoals wordt beschreven in het dialoogvenster [tabel boven](#managing-aem-hosts).

Wanneer u [AEM Headless SDK&#39;s](../../how-to/aem-headless-sdk.md) (beschikbaar voor JavaScript op basis van een browser, JavaScript op basis van een server en Java™) kan een AEM host het AEM Headless-clientobject initialiseren met de AEM Service om verbinding te maken.

Wanneer het ontwikkelen van een douane AEM de Draadloze cliënt, zorg ervoor de gastheer van de AEM dienst parameterize-able op bouwstijlparameters wordt gebaseerd.

### Voorbeelden

Hieronder volgen voorbeelden van hoe AEM GraphQL API-aanvragen de AEM hostwaarde kunnen hebben die configureerbaar is voor verschillende raamwerken voor toepassingen zonder kop.

+++ Voorbeeld Reageren

In dit voorbeeld wordt losjes gebaseerd op de [AEM toepassing zonder hoofd Reageren](../../example-apps/react-app.md), illustreert hoe AEM GraphQL API-verzoeken kunnen worden geconfigureerd om verbinding te maken met verschillende AEM Services op basis van omgevingsvariabelen.

Reactie-apps moeten de [AEM headless-client voor JavaScript](../../how-to/aem-headless-sdk.md) om te communiceren met AEM GraphQL-API&#39;s. De AEM Headless-client, die wordt geleverd door de AEM Headless-client voor JavaScript, moet worden geïnitialiseerd met de AEM Service-host waarmee deze verbinding maakt.

#### Omgevingsbestand reageren

Reageergebruik [aangepaste omgevingsbestanden](https://create-react-app.dev/docs/adding-custom-environment-variables/), of `.env` bestanden, opgeslagen in de hoofdmap van het project om ontwikkelspecifieke waarden te definiëren. De `.env.development` het bestand bevat waarden die worden gebruikt tijdens de ontwikkeling, terwijl `.env.production` bevat waarden die worden gebruikt voor productiebuilds.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` bestanden voor ander gebruik [kan worden opgegeven](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) bij postfixatie `.env` en een semantische descriptor, zoals `.env.stage` of `.env.production`. Verschil `.env` bestanden kunnen worden gebruikt bij het uitvoeren of samenstellen van de React-app door de instelling van de `REACT_APP_ENV` voordat u een `npm` gebruiken.

Bijvoorbeeld, React app `package.json` kan het volgende bevatten `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### AEM headless-client

De [AEM headless-client voor JavaScript](../../how-to/aem-headless-sdk.md) bevat een AEM Headless-client die HTTP-aanvragen indient om GraphQL API&#39;s te AEM. De cliënt van de Zwaartepunt van de AEM moet met de AEM worden geïnitialiseerd het met interactie heeft, gebruikend de waarde van actief `.env` bestand.

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### UseEffect(..) Reageren haak

De haken van het gebruiksEffect van de Reactie van de douane roepen de Zwaardeloze cliënt van de AEM, die met de AEM gastheer, namens de component van de Reactie wordt geïnitialiseerd die de mening teruggeeft.

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### Reageren, component

De aangepaste useEffect-haak, `useAdventureByPath` wordt geïmporteerd en gebruikt om de gegevens op te halen met de AEM Headless-client en de inhoud uiteindelijk weer te geven aan de eindgebruiker.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™-voorbeeld

Dit voorbeeld, gebaseerd op [Voorbeeld AEM iOS™-app zonder headless](../../example-apps/ios-swiftui-app.md), illustreert hoe AEM GraphQL API-aanvragen kunnen worden geconfigureerd om verbinding te maken met verschillende AEM hosts op basis van [bouwstijlspecifieke configuratievariabelen](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

iOS™-toepassingen vereisen een aangepaste AEM Headless-client voor interactie met AEM GraphQL-API&#39;s. De cliënt van de AEM Zwaartepunt moet worden geschreven zodat de AEM de dienstgastheer configureerbaar is.

#### Configuratie samenstellen

Het XCode-configuratiebestand bevat de standaardconfiguratiedetails.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### De aangepaste AEM zonder kop initialiseren

De [Voorbeeld AEM iOS-app zonder titel](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) gebruikt een douane AEM zonder hoofd cliënt die met config waarden voor wordt geïnitialiseerd `AEM_SCHEME` en `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

De aangepaste AEM headless-client (`api/Aem.swift`) bevat een methode `makeRequest(..)` dat prefixeert AEM verzoeken GraphQL APIs met gevormde AEM `scheme` en `host`.

+ `api/Aem.swift`

```swift
/// #makeRequest(..)
/// Generic method for constructing and executing AEM GraphQL persisted queries
private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
    // Encode optional parameters as required by AEM
    let persistedQueryParams = params.map { (param) -> String in
        encode(string: ";\(param.key)=\(param.value)")
    }.joined(separator: "")
    
    // Construct the AEM GraphQL persisted query URL, including optional query params
    let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

    var request = URLRequest(url: URL(string: url)!);
    
    return request;
}
```

[De nieuwe bouwstijlconfiguratiedossiers kunnen worden gecreeerd](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) om verbinding te maken met verschillende AEM services. De bouwstijlspecifieke waarden voor `AEM_SCHEME` en `AEM_HOST` worden gebruikt op basis van de geselecteerde build in XCode, wat resulteert in de aangepaste AEM Headless-client voor verbinding met de juiste AEM-service.

+++

+++ Android™-voorbeeld

Dit voorbeeld, gebaseerd op [voorbeeld AEM Android™-app zonder headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustreert hoe AEM toepassingen GraphQL API kunnen worden gevormd om met verschillende AEMDiensten te verbinden die op bouwstijlspecifieke (of vlotterige) configuratievariabelen worden gebaseerd.

Android™-apps (indien geschreven in Java™) moeten de [AEM headless Client voor Java™](https://github.com/adobe/aem-headless-client-java) om te communiceren met AEM GraphQL-API&#39;s. De AEM Headless-client, die wordt geleverd door de AEM Headless Client voor Java™, moet worden geïnitialiseerd met de AEM Service-host waarmee deze verbinding maakt.

#### Configuratiebestand samenstellen

Android™-apps definiëren &quot;productFlavors&quot; die worden gebruikt om artefacten voor verschillende toepassingen te maken.
In dit voorbeeld wordt getoond hoe twee Android™-productreplicators kunnen worden gedefinieerd, waardoor verschillende AEM servicehosts worden geboden (`AEM_HOST`) waarden voor ontwikkeling (`dev`) en productie (`prod`) gebruikt.

In de app `build.gradle` bestand, een nieuw bestand `flavorDimension` benoemd `env` wordt gemaakt.

In de `env` dimensie, twee `productFlavors` worden gedefinieerd: `dev` en `prod`. Elk `productFlavor` gebruik `buildConfigField` om bouwstijlspecifieke variabelen te plaatsen die de AEM dienst bepalen om met te verbinden.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Android™-loader

Initialiseer de `AEMHeadlessClient` builder, geleverd door de AEM Headless Client voor Java™ met de `AEM_HOST` waarde van de `buildConfigField` veld.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

Geef bij het maken van de Android™-app voor verschillende toepassingen de `env` smaak, en de overeenkomstige AEM gastheerwaarde wordt gebruikt.

+++

## URL&#39;s AEM

De afbeeldingsaanvragen van de headless-app voor AEM moeten zo zijn geconfigureerd dat ze communiceren met de juiste AEM, zoals wordt beschreven in het dialoogvenster [boven tabel](#managing-aem-hosts).

AEM GraphQL&#39;s `... on ImageRef` verschaft velden `_authorUrl` en `_publishUrl` die absolute URLs aan de respectieve AEM diensten bevat, is het vaak het meest rechtuit-door:sturen om te gebruiken `_path` veld en voorvoegsel van de AEM servicehost die wordt gebruikt om query&#39;s uit te voeren op AEM GraphQL API&#39;s.

Gebruiken `_path` Dit kan vooral handig zijn als de headless-app verbinding kan maken met AEM Author of AEM Publish op basis van de implementatiecontext.

Als de app zonder koppen uitsluitend communiceert met AEM Author of Publish, `_authorUrl` of `_publishUrl` velden kunnen worden gebruikt om de implementatie te vereenvoudigen en de richtsnoeren in de onderstaande voorbeelden kunnen worden genegeerd.

### Voorbeelden

Hieronder ziet u voorbeelden van hoe afbeeldings-URL&#39;s de waarde van de AEM host kunnen voorvoegsel die voor verschillende raamwerken zonder kop kan worden geconfigureerd. De voorbeelden veronderstellen het gebruik van vragen GraphQL die beeldverwijzingen terugkeren gebruikend `_path` veld.

Bijvoorbeeld:

#### GraphQL-voortgezette query

Deze GraphQL-query retourneert een verwijzing naar een afbeelding `_path`. Zoals u ziet in het dialoogvenster [GraphQL-respons](#examples-react-graphql-response) waarbij een host wordt uitgesloten.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
        }
      }
    }
  }
}
```

#### GraphQL-respons

Deze GraphQL-reactie retourneert de referentie van de afbeelding `_path` waarbij een host wordt uitgesloten.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
        }
      }
    }
  }
}
```

+++ Voorbeeld Reageren

Dit voorbeeld, gebaseerd op [Voorbeeld AEM de toepassing Headless React](../../example-apps/react-app.md), illustreert hoe beeld URLs kan worden gevormd om met de correcte AEMDiensten te verbinden die op omgevingsvariabelen worden gebaseerd.

In dit voorbeeld wordt getoond hoe voorvoegsel de afbeeldingsverwijzing `_path` veld, met een configureerbaar `REACT_APP_AEM_HOST` Reageer omgevingsvariabele.

#### Omgevingsbestand reageren

Reageergebruik [aangepaste omgevingsbestanden](https://create-react-app.dev/docs/adding-custom-environment-variables/), of `.env` bestanden, opgeslagen in de hoofdmap van het project om ontwikkelspecifieke waarden te definiëren. De `.env.development` het bestand bevat waarden die worden gebruikt tijdens de ontwikkeling, terwijl `.env.production` bevat waarden die worden gebruikt voor productiebuilds.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` bestanden voor ander gebruik [kan worden opgegeven](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) bij postfixatie `.env` en een semantische descriptor, zoals `.env.stage` of `.env.production`. Verschil `.env` kan worden gebruikt bij het uitvoeren of samenstellen van de React-app door het instellen van de `REACT_APP_ENV` voordat u een `npm` gebruiken.

Bijvoorbeeld, React app `package.json` kan het volgende bevatten `scripts` config:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### Reageren, component

De component React importeert de `REACT_APP_AEM_HOST` omgevingsvariabele en voorvoegsel van de afbeelding `_path` waarde, om een volledig oplosbare afbeeldings-URL op te geven.

Hetzelfde `REACT_APP_AEM_HOST` omgevingsvariabele wordt gebruikt om de AEM Headless-client te initialiseren die wordt gebruikt door `useAdventureByPath(..)` de haken van het douane useEffect die wordt gebruikt om de gegevens GraphQL van AEM te halen. Wanneer u dezelfde variabele gebruikt om de GraphQL API-aanvraag samen te stellen als de afbeeldings-URL, moet u ervoor zorgen dat de React-app voor beide gebruiksgevallen interageert met dezelfde AEM-service.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._path }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™-voorbeeld

Dit voorbeeld, gebaseerd op [Voorbeeld AEM iOS™-app zonder headless](../../example-apps/ios-swiftui-app.md), illustreert hoe AEM beeld-URL&#39;s kunnen worden geconfigureerd om verbinding te maken met verschillende AEM hosts op basis van [bouwstijlspecifieke configuratievariabelen](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Configuratie samenstellen

Het XCode-configuratiebestand bevat de standaardconfiguratiedetails.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### URL-generator voor afbeelding

In `Aem.swift`, de aangepaste AEM clientimplementatie zonder kop, een aangepaste functie `imageUrl(..)` neemt het afbeeldingspad zoals opgegeven in het dialoogvenster `_path` in de reactie GraphLQ, en prepends het met AEM gastheer. Deze functie wordt vervolgens aangeroepen in de iOS-weergaven wanneer een afbeelding wordt gerenderd.

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image paths wit the AEM scheme/host
    func imageUrl(path: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(path)")!
    }
    ...
}
```

#### iOS-weergave

De iOS-weergave en de voorvoegsel van de afbeelding `_path` waarde, om een volledig oplosbare afbeeldings-URL op te geven.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image path to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(path: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[De nieuwe bouwstijlconfiguratiedossiers kunnen worden gecreeerd](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) om verbinding te maken met verschillende AEM services. De bouwstijlspecifieke waarden voor `AEM_SCHEME` en `AEM_HOST` worden gebruikt gebaseerd op de geselecteerde build in XCode, resulterend in de aangepaste AEM Headless client voor interactie met de juiste AEM service.

+++

+++ Android™-voorbeeld

Dit voorbeeld, gebaseerd op [voorbeeld AEM Android™-app zonder headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustreert hoe AEM beeld URLs kan worden gevormd om met verschillende AEMDiensten te verbinden die op bouwstijlspecifieke (of vlotterige) configuratievariabelen worden gebaseerd.

#### Configuratiebestand samenstellen

Android™-apps definiëren &quot;productFlavors&quot; die worden gebruikt om artefacten voor verschillende toepassingen te maken.
In dit voorbeeld wordt getoond hoe twee Android™-productreplicators kunnen worden gedefinieerd, waardoor verschillende AEM servicehosts worden geboden (`AEM_HOST`) waarden voor ontwikkeling (`dev`) en productie (`prod`) gebruikt.

In de app `build.gradle` bestand, een nieuw bestand `flavorDimension` benoemd `env` wordt gemaakt.

In de `env` dimensie, twee `productFlavors` worden gedefinieerd: `dev` en `prod`. Elk `productFlavor` gebruik `buildConfigField` om bouwstijlspecifieke variabelen te plaatsen die de AEM dienst bepalen om met te verbinden.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### De AEM-afbeelding laden

Android™ gebruikt een `ImageGetter` om afbeeldingsgegevens van AEM op te halen en lokaal in cache op te slaan. In `prepareDrawableFor(..)` de AEM de dienstgastheer, die in de actieve bouwstijlconfig wordt bepaald, wordt gebruikt om de beeldweg voor te stellen die tot een oplosbare URL aan AEM leidt.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String path) {
        // Get the image data from the cache using the path as the key
        Drawable drawable = drawablesByPath.get(path);
        return drawable;
    }
}
```

#### Android™-weergave

In de Android™-weergave worden de afbeeldingsgegevens opgehaald via de `RemoteImagesCache` met de `_path` waarde uit de GraphQL-respons.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImagePath()));
        ...
    }
...
}
```

Geef bij het maken van de Android™-app voor verschillende toepassingen de `env` smaak, en de overeenkomstige AEM gastheerwaarde wordt gebruikt.

+++