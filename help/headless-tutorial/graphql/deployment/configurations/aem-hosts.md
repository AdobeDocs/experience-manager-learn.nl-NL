---
title: AEM hosts voor AEM GraphQL beheren
description: Leer hoe u AEM hosts configureert in AEM headless-app.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 496
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 0%

---

# AEM hosts beheren

Bij het implementeren van een AEM toepassing zonder kop moet u nagaan hoe AEM URL&#39;s zijn samengesteld om ervoor te zorgen dat de juiste AEM host/domein wordt gebruikt. De primaire URL/aanvraagtypen zijn:

+ De verzoeken van HTTP aan __[AEM GraphQL APIs](#aem-graphql-api-requests)__
+ __[Beeld URLs](#aem-image-urls)__ aan beeldactiva die in de Fragmenten van de Inhoud van verwijzingen worden voorzien, en door AEM geleverd

Een AEM Headless-app communiceert doorgaans met één AEM voor zowel GraphQL API- als afbeeldingsaanvragen. De AEM-service verandert op basis van de implementatie van de AEM Headless-app:

| AEM implementatietype voor headless | AEM | AEM |
|-------------------------------|:---------------------:|:----------------:|
| Productie | Productie | Publish |
| Voorvertoning ontwerpen | Productie | Voorvertoning |
| Ontwikkeling | Ontwikkeling | Publish |

Om plaatsingstype permutaties te behandelen, wordt elke app plaatsing gebouwd gebruikend een configuratie die de AEM te verbinden dienst specificeert. De host/het domein van de geconfigureerde AEM service wordt vervolgens gebruikt om de AEM GraphQL API-URL&#39;s en afbeeldings-URL&#39;s samen te stellen. Als u de juiste benadering voor het beheer van build-afhankelijke configuraties wilt bepalen, raadpleegt u de documentatie van de AEM Headless-app (bijvoorbeeld React, iOS, Android™, enzovoort), aangezien de aanpak per framework verschilt.

| Type client | [ Enige-pagina app (SPA) ](../spa.md) | [ Component/JS van het Web ](../web-component.md) | [ Mobiel ](../mobile.md) | [ server-aan-server ](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuratie AEM hosts | ✔ | ✔ | ✔ | ✔ |

Het volgende is voorbeelden van mogelijke benaderingen om URLs voor [ te construeren AEM GraphQL API ](#aem-graphql-api-requests) en [ beeldverzoeken ](#aem-image-requests), voor verscheidene populaire koploze kaders en platforms.

## GraphQL API-aanvragen AEM

De verzoeken van de GET van HTTP van headless app aan AEM GraphQL APIs moeten worden gevormd om met de correcte AEM dienst in wisselwerking te staan, zoals die in de [ lijst hierboven ](#managing-aem-hosts) wordt beschreven.

Wanneer het gebruiken van [ AEM Koploze SDKs ](../../how-to/aem-headless-sdk.md) (beschikbaar voor op browser-gebaseerde JavaScript, op server-gebaseerde JavaScript, en Java™), kan een AEM gastheer het AEM Hoofdloze cliëntvoorwerp met de AEMDienst initialiseren om met te verbinden.

Wanneer het ontwikkelen van een douane AEM de Draadloze cliënt, zorg ervoor de gastheer van de AEM dienst parameterize-able op bouwstijlparameters wordt gebaseerd.

### Voorbeelden

Hieronder volgen voorbeelden van hoe AEM GraphQL API-aanvragen de AEM hostwaarde kunnen hebben die configureerbaar is voor verschillende raamwerken voor toepassingen zonder kop.

+++ Voorbeeld Reageren

Dit voorbeeld, los gebaseerd op [ AEM Headless React app ](../../example-apps/react-app.md), illustreert hoe AEM de verzoeken van GraphQL API kunnen worden gevormd om met verschillende AEM Diensten te verbinden die op milieuvariabelen worden gebaseerd.

Reageer apps zou de [ Zwaarloze Cliënt van de Zwaartepunt voor JavaScript ](../../how-to/aem-headless-sdk.md) moeten gebruiken om met AEM te communiceren GraphQL APIs. De AEM Headless-client, die wordt geleverd door de AEM Headless Client voor JavaScript, moet worden geïnitialiseerd met de AEM Service-host waarmee deze verbinding maakt.

#### Omgevingsbestand Reageren

Reageer gebruik [ dossiers van het douanemilieu ](https://create-react-app.dev/docs/adding-custom-environment-variables/), of `.env` dossiers, die in de wortel van het project worden opgeslagen om bouwstijl-specifieke waarden te bepalen. Het bestand `.env.development` bevat bijvoorbeeld waarden die worden gebruikt tijdens de ontwikkeling, terwijl `.env.production` waarden bevat die worden gebruikt voor productiebuilds.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` dossiers voor ander gebruik [ kunnen worden gespecificeerd ](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) door `.env` en een semantische beschrijver, zoals `.env.stage` of `.env.production` postfixeren. Verschillende `.env` -bestanden kunnen worden gebruikt wanneer de React-app wordt uitgevoerd of gemaakt, door de `REACT_APP_ENV` in te stellen voordat een `npm` -opdracht wordt uitgevoerd.

Een React-app `package.json` kan bijvoorbeeld de volgende `scripts` config bevatten:

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

De [ AEM Zwaarloze Cliënt voor JavaScript ](../../how-to/aem-headless-sdk.md) bevat een AEM Zwaardeloze cliënt die HTTP- verzoeken aan AEM van GraphQL APIs doet. De AEM Headless-client moet worden geïnitialiseerd met de AEM host waarmee deze communiceert, met behulp van de waarde van het actieve `.env` -bestand.

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

#### Reageercomponent

De aangepaste useEffect-haak `useAdventureByPath` wordt geïmporteerd en gebruikt om de gegevens op te halen met de AEM Headless-client en de inhoud uiteindelijk te renderen naar de eindgebruiker.

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

Dit voorbeeld, dat op het [ voorbeeld wordt gebaseerd AEM Headless iOS™ app ](../../example-apps/ios-swiftui-app.md), illustreert hoe AEM de verzoeken van GraphQL API kunnen worden gevormd om met verschillende AEM te verbinden die gastheren op [ worden gebaseerd bouwt-specifieke configuratievariabelen ](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Voor iOS™-toepassingen is een aangepaste AEM Headless-client vereist voor interactie met AEM GraphQL API&#39;s. De cliënt van de Zwaartepunt van de AEM moet worden geschreven zodat de AEM de dienstgastheer configureerbaar is.

#### Configuratie samenstellen

Het XCode-configuratiebestand bevat de standaardconfiguratiegegevens.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### De aangepaste AEM zonder kop initialiseren

Het [ voorbeeld AEM Headless iOS app ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) gebruikt een douane AEM zonder kop cliënt die met de configuratiewaarden voor `AEM_SCHEME` en `AEM_HOST` wordt geïnitialiseerd.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

De aangepaste AEM headless-client (`api/Aem.swift`) bevat een methode `makeRequest(..)` die AEM GraphQL APIs-aanvragen voorstelt met de geconfigureerde AEM `scheme` en `host` .

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

[ de Nieuwe dossiers van de bouwstijlconfiguratie kunnen ](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) worden gecreeerd om met de verschillende AEM diensten te verbinden. De build-specifieke waarden voor de `AEM_SCHEME` en `AEM_HOST` worden gebruikt op basis van de geselecteerde build in XCode, wat resulteert in de aangepaste AEM Headless-client voor verbinding met de juiste AEM.

+++

+++ Android™-voorbeeld

Dit voorbeeld, dat op het [ voorbeeld wordt gebaseerd AEM Headless Android™ app ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustreert hoe AEM de verzoeken van GraphQL API kunnen worden gevormd om met verschillende AEMDiensten te verbinden die op bouwstijlspecifieke (of vlotters) configuratievariabelen worden gebaseerd.

Android™ apps (wanneer geschreven in Java™) zou de [ Zwaarloze Cliënt van de Zwaartepunt voor Java™ ](https://github.com/adobe/aem-headless-client-java) moeten gebruiken om met AEM GraphQL APIs in wisselwerking te staan. De AEM Headless-client, die wordt geleverd door de AEM Headless Client voor Java™, moet worden geïnitialiseerd met de AEM Service-host waarmee deze verbinding maakt.

#### Configuratiebestand samenstellen

Android™-apps definiëren &quot;productFlavors&quot; die worden gebruikt om artefacten te maken voor verschillende toepassingen.
Dit voorbeeld toont hoe twee Android™ productaroma&#39;s kunnen worden bepaald, die verschillende AEM dienstgastheren (`AEM_HOST`) waarden voor ontwikkeling (`dev`) verstrekken en productie (`prod`) gebruik.

In het `build.gradle` -bestand van de app wordt een nieuwe `flavorDimension` genaamd `env` gemaakt.

In de `env` -dimensie zijn twee `productFlavors` gedefinieerd: `dev` en `prod` . Elke `productFlavor` gebruikt `buildConfigField` om bouwstijlspecifieke variabelen te plaatsen die de AEM dienst bepalen om met te verbinden.

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

#### Android™ loader

Initialiseer de `AEMHeadlessClient` builder, die wordt geleverd door de AEM Headless Client voor Java™, met de `AEM_HOST` -waarde van het veld `buildConfigField` .

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

Wanneer u de Android™-app voor verschillende toepassingen maakt, geeft u de `env` -smaak op en wordt de bijbehorende AEM hostwaarde gebruikt.

+++

## URL&#39;s AEM

De beeldverzoeken van headless app aan AEM moeten worden gevormd om met de correcte AEM dienst in wisselwerking te staan, zoals die in [ hierboven lijst ](#managing-aem-hosts) wordt beschreven.

De Adobe adviseert het gebruiken van [ geoptimaliseerde beelden ](../../how-to/images.md) beschikbaar gemaakt door het `_dynamicUrl` gebied in het AEM van GraphQL APIs. Het veld `_dynamicUrl` retourneert een URL zonder host die kan worden voorafgegaan door de host van de AEM die wordt gebruikt voor query AEM GraphQL API&#39;s. Het veld `_dynamicUrl` in het GraphQL-antwoord ziet er als volgt uit:

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Voorbeelden

Hieronder ziet u voorbeelden van hoe afbeeldings-URL&#39;s de waarde van de AEM host kunnen voorvoegsel die voor verschillende raamwerken zonder kop kan worden geconfigureerd. In de voorbeelden wordt ervan uitgegaan dat GraphQL-query&#39;s worden gebruikt die afbeeldingsreferenties retourneren met het veld `_dynamicUrl` .

Bijvoorbeeld:

#### GraphQL-query voortgezet

Deze GraphQL-query retourneert de `_dynamicUrl` van een afbeeldingsverwijzing. Zoals gezien in de [ reactie van GraphQL ](#examples-react-graphql-response) die een gastheer uitsluit.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### GraphQL-reactie

Deze GraphQL-reactie retourneert de verwijzing naar de afbeelding `_dynamicUrl` , die een host uitsluit.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ Voorbeeld Reageren

Dit voorbeeld, dat op het [ voorbeeld wordt gebaseerd AEM Headless React app ](../../example-apps/react-app.md), illustreert hoe beeld URLs kan worden gevormd om met de correcte AEMDiensten te verbinden die op milieuvariabelen worden gebaseerd.

In dit voorbeeld wordt getoond hoe voorvoegsel voor het afbeeldingsverwijzingsveld `_dynamicUrl` wordt weergegeven met een configureerbare `REACT_APP_AEM_HOST` Omgevingsvariabele React.

#### Omgevingsbestand Reageren

Reageer gebruik [ dossiers van het douanemilieu ](https://create-react-app.dev/docs/adding-custom-environment-variables/), of `.env` dossiers, die in de wortel van het project worden opgeslagen om bouwstijl-specifieke waarden te bepalen. Het bestand `.env.development` bevat bijvoorbeeld waarden die worden gebruikt tijdens de ontwikkeling, terwijl `.env.production` waarden bevat die worden gebruikt voor productiebuilds.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` dossiers voor ander gebruik [ kunnen worden gespecificeerd ](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) door `.env` en een semantische beschrijver, zoals `.env.stage` of `.env.production` postfixeren. Er kan een ander `.env` -bestand worden gebruikt wanneer de React-app wordt uitgevoerd of gemaakt, door de `REACT_APP_ENV` in te stellen voordat een `npm` -opdracht wordt uitgevoerd.

Een React-app `package.json` kan bijvoorbeeld de volgende `scripts` config bevatten:

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

#### Reageercomponent

De component React importeert de omgevingsvariabele `REACT_APP_AEM_HOST` en corrigeert de afbeeldings `_dynamicUrl` -waarde om een volledig oplosbare afbeeldings-URL te verkrijgen.

Deze zelfde `REACT_APP_AEM_HOST` omgevingsvariabele wordt gebruikt om de cliënt te initialiseren AEM Headless die door `useAdventureByPath(..)` wordt gebruikt de haken van het douaneuseEffect die wordt gebruikt om de gegevens van GraphQL van AEM te halen. Wanneer u dezelfde variabele gebruikt om de GraphQL API-aanvraag samen te stellen als de afbeeldings-URL, moet u ervoor zorgen dat de React-app voor beide gebruiksgevallen interageert met dezelfde AEM.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™-voorbeeld

Dit voorbeeld, dat op het [ voorbeeld wordt gebaseerd AEM Headless iOS™ app ](../../example-apps/ios-swiftui-app.md), illustreert hoe AEM beeld URLs kan worden gevormd om met verschillende AEM gastheren te verbinden die op [ worden gebaseerd bouwt-specifieke configuratievariabelen ](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Configuratie samenstellen

Het XCode-configuratiebestand bevat de standaardconfiguratiegegevens.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### URL-generator voor afbeelding

In `Aem.swift` gebruikt een aangepaste AEM headless-clientimplementatie het afbeeldingspad zoals opgegeven in het veld `_dynamicUrl` in de GraphQL-reactie en voegt deze aan de AEM toe. `imageUrl(..)` Deze functie wordt vervolgens aangeroepen in de iOS-weergaven wanneer een afbeelding wordt gerenderd.

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
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### iOS-weergave

In de iOS-weergave en de voorvoegsel van de afbeeldings `_dynamicUrl` -waarde wordt een volledig oplosbare afbeeldings-URL weergegeven.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[ de Nieuwe dossiers van de bouwstijlconfiguratie kunnen ](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) worden gecreeerd om met de verschillende AEM diensten te verbinden. De build-specifieke waarden voor de `AEM_SCHEME` en `AEM_HOST` worden gebruikt op basis van de geselecteerde build in XCode, wat resulteert in de aangepaste AEM Headless-client voor interactie met de juiste AEM.

+++

+++ Android™-voorbeeld

Dit voorbeeld, dat op het [ voorbeeld wordt gebaseerd AEM Headless Android™ app ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), illustreert hoe AEM beeld URLs kan worden gevormd om met verschillende AEMDiensten te verbinden die op bouwstijlspecifieke (of vlotters) configuratievariabelen worden gebaseerd.

#### Configuratiebestand samenstellen

Android™-apps definiëren &quot;productFlavors&quot; die worden gebruikt om artefacten voor verschillende toepassingen te maken.
Dit voorbeeld toont hoe twee Android™ productaroma&#39;s kunnen worden bepaald, die verschillende AEM dienstgastheren (`AEM_HOST`) waarden voor ontwikkeling (`dev`) verstrekken en productie (`prod`) gebruik.

In het `build.gradle` -bestand van de app wordt een nieuwe `flavorDimension` genaamd `env` gemaakt.

In de `env` -dimensie zijn twee `productFlavors` gedefinieerd: `dev` en `prod` . Elke `productFlavor` gebruikt `buildConfigField` om bouwstijlspecifieke variabelen te plaatsen die de AEM dienst bepalen om met te verbinden.

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

De Android™ gebruikt een `ImageGetter` om afbeeldingsgegevens van AEM op te halen en lokaal in de cache op te slaan. In `prepareDrawableFor(..)` wordt de AEM servicehost, die in de actieve build-config is gedefinieerd, gebruikt om een voorvoegsel van het afbeeldingspad te maken dat een oplosbare URL voor AEM maakt.

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
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Android™-weergave

In de Android™-weergave worden de afbeeldingsgegevens opgehaald via de `RemoteImagesCache` met behulp van de `_dynamicUrl` -waarde uit het GraphQL-antwoord.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

Wanneer u de Android™-app voor verschillende toepassingen maakt, geeft u de `env` -smaak op en wordt de bijbehorende AEM hostwaarde gebruikt.

+++
