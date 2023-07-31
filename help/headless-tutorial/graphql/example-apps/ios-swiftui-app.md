---
title: iOS App - voorbeeld zonder kop AEM
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze iOS-toepassing laat zien hoe u inhoud kunt opvragen met behulp van AEM GraphQL API's met behulp van doorlopende query's.
version: Cloud Service
mini-toc-levels: 2
kt: 10587
thumbnail: KT-10587.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: 6c5373db-86ec-410b-8a3b-9d4f86e06812
source-git-commit: 7938325427b6becb38ac230a3bc4b031353ca8b1
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 0%

---

# iOS-app

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze iOS-toepassing laat zien hoe u inhoud kunt opvragen met behulp van AEM GraphQL API&#39;s met behulp van doorlopende query&#39;s.

![iOS SwiftUI-app met AEM Headless](./assets/ios-swiftui-app/ios-app.png)

De weergave [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [Xcode](https://developer.apple.com/xcode/) (vereist macOS)
+ [Git](https://git-scm.com/)

## AEM

De iOS-toepassing werkt met de volgende AEM implementatieopties. Alle implementaties vereisen de [WKND-site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) te installeren.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ Lokale instelling met [de SDK van AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)

De iOS-toepassing is ontworpen om verbinding te maken met een __AEM-publicatie__ omgeving, maar het kan inhoud van AEM Author bron als de authentificatie in de configuratie van de toepassing van iOS wordt verstrekt.

## Hoe wordt het gebruikt

1. Klonen met `adobe/aem-guides-wknd-graphql` opslagplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Starten [Xcode](https://developer.apple.com/xcode/) en opent u de map `ios-app`
1. Het bestand wijzigen `Config.xcconfig` bestand en update `AEM_SCHEME` en `AEM_HOST` om overeen te komen met uw doel-AEM-publicatieservice.

   ```plain
   // The http/https protocol scheme used to access the AEM_HOST
   AEM_SCHEME = https
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = publish-p123-e456.adobeaemcloud.com
   ```

   Als u verbinding maakt met AEM-auteur, voegt u de opdracht `AEM_AUTH_TYPE` en ondersteunende authenticatie-eigenschappen voor de `Config.xcconfig`.

   __Basisverificatie__

   De `AEM_USERNAME` en `AEM_PASSWORD` Een lokale AEM verifiëren met toegang tot WKND GraphQL-inhoud.

   ```plain
   AEM_AUTH_TYPE = basic
   AEM_USERNAME = admin
   AEM_PASSWORD = admin
   ```

   __Tokenverificatie__

   De `AEM_TOKEN` is een [toegangstoken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) die wordt geverifieerd aan een AEM gebruiker met toegang tot WKND GraphQL-inhoud.

   ```plain
   AEM_AUTH_TYPE = token
   AEM_TOKEN = abcd...0123
   ```

1. De toepassing samenstellen met behulp van Xcode en de toepassing implementeren in iOS-simulator
1. Een lijst met avonturen van de WKND-site moet in de toepassing worden weergegeven. Het selecteren van een avontuur opent de avontuurdetails. Voor de avonturenlijstmening, trek om de gegevens van AEM te verfrissen.

## De code

Hieronder volgt een overzicht van hoe de iOS-toepassing is gebouwd, hoe deze verbinding maakt met AEM Headless om inhoud op te halen met GraphQL persisted query&#39;s en hoe deze gegevens worden gepresenteerd. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app).

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

+ `wknd/adventure-by-slug` persistente query, die één avontuur retourneert door `slug` (een douanebezit dat uniek een avontuur identificeert) met een volledige reeks eigenschappen. Dit bleef vraagbevoegdheden de meningen van het avontuurdetail.

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

AEM persisted query&#39;s worden uitgevoerd via HTTP GET en daarom kunnen veelgebruikte GraphQL-bibliotheken die HTTP-POST gebruiken, zoals Apollo, niet worden gebruikt. In plaats daarvan, creeer een douaneklasse die de voortgezette vraagHTTP- verzoeken aan AEM uitvoert.

`AEM/Aem.swift` instantieert het `Aem` klasse gebruikt voor alle interacties met AEM Headless. Het patroon is:

1. Elke voortgezette query heeft een overeenkomende openbare functie (bijv. `getAdventures(..)` of `getAdventureBySlug(..)`) de opvattingen van de iOS-toepassing oproepen om avontuurgegevens op te halen.
1. De publieke functie noemt een particuliere func `makeRequest(..)` die een asynchrone HTTP-GET-aanvraag aanroept om Headless te AEM en de JSON-gegevens retourneert.
1. Elke openbare functie decodeert vervolgens de JSON-gegevens en voert de vereiste controles of transformaties uit voordat de Adventure-gegevens naar de weergave worden geretourneerd.

   + AEM JSON-gegevens van GraphQL worden gedecodeerd met de instructies/klassen die zijn gedefinieerd in `AEM/Models.swift`Deze kaart naar de JSON-objecten retourneerde mijn AEM Headless.

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

De `src/AEM/Models.swift` definieert de [decoderbaar](https://developer.apple.com/documentation/swift/decodable) Swift-structuren en -klassen die zijn toegewezen aan de AEM JSON-reacties die zijn geretourneerd door AEM JSON-reacties.

### Weergaven

SwiftUI wordt gebruikt voor de diverse meningen in de toepassing. Apple biedt een zelfstudie aan de slag voor [het bouwen van lijsten en navigatie met SwiftUI](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation).

+ `WKNDAdventuresApp.swift`

  De vermelding van de aanvraag en omvat `AdventureListView` wiens `.onAppear` gebeurtenishandler wordt gebruikt om alle avonturgegevens op te halen via `aem.getAdventures()`. De gedeelde `aem` object wordt hier geïnitialiseerd en als een [EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject).

+ `Views/AdventureListView.swift`

  Hiermee geeft u een lijst met avonturen weer (op basis van de gegevens van `aem.getAdventures()`) en geeft een lijstitem weer voor elk avontuur met behulp van de `AdventureListItemView`.

+ `Views/AdventureListItemView.swift`

  Hiermee geeft u elk item in de lijst met avonturen weer (`Views/AdventureListView.swift`).

+ `Views/AdventureDetailView.swift`

  Toont de details van een avontuur met inbegrip van de titel, de beschrijving, de prijs, het type van activiteit, en primair beeld. Deze mening vraagt AEM voor volledige adventure details gebruiken `aem.getAdventureBySlug(slug: slug)`, waarbij de `slug` wordt doorgegeven op basis van de rij met de selectielijst.

### Externe afbeeldingen

Afbeeldingen waarnaar wordt verwezen door adventure Content Fragments, worden AEM. Deze iOS-toepassing gebruikt het pad `_dynamicUrl` in het GraphQL-antwoord en stelt het `AEM_SCHEME` en `AEM_HOST` om een volledig gekwalificeerde URL te maken. Indien zich ontwikkelt tegen de AE SDK, `_dynamicUrl` retourneert null, dus voor ontwikkelingsfallback naar de `_path` veld.

Als verbinding wordt gemaakt met beveiligde bronnen op AEM waarvoor toestemming vereist is, moeten ook referenties worden toegevoegd aan afbeeldingsaanvragen.

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) en [SDWebImage](https://github.com/SDWebImage/SDWebImage) worden gebruikt om de verre beelden van AEM te laden die het beeld van het Avontuur op het `AdventureListItemView` en `AdventureDetailView` weergaven.

De `aem` klasse (in `AEM/Aem.swift`) vergemakkelijkt het gebruik van AEM afbeeldingen op twee manieren:

1. `aem.imageUrl(path: String)` wordt gebruikt in weergaven om het AEM-schema voor te bereiden en als host op te nemen voor het afbeeldingspad, zodat een volledig gekwalificeerde URL wordt gemaakt.

   ```swift
   // adventure.image() => /adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   
   let imageUrl = aem.imageUrl(path: adventure.image()) 
   // imageUrl => https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--741ed388-d5f8-4797-8095-10c896dc9f1d/example.jpg?quality=80&preferwebp=true
   ```

2. De `convenience init(..)` in `Aem` Stel HTTP-autorisatiekoppen in op de HTTP-aanvraag voor de afbeelding, op basis van de configuratie van de iOS-toepassingen.

   + Indien __basisverificatie__ wordt gevormd, dan wordt de basisauthentificatie in bijlage aan alle beeldverzoeken.

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

   + Indien __tokenverificatie__ wordt gevormd, dan wordt de symbolische authentificatie in bijlage aan alle beeldverzoeken.

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

   + Indien __geen verificatie__ wordt gevormd, dan wordt geen authentificatie in bijlage aan beeldverzoeken.

Een gelijkaardige benadering kan met SwiftUI-inheems worden gebruikt [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage). `AsyncImage` wordt ondersteund op iOS 15.0+.

## Aanvullende bronnen

+ [Aan de slag met AEM headless - zelfstudie voor GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [Zelfstudie voor SwiftUI-lijsten en navigatie](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
