---
title: iOS App - AEM Headless-voorbeeld
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze iOS-toepassing laat zien hoe u query's uitvoert op inhoud met GraphQL API's van AEM die doorlopende query's gebruiken.
version: Experience Manager as a Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# iOS-app

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze iOS-toepassing laat zien hoe u query&#39;s uitvoert op inhoud met GraphQL API&#39;s van AEM die doorlopende query&#39;s gebruiken.

![&#x200B; iOS SwiftUI app met AEM Headless &#x200B;](./assets/ios-swiftui-app/ios-app.png)

Bekijk de [&#x200B; broncode op GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [&#x200B; Xcode &#x200B;](https://developer.apple.com/xcode/) (vereist macOS)
+ [&#x200B; Git &#x200B;](https://git-scm.com/)

## AEM-vereisten

De iOS-toepassing werkt met de volgende AEM-implementatieopties. Alle plaatsingen vereisen de [&#x200B; Plaats van WKND v3.0.0+ &#x200B;](https://github.com/adobe/aem-guides-wknd/releases/latest) om worden geïnstalleerd.

+ [&#x200B; AEM as a Cloud Service &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=nl-NL)
+ De lokale opstelling die [&#x200B; AEM Cloud Service SDK &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL) gebruikt

De toepassing van iOS wordt ontworpen om met __AEM te verbinden publiceert__ milieu, nochtans kan het inhoud van de Auteur van AEM als de authentificatie in de configuratie van de toepassing van iOS wordt verstrekt.

## Hoe wordt het gebruikt

1. De gegevensopslagruimte `adobe/aem-guides-wknd-graphql` klonen:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Open [&#x200B; Xcode &#x200B;](https://developer.apple.com/xcode/) en open de omslag `ios-app`
1. Wijzig het bestand `Config.xcconfig` en werk `AEM_SCHEME` en `AEM_HOST` bij zodat deze overeenkomen met de AEM-doelservice Publiceren.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Als u verbinding maakt met AEM Author, voegt u de `AEM_AUTH_TYPE` - en ondersteunende verificatie-eigenschappen toe aan de `Config.xcconfig` .

   __Basisauthentificatie__

   Met `AEM_USERNAME` en `AEM_PASSWORD` kunt u een lokale AEM-gebruiker verifiëren met toegang tot WKND GraphQL-inhoud.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Symbolische authentificatie__

   `AEM_TOKEN` is een [&#x200B; toegangstoken &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=nl-NL) dat aan een gebruiker van AEM met toegang tot de inhoud van GraphQL WKND voor authentiek verklaart.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. De toepassing samenstellen met behulp van Xcode en de toepassing implementeren in iOS-simulator
1. Een lijst met avonturen van de WKND-site moet in de toepassing worden weergegeven. Het selecteren van een avontuur opent de avontuurdetails. In de lijst met avonturen kunt u de gegevens uit AEM vernieuwen.

## De code

Hieronder volgt een overzicht van de manier waarop de iOS-toepassing is gebouwd, hoe deze verbinding maakt met AEM Headless om inhoud op te halen met behulp van GraphQL persisted query&#39;s en hoe deze gegevens worden gepresenteerd. De volledige code kan op [&#x200B; GitHub &#x200B;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) worden gevonden.

### Blijvende query&#39;s

Na de beste praktijken van AEM Headless, gebruikt de toepassing van iOS AEM GraphQL voortgezette vragen om avontuurgegevens te vragen. De toepassing gebruikt twee voortgeduurde vragen:

+ `wknd/adventures-all` bleef query uitvoeren, die alle avonturen in AEM retourneert met een verkorte set eigenschappen. Deze hardnekkige vraag drijft de aanvankelijke lijst van het avontuur van de mening.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }

query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug` persisted query, die één avontuur retourneert van `slug` (een aangepaste eigenschap die een avontuur op unieke wijze identificeert) met een volledige set eigenschappen. Dit bleef vraagbevoegdheden de meningen van het avontuurdetail.

```
query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### GraphQL-query uitgevoerd

AEM persisted query&#39;s worden uitgevoerd via HTTP GET en daarom kunnen veelgebruikte GraphQL-bibliotheken die HTTP POST gebruiken, zoals Apollo, niet worden gebruikt. Maak in plaats daarvan een aangepaste klasse die de voortgezette HTTP-query-aanvragen bij AEM uitvoert.

`AEM/Aem.swift` instantieert de klasse `Aem` die wordt gebruikt voor alle interacties met AEM Headless. Het patroon is:

1. Elke voortgezette query heeft een overeenkomende openbare functie (bijv. `getAdventures(..)` of `getAdventureBySlug(..)` ) de weergave van de iOS-toepassing wordt aangeroepen om gegevens over avontuur op te halen.
1. De openbare functie roept een privéfunctie `makeRequest(..)` aan die een asynchrone HTTP GET-aanvraag naar AEM Headless aanroept en de JSON-gegevens retourneert.
1. Elke openbare functie decodeert vervolgens de JSON-gegevens en voert de vereiste controles of transformaties uit voordat de Adventure-gegevens naar de weergave worden geretourneerd.

   + AEM GraphQL JSON-gegevens worden gedecodeerd met de structs/klassen die zijn gedefinieerd in `AEM/Models.swift` , die zijn toegewezen aan de JSON-objecten en die mijn AEM Headless hebben geretourneerd.

```swift
    /// # getAdventures(..)
    /// Returns all WKND adventures using the `wknd-shared/adventures-all` persisted query.
    /// For this func call to work, the `wknd-shared/adventures-all` query must be deployed to the AEM environment/service specified by the host.
    /// 
    /// Since HTTP requests are async, the completion syntax is used.
    func getAdventures(params: [String:String], completion: @escaping ([Adventure]) ->  ()) {
               
        let request = makeRequest(persistedQueryName: "wknd-shared/adventures-all", params: params)
        
        URLSession.shared.dataTask(with: request) { (data, response, error) in
            if ((error) != nil) {
                print("Unable to connect to AEM GraphQL endpoint")
                completion([])
            } else if (!data!.isEmpty) {
                let adventures = try! JSONDecoder().decode(Adventures.self, from: data!)
                DispatchQueue.main.async {
                    completion(adventures.data.adventureList.items)
                }
            }
        }.resume();
    }
    
    ...

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

        // Add authentication to the AEM GraphQL persisted query requests as defined by the iOS application's configuration
        request = addAuthHeaders(request: request)
        
        return request
    }
    
    ...
```

### GraphQL-responsgegevensmodellen

iOS geeft de voorkeur aan het toewijzen van JSON-objecten aan getypte gegevensmodellen.

`src/AEM/Models.swift` bepaalt [&#x200B; decodable &#x200B;](https://developer.apple.com/documentation/swift/decodable) Swift structs en klassen die aan de antwoorden van AEM JSON in kaart brengen die door AEM JSON antwoorden zijn teruggekeerd.

### Weergaven

SwiftUI wordt gebruikt voor de diverse meningen in de toepassing. Apple verstrekt een het worden begonnen leerprogramma voor [&#x200B; bouwend lijsten en navigatie met SwiftUI &#x200B;](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  De vermelding van de toepassing en bevat `AdventureListView` waarvan de `.onAppear` -gebeurtenishandler wordt gebruikt om alle avonturgegevens op te halen via `aem.getAdventures()` . Het gedeelde `aem` voorwerp wordt hier geïnitialiseerd, en blootgesteld aan andere meningen als [&#x200B; EnvironmentObject &#x200B;](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Hiermee geeft u een lijst met avonturen weer (op basis van de gegevens van `aem.getAdventures()` ) en geeft u een lijstitem voor elk avontuur weer met behulp van `AdventureListItemView` .

+ `Views/AdventureListItemView.swift`

  Toont elk punt in de avonturenlijst (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Toont de details van een avontuur met inbegrip van de titel, de beschrijving, de prijs, het type van activiteit, en primair beeld. In deze weergave wordt met `aem.getAdventureBySlug(slug: slug)` naar AEM gezocht voor volledige avontuurgegevens. De parameter `slug` wordt hierbij doorgegeven op basis van de rij met de selectielijst.

### Externe afbeeldingen

Afbeeldingen waarnaar wordt verwezen door adventure Content Fragments, worden aangeboden door AEM. Deze iOS-toepassing gebruikt het veld path `_dynamicUrl` in de GraphQL-reactie en plaatst de voorvoegsels `AEM_SCHEME` en `AEM_HOST` voor het maken van een volledig gekwalificeerde URL. Als `_dynamicUrl` zich ontwikkelt tegen de AE SDK, wordt null geretourneerd, zodat de ontwikkeling terugvalt op het `_path` -veld van de afbeelding.

Als u verbinding wilt maken met beveiligde bronnen op AEM waarvoor toestemming vereist is, moeten ook referenties worden toegevoegd aan afbeeldingsaanvragen.

[&#x200B; SDWebImageSwiftUI &#x200B;](https://github.com/SDWebImage/SDWebImageSwiftUI) en [&#x200B; SDWebImage &#x200B;](https://github.com/SDWebImage/SDWebImage) worden gebruikt om de verre beelden van AEM te laden die het beeld van het Avontuur op `AdventureListItemView` en `AdventureDetailView` meningen bevolken.

De `aem` -klasse (in `AEM/Aem.swift` ) vereenvoudigt het gebruik van AEM-afbeeldingen op twee manieren:

1. `aem.imageUrl(path: String)` wordt gebruikt in weergaven om het AEM-schema voor te bereiden en als host op te slaan voor het afbeeldingspad, zodat een volledig gekwalificeerde URL wordt gemaakt.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. De `convenience init(..)` in `Aem` stelt HTTP-autorisatieheaders in op de HTTP-afbeeldingsaanvraag op basis van de configuratie van iOS-toepassingen.

   + Als __basisauthentificatie__ wordt gevormd, dan wordt de basisauthentificatie in bijlage aan alle beeldverzoeken.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Basic authentication init
   /// Used when authenticating to AEM using local accounts (basic auth)
   convenience init(scheme: String, host: String, username: String, password: String) {
       ...
   
       // Add basic auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Basic \(encodeBasicAuth(username: username, password: password))", forHTTPHeaderField: "Authorization")
   }
   ```

   + Als __symbolische authentificatie__ wordt gevormd, dan is de symbolische authentificatie in bijlage aan alle beeldverzoeken.

   ```swift
   /// AEM/Aem.swift
   ///
   /// # Token authentication init
   ///  Used when authenticating to AEM using token authentication (Dev Token or access token generated from Service Credentials)
   convenience init(scheme: String, host: String, token: String) {
       ...
   
       // Add token auth headers to all Image requests, as they are (likely) protected as well
       SDWebImageDownloader.shared.setValue("Bearer \(token)", forHTTPHeaderField: "Authorization")
   }
   ```

   + Als __geen authentificatie__ wordt gevormd, dan wordt geen authentificatie in bijlage aan beeldverzoeken.

Een gelijkaardige benadering kan met SwiftUI-inheemse [&#x200B; AsyncImage &#x200B;](https://developer.apple.com/documentation/swiftui/asyncimage) worden gebruikt. `AsyncImage` wordt ondersteund door iOS 15.0+.

## Aanvullende bronnen

+ [&#x200B; Begonnen het Worden met de Zetel van AEM - Zelfstudie van GraphQL &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=nl-NL)
+ [&#x200B; Lijsten SwiftUI en het Leerprogramma van de Navigatie &#x200B;](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
