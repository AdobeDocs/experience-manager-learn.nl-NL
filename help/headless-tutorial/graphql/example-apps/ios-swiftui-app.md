---
title: iOS App - voorbeeld zonder kop AEM
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze iOS-toepassing laat zien hoe u inhoud kunt opvragen met behulp van AEM GraphQL API's met behulp van doorlopende query's.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM, hoofdloos as a Cloud Service" before-title="false"
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
duration: 278
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 0%

---

# iOS-app

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze iOS-toepassing laat zien hoe u inhoud kunt opvragen met behulp van AEM GraphQL API&#39;s met behulp van doorlopende query&#39;s.

![ iOS SwiftUI app met AEM Headless ](./assets/ios-swiftui-app/ios-app.png)

Bekijk de [ broncode op GitHub ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [ Xcode ](https://developer.apple.com/xcode/) (vereist macOS)
+ [ Git ](https://git-scm.com/)

## AEM

De iOS-toepassing werkt met de volgende AEM implementatieopties. Alle plaatsingen vereisen de [ Plaats van WKND v3.0.0+ ](https://github.com/adobe/aem-guides-wknd/releases/latest) om worden geïnstalleerd.

+ [ AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ De lokale opstelling die [ SDK van AEM Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) gebruikt

De toepassing van iOS wordt ontworpen om met een __AEM het milieu van Publish__ te verbinden, nochtans kan het inhoud van AEM Auteur als de authentificatie in de configuratie van de toepassing van iOS wordt verstrekt.

## Hoe wordt het gebruikt

1. De gegevensopslagruimte `adobe/aem-guides-wknd-graphql` klonen:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Open [ Xcode ](https://developer.apple.com/xcode/) en open de omslag `ios-app`
1. Wijzig het bestand `Config.xcconfig` en werk `AEM_SCHEME` en `AEM_HOST` bij zodat deze overeenkomen met uw doel AEM Publish-service.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Als u verbinding maakt met AEM auteur, voegt u de eigenschappen `AEM_AUTH_TYPE` en support voor verificatie toe aan de `Config.xcconfig` .

   __Basisauthentificatie__

   Met `AEM_USERNAME` en `AEM_PASSWORD` kunt u een lokale AEM verifiëren met toegang tot WKND GraphQL-inhoud.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Symbolische authentificatie__

   `AEM_TOKEN` is een [ toegangstoken ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) dat aan een AEM gebruiker met toegang tot de inhoud van GraphQL WKND voor authentiek verklaart.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. De toepassing samenstellen met behulp van Xcode en de toepassing implementeren in iOS-simulator
1. Een lijst met avonturen van de WKND-site moet in de toepassing worden weergegeven. Het selecteren van een avontuur opent de avontuurdetails. Voor de avonturenlijstmening, trek om de gegevens van AEM te verfrissen.

## De code

Hieronder volgt een overzicht van hoe de iOS-toepassing is gebouwd, hoe deze verbinding maakt met AEM Headless om inhoud op te halen met GraphQL persisted query&#39;s en hoe deze gegevens worden gepresenteerd. De volledige code kan op [ GitHub ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) worden gevonden.

### Blijvende query&#39;s

Na AEM Beste praktijken zonder hoofd, gebruikt de toepassing van iOS AEM GraphQL voortgezette vragen om avontuurgegevens te vragen. De toepassing gebruikt twee voortgeduurde vragen:

+ `wknd/adventures-all` persisted query, die alle avonturen in AEM met een verkorte set eigenschappen retourneert. Deze hardnekkige vraag drijft de aanvankelijke lijst van het avontuur van de mening.

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

AEM voortgeduurde vragen worden uitgevoerd over de GET van HTTP en zo, kunnen de gemeenschappelijke bibliotheken van GraphQL die de POST van HTTP zoals Apollo gebruiken, niet worden gebruikt. In plaats daarvan, creeer een douaneklasse die de voortgezette vraagHTTP- verzoeken aan AEM uitvoert.

`AEM/Aem.swift` instantieert de klasse `Aem` die wordt gebruikt voor alle interacties met AEM Headless. Het patroon is:

1. Elke voortgezette query heeft een overeenkomende openbare functie (bijv. `getAdventures(..)` of `getAdventureBySlug(..)` ) de weergave van de iOS-toepassing wordt aangeroepen om gegevens over avontuur op te halen.
1. De openbare functie roept een privéfunctie `makeRequest(..)` aan die een asynchrone HTTP-aanvraag tot AEM Headless aanroept en de JSON-gegevens retourneert.
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

`src/AEM/Models.swift` bepaalt [ decodable ](https://developer.apple.com/documentation/swift/decodable) Swift structs en klassen die aan de AEM JSON reacties in kaart brengen die door AEM JSON reacties zijn teruggekeerd.

### Weergaven

SwiftUI wordt gebruikt voor de diverse meningen in de toepassing. Apple verstrekt een het worden begonnen leerprogramma voor [ bouwend lijsten en navigatie met SwiftUI ](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  De vermelding van de toepassing en bevat `AdventureListView` waarvan de `.onAppear` -gebeurtenishandler wordt gebruikt om alle avonturgegevens op te halen via `aem.getAdventures()` . Het gedeelde `aem` voorwerp wordt hier geïnitialiseerd, en blootgesteld aan andere meningen als [ EnvironmentObject ](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Hiermee geeft u een lijst met avonturen weer (op basis van de gegevens van `aem.getAdventures()` ) en geeft u een lijstitem voor elk avontuur weer met behulp van `AdventureListItemView` .

+ `Views/AdventureListItemView.swift`

  Toont elk punt in de avonturenlijst (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Toont de details van een avontuur met inbegrip van de titel, de beschrijving, de prijs, het type van activiteit, en primair beeld. In deze weergave wordt AEM gevraagd of er details zijn met betrekking tot volledige avontuur met `aem.getAdventureBySlug(slug: slug)` , waarbij de parameter `slug` wordt doorgegeven op basis van de rij met de selectielijst.

### Externe afbeeldingen

Afbeeldingen waarnaar wordt verwezen door adventure Content Fragments, worden AEM. Deze iOS-toepassing gebruikt het veld path `_dynamicUrl` in de GraphQL-reactie en plaatst de voorvoegsels `AEM_SCHEME` en `AEM_HOST` voor het maken van een volledig gekwalificeerde URL. Als `_dynamicUrl` zich ontwikkelt tegen de AE SDK, wordt null geretourneerd, zodat de ontwikkeling terugvalt op het `_path` -veld van de afbeelding.

Als verbinding wordt gemaakt met beveiligde bronnen op AEM waarvoor toestemming vereist is, moeten ook referenties worden toegevoegd aan afbeeldingsaanvragen.

[ SDWebImageSwiftUI ](https://github.com/SDWebImage/SDWebImageSwiftUI) en [ SDWebImage ](https://github.com/SDWebImage/SDWebImage) worden gebruikt om de verre beelden van AEM te laden die het beeld van het Avontuur op `AdventureListItemView` en `AdventureDetailView` meningen bevolken.

De `aem` -klasse (in `AEM/Aem.swift` ) vereenvoudigt het gebruik van AEM afbeeldingen op twee manieren:

1. `aem.imageUrl(path: String)` wordt gebruikt in weergaven om het AEM aan te vullen en het pad van de afbeelding te hosten, zodat een volledig gekwalificeerde URL wordt gemaakt.

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

Een gelijkaardige benadering kan met SwiftUI-inheemse [ AsyncImage ](https://developer.apple.com/documentation/swiftui/asyncimage) worden gebruikt. `AsyncImage` wordt ondersteund door iOS 15.0+.

## Aanvullende bronnen

+ [ Begonnen het worden met AEM Zwaartepunt - het Leerprogramma van GraphQL ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [ Lijsten SwiftUI en het Leerprogramma van de Navigatie ](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
