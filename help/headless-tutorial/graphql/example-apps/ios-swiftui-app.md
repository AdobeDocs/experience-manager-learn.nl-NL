---
title: iOS SwiftUI-toepassing - Voorbeeld AEM zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Er is een iOS-toepassing beschikbaar die laat zien hoe inhoud kan worden opgevraagd met behulp van de GraphQL API's van AEM. De Apollo-client iOS wordt gebruikt om de GraphQL-query's te genereren en gegevens toe te wijzen aan Swift-objecten om de app te activeren. SwiftUI wordt gebruikt om een eenvoudige lijst en detailmening van de inhoud terug te geven.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9c1649247c65a1fa777b7574d1ab6ab49d0f722b
workflow-type: tm+mt
source-wordcount: '947'
ht-degree: 0%

---


# iOS SwiftUI App

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze iOS-toepassing laat zien hoe u inhoud kunt opvragen met behulp van de GraphQL-API&#39;s van AEM. De Apollo-client iOS wordt gebruikt om de GraphQL-query&#39;s te genereren en gegevens toe te wijzen aan Swift-objecten om de app te activeren. SwiftUI wordt gebruikt om een eenvoudige lijst en detailmening van de inhoud terug te geven.

>[!VIDEO](https://video.tv.adobe.com/v/338042/?quality=12&learn=on)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

* [Xcode 9.3+](https://developer.apple.com/xcode/)
* [Git](https://git-scm.com/)

## AEM

De toepassing is ontworpen om verbinding te maken met een AEM **Publiceren** milieu met de meest recente introductie van de [WKND-referentiesite](https://github.com/adobe/aem-guides-wknd/releases/latest) geïnstalleerd.

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html)

We raden aan [het opstellen van de plaats van de Verwijzing WKND aan een milieu van de Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). Een lokale instelling die [de SDK van AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) of [AEM 6.5 QuickStart-jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) kan ook worden gebruikt.

## Hoe wordt het gebruikt

1. Klonen met `aem-guides-wknd-graphql` opslagplaats:

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Starten [Xcode](https://developer.apple.com/xcode/) en opent u de map `ios-swiftui-app`
1. Het bestand wijzigen `Config.xcconfig` bestand en update `AEM_HOST` om aan uw doel-AEM-publicatieomgeving aan te passen

   ```plain
   // Target hostname for AEM environment, do not include http:// or https://
   AEM_HOST = localhost:4503
   // GraphQL Endpoint
   AEM_GRAPHQL_ENDPOINT = /content/cq:graphql/wknd/endpoint.json
   ```

1. De toepassing samenstellen met behulp van Xcode en de toepassing implementeren in iOS-simulator
1. Op de toepassing moet een lijst met avonturen van de WKND-referentiesite worden weergegeven.

## De code

Hieronder volgt een korte samenvatting van de belangrijke bestanden en code die worden gebruikt om de toepassing te besturen. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-swiftui-app).

### Apollo iOS

De [Apollo iOS](https://www.apollographql.com/docs/ios/) De client wordt door de toepassing gebruikt om de GraphQL-query uit te voeren tegen AEM. De ambtenaar [Apollo-zelfstudie](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/) heeft veel meer details over hoe te installeren en te gebruiken.

`schema.json` is een dossier dat het GrafiekQL- schema van een AEM milieu met de geïnstalleerde plaats van de Verwijzing WKND vertegenwoordigt. `schema.json` is gedownload van AEM en toegevoegd aan het project. De Apollo-client inspecteert alle bestanden met de extensie `.graphql` als onderdeel van een aangepaste build-fase. De Apollo-client gebruikt vervolgens de `schema.json` en alle `.graphql` vragen om het bestand automatisch te genereren `API.swift`.

Dit verstrekt de toepassing een sterk getypt model om de vraag uit te voeren en model(en) die de resultaten vertegenwoordigen.

![Aangepaste Xcode-constructiefase](assets/ios-swiftui-app/xcode-build-phase-apollo.png)

`AdventureList.graphql` bevat de vraag die wordt gebruikt om de avonturen te vragen:

```
query AdventureList
{
  adventureList {
    items {
      _path
      adventureTitle
      adventurePrice
      adventureActivity
      adventureDescription {
        plaintext
        markdown
      }
      adventureDifficulty
      adventureTripLength
      adventurePrimaryImage {
        ...on ImageRef {
          _authorUrl
          _publishUrl
        }
      }
    }
  }
}
```

`Network.swift` bouwt de `ApolloClient`. De `endpointURL` wordt geconstrueerd door de waarden van de `Config.xcconfig` bestand. Als u verbinding wilt maken met een AEM **Auteur** instantie en nodig om extra kopballen voor authentificatie toe te voegen, zou u willen wijzigen `ApolloClient` hier.

```swift
// Network.swift
private(set) lazy var apollo: ApolloClient = {
        // The cache is necessary to set up the store, which we're going to hand to the provider
        let cache = InMemoryNormalizedCache()
        let store = ApolloStore(cache: cache)
  
        let client = URLSessionClient()
        let provider = DefaultInterceptorProvider(client: client, shouldInvalidateClientOnDeinit: true, store: store)
        let url = Connection.baseURL // from Configx.xcconfig 

        // no additional headers, public instances by default require no additional authentication
        let requestChainTransport = RequestChainNetworkTransport(interceptorProvider: provider, endpointURL: url)

        return ApolloClient(networkTransport: requestChainTransport,store: store)
    }()
}
```

### Adventure-gegevens

De toepassing wordt ontworpen om een lijst van avonturen en dan een detailmening van elk avontuur te tonen.

`AdventuresDataModel.swift` is een klasse die een functie bevat `fetchAdventures()`. Deze functie gebruikt de `ApolloClient` om de query uit te voeren. Bij een geslaagde query zal de resultatenarray van het type zijn `AdventureListQuery.Data.AdventureList.Item`, automatisch gegenereerd door de `API.swift` bestand.

```swift
func fetchAdventures() {
        Network.shared.apollo
            //AdventureListQuery() generated based on AdventureList.graphql file
           .fetch(query: AdventureListQuery()) { [weak self] result in
           
             guard let self = self else {
               return
             }
                   
             switch result {
             case .success(let graphQLResult):
                print("Success AdventureListQuery() from: \(graphQLResult.source)")

                if let adventureDataItems =  graphQLResult.data?.adventureList.items {
                    // map graphQL items to an array of Adventure objects
                    self.adventures = adventureDataItems.compactMap { Adventure(adventureData: $0!) }
                }
                ...
             }
           }
}
```

Het is mogelijk `AdventureListQuery.Data.AdventureList.Item` rechtstreeks om de toepassing aan te sturen. Het is echter zeer mogelijk dat sommige gegevens onvolledig zijn en dat sommige eigenschappen daarom null kunnen zijn.

`Adventure.swift` is een aangepast model dat is geïntroduceerd en fungeert als een omslag van het model dat is gegenereerd door Apollo. `Adventure` is geïnitialiseerd met `AdventureListQuery.Data.AdventureList.Item`. A `typealias` wordt gebruikt om de code leesbaarder te maken:

```
// use typealias
typealias AdventureData = AdventureListQuery.Data.AdventureList.Item
```

De `Adventure` struct is geïnitialiseerd met een `AdventureData` object:

```swift
struct Adventure: Identifiable {
    let id: String
    let adventureTitle: String
    let adventurePrice: String
    let adventureDescription: String
    let adventureActivity: String
    let adventurePrimaryImageUrl: String
    
    // initialize with AdventureData object aka AdventureListQuery.Data.AdventureList.Item
    init(adventureData: AdventureData) {
        // use path as unique idenitifer, otherwise
        self.id = adventureData._path ?? UUID().uuidString
        self.adventureTitle = adventureData.adventureTitle ?? "Untitled"
        self.adventurePrice = adventureData.adventurePrice ?? "Free"
        self.adventureActivity = adventureData.adventureActivity ?? ""
        ...
```

Op deze manier kunnen we standaardwaarden opgeven en aanvullende controles voor onvolledige gegevens uitvoeren. We kunnen dan de `Adventure` model veilig om diverse elementen UI te aandrijven en te hoeven niet constant om ongeldige waarden te controleren.

In AEM worden stukken inhoud op unieke wijze geïdentificeerd door `_path`. In `Adventure.swift` we vullen de `id` eigenschap met de waarde van `_path`. Dit maakt `Adventure` de `Identifiable` en maakt het makkelijker om een array of lijst te doorlopen.

### Weergaven

SwiftUI wordt gebruikt voor de diverse meningen in de toepassing. Een geweldige zelfstudie voor [lijsten en navigatie samenstellen](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation) vindt u op de Apple-site voor ontwikkelaars. De code voor deze toepassing is er los van afgeleid.

`WKNDAdventuresApp.swift` De vermelding van de aanvraag. Hieronder vallen `AdventureListView` en de `.onAppear` event wordt gebruikt om de avontuurgegevens op te halen.

`AdventureListView.swift` - maakt een `NavigationView` en een lijst van door `AdventureRowView`. Navigatie naar een `AdventureDetailView` is hier ingesteld.

`AdventureRowView` - geeft het primaire beeld van het Avontuur en de Titel van het Avontuur in een rij weer.

`AdventureDetailView` - een volledig overzicht van het specifieke avontuur, met inbegrip van de titel, de beschrijving, de prijs, het type activiteit en het primaire beeld.

Wanneer de Apollo CLI loopt en regenereert `API.swift` de voorvertoning wordt gestopt. Als u de functie Automatische voorvertoning wilt gebruiken, moet u het dialoogvenster **Apollo CLI** Fase maken en controleren om het script uit te voeren **Alleen voor installatiebuilds**.

![Alleen controleren op builds](assets/ios-swiftui-app/update-build-phases.png)

### Externe afbeeldingen

[SDWebImageSwiftUI](https://github.com/SDWebImage/SDWebImageSwiftUI) en [SDWEbImage](https://github.com/SDWebImage/SDWebImage) worden gebruikt om de verre beelden van AEM te laden die het primaire beeld van het Avontuur op de mening van de Rij en van het Detail bevolken.

De [AsyncImage](https://developer.apple.com/documentation/swiftui/asyncimage) is een native SwiftUI-weergave die ook kan worden gebruikt. `AsyncImage` wordt alleen ondersteund voor iOS 15.0+.

## Aanvullende bronnen

* [Aan de slag met AEM headless - GraphQL-zelfstudie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [Zelfstudie voor SwiftUI-lijsten en navigatie](https://developer.apple.com/tutorials/swiftui/building-lists-and-navigation)
* [Zelfstudie voor Apollo iOS-client](https://www.apollographql.com/docs/ios/tutorial/tutorial-introduction/)

