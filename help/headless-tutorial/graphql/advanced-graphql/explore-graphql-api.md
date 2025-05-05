---
title: Ontdek de AEM GraphQL API - Geavanceerde concepten van AEM Headless - GraphQL
description: Verzend GraphQL-query's met behulp van GraphiQL IDE. Leer over geavanceerde vragen gebruikend filters, variabelen, en richtlijnen. Vraag naar fragment- en inhoudsverwijzingen, inclusief verwijzingen uit tekstvelden met meerdere regels.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
duration: 438
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# De AEM GraphQL API verkennen

Met de GraphQL API in AEM kunt u gegevens van inhoudsfragmenten toegankelijk maken voor downstreamtoepassingen. In het basisleerprogramma [ multi-step zelfstudie van GraphQL ](../multi-step/explore-graphql-api.md), gebruikte u de Ontdekkingsreiziger GraphiQL om de vragen van GraphQL te testen en te raffineren.

In dit hoofdstuk, gebruikt u de Ontdekkingsreiziger GraphiQL om geavanceerdere vragen te bepalen om gegevens van de Fragmenten van de Inhoud te verzamelen die u in het [ vorige hoofdstuk ](../advanced-graphql/author-content-fragments.md) creeerde.

## Vereisten {#prerequisites}

Dit document is onderdeel van een zelfstudie met meerdere onderdelen. Controleer of de vorige hoofdstukken zijn voltooid voordat u verdergaat met dit hoofdstuk.

## Doelstellingen {#objectives}

In dit hoofdstuk leert u hoe te:

* Een lijst met inhoudsfragmenten filteren met verwijzingen met behulp van queryvariabelen
* Filter voor inhoud binnen een fragmentverwijzing
* Query voor inline-inhoud en fragmentverwijzingen vanuit een tekstveld met meerdere regels
* Query uitvoeren met instructies
* Query voor het inhoudstype JSON-object

## De GraphiQL Explorer gebruiken


Het [&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=nl-NL) hulpmiddel van de Ontdekkingsreiziger 0&rbrace; GraphiQL laat ontwikkelaars toe om, en testvragen tegen inhoud op het huidige milieu van AEM tot stand te brengen.  Het hulpmiddel GraphiQL laat ook gebruikers toe om **voort te zetten of** vragen te bewaren die door cliënttoepassingen in een productie het plaatsen moeten worden gebruikt.

Verken vervolgens de kracht van de AEM GraphQL API met behulp van de ingebouwde GraphiQL Explorer.

1. Van het scherm van het Begin van AEM, navigeer aan **Hulpmiddelen** > **Algemeen** > **de Redacteur van de Vraag van GraphQL**.

   ![ navigeer aan GrahiQL winde ](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>In, moeten sommige versies van AEM (6.X.X) de Ontdekkingsreiziger GraphiQL (alias GrahiQL winde) hulpmiddel manueel worden geïnstalleerd, [ instructie van hier volgen ](../how-to/install-graphiql-aem-6-5.md).

1. In de hoger-juiste hoek, zorg ervoor dat het Eindpunt aan **WKND Gedeeld Eindpunt** wordt geplaatst. Het veranderen van de _drop-down waarde van het Eindpunt_ toont hier de bestaande _Gepersisteerde Vragen_ in de top-left hoek.

   ![ plaats GraphQL Eindpunt ](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

Dit zal werkingsgebied alle vragen aan modellen die in het **Gedeelde WKND** project worden gecreeerd.


## Een lijst met inhoudsfragmenten filteren met behulp van queryvariabelen

In het vorige [ multi-step GraphQL leerprogramma ](../multi-step/explore-graphql-api.md), bepaalde u, en gebruikte, fundamentele voortgeduurde vragen om de gegevens van Fragments van de Inhoud te krijgen. Hier, breidt u deze kennis uit en de gegevens van de Fragmenten van de filterinhoud door variabelen tot de persisted query over te gaan.

Wanneer het ontwikkelen van cliënttoepassingen, gewoonlijk moet u de Fragmenten van de Inhoud filtreren die op dynamische argumenten worden gebaseerd. Met de AEM GraphQL-API kunt u deze argumenten doorgeven als variabelen in een query om te voorkomen dat tekenreeksconstructies op de client worden uitgevoerd. Voor meer informatie over de variabelen van GraphQL, zie de [ documentatie van GraphQL ](https://graphql.org/learn/queries/#variables).

Voor dit voorbeeld, vraag alle Instructeurs die een bepaalde vaardigheid hebben.

1. In GrahiQL winde, kleef de volgende vraag in het linkerpaneel:

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
         fullName
         contactInfo {
           phone
           email
         }
         profilePicture {
           ... on ImageRef {
             _path
           }
         }
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   De bovenstaande `listPersonBySkill` query accepteert één variabele ( `skillFilter` ) die een vereiste `String` is. Deze query voert een zoekopdracht uit tegen alle Person Content Fragments en filtert deze op basis van het `skills` -veld en de tekenreeks die wordt doorgegeven in `skillFilter` .

   De eigenschap `listPersonBySkill` bevat de eigenschap `contactInfo` . Dit is een fragmentverwijzing naar het model Contactinfo dat in de vorige hoofdstukken is gedefinieerd. Het model Contactinfo bevat `phone` - en `email` -velden. Ten minste een van deze velden in de query moet aanwezig zijn om correct te kunnen functioneren.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. Vervolgens definiëren we `skillFilter` en krijgen we alle instructeurs die deskundig zijn op het gebied van skiën. Plak de volgende JSON-tekenreeks in het deelvenster Query-variabelen in de GraphiQL IDE:

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. Voer de vraag uit. Het resultaat moet er ongeveer als volgt uitzien:

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

Druk de **knoop van het Spel** in het hoogste menu om de vraag uit te voeren. De resultaten van de inhoudsfragmenten uit het vorige hoofdstuk worden weergegeven:

![ Persoon door de Resultaten van de Vaardigheid ](assets/explore-graphql-api/person-by-skill.png)

## Filter voor inhoud binnen een fragmentverwijzing

Met de AEM GraphQL API kunt u zoeken naar geneste inhoudsfragmenten. In het vorige hoofdstuk hebt u drie nieuwe fragmentverwijzingen toegevoegd aan een Adventure Content-fragment: `location` , `instructorTeam` en `administrator` . Nu, filter alle avonturen voor om het even welke Beheerder die een bepaalde naam heeft.

>[!CAUTION]
>
>Slechts één model moet als verwijzing voor deze vraag worden toegestaan correct te presteren.

1. In GrahiQL winde, kleef de volgende vraag in het linkerpaneel:

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         title
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. Vervolgens plakt u de volgende JSON-tekenreeks in het deelvenster Query-variabelen:

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   De query van `getAdventureAdministratorDetailsByAdministratorName` filtert alle avonturen op een `administrator` van `fullName` &quot;Jacob Wester&quot;, waarbij informatie uit twee geneste inhoudsfragmenten wordt geretourneerd: avontuur en instructeur.

1. Voer de vraag uit. Het resultaat moet er ongeveer als volgt uitzien:

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "title": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## Query voor inline-verwijzingen vanuit een tekstveld met meerdere regels {#query-rte-reference}

Met de AEM GraphQL API kunt u zoeken naar inhoud en fragmentverwijzingen binnen tekstvelden met meerdere regels. In het vorige hoofdstuk, voegde u beide verwijzingstypes in het **gebied van de Beschrijving** van het Fragment van de Inhoud van het Team Yosemite toe. Laten we deze referenties nu ophalen.

1. In GrahiQL winde, kleef de volgende vraag in het linkerpaneel:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   De query van `getTeamByAdventurePath` filtert alle avonturen per pad en retourneert gegevens voor de fragmentverwijzing van een specifiek avontuur van `instructorTeam` .

   `_references` is een door het systeem gegenereerd veld dat wordt gebruikt om verwijzingen weer te geven, inclusief die welke in tekstvelden met meerdere regels zijn ingevoegd.

   Met de query `getTeamByAdventurePath` worden meerdere referenties opgehaald. Eerst wordt het ingebouwde `ImageRef` -object gebruikt om de `_path` en `__typename` afbeeldingen op te halen die als inhoudsverwijzingen zijn ingevoegd in het tekstveld met meerdere regels. Vervolgens wordt `LocationModel` gebruikt om de gegevens op te halen van het Locatie-inhoudfragment dat in hetzelfde veld wordt ingevoegd.

   De query bevat ook het veld `_metadata` . Hierdoor kunt u de naam van het Team Content Fragment ophalen en later in de WKND-app weergeven.

1. Vervolgens plakt u de volgende JSON-tekenreeks in het deelvenster Query-variabelen om het Yosemite Backpackaging Adventure op te halen:

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. Voer de vraag uit. Het resultaat moet er ongeveer als volgt uitzien:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   Het `_references` gebied onthult zowel het embleembeeld als het Fragment van de Inhoud van de Inhoud van de Bodge van de Vallei Yosemite dat in het **2&rbrace; gebied van de Beschrijving werd opgenomen.**


## Query uitvoeren met instructies

Soms moet u bij het ontwikkelen van clienttoepassingen de structuur van uw query&#39;s voorwaardelijk wijzigen. In dit geval kunt u met de AEM GraphQL API GraphQL-instructies gebruiken om het gedrag van uw query&#39;s te wijzigen op basis van de opgegeven criteria. Voor meer informatie over de richtlijnen van GraphQL, zie de [ documentatie van GraphQL ](https://graphql.org/learn/queries/#directives).

In de [ vorige sectie ](#query-rte-reference), leerde u hoe te voor gealigneerde verwijzingen binnen multi-line tekstgebieden vragen. De inhoud is opgehaald uit het `description` -veld in de `plaintext` -indeling. Vervolgens vouwen we die query uit en gebruiken we een instructie om `description` ook voorwaardelijk op te halen in de `json` -indeling.

1. In GrahiQL winde, kleef de volgende vraag in het linkerpaneel:

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   De vraag hierboven keurt één meer variabele (`includeJson`) goed die vereist `Boolean` is, die ook als richtlijn van de vraag wordt bekend. Een instructie kan worden gebruikt om gegevens van het veld `description` voorwaardelijk op te nemen in de `json` -indeling op basis van de Booleaanse waarde die wordt doorgegeven in `includeJson` .

1. Vervolgens plakt u de volgende JSON-tekenreeks in het deelvenster Query-variabelen:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Voer de vraag uit. U zou het zelfde resultaat zoals in de vorige sectie op [ moeten krijgen hoe te om voor gealigneerde verwijzingen binnen multi-line tekstgebieden ](#query-rte-reference) te vragen.

1. Werk de aanwijzing `includeJson` bij naar `true` en voer de query opnieuw uit. Het resultaat moet er ongeveer als volgt uitzien:

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Query voor het inhoudstype JSON-object

Herinner dat in het vorige hoofdstuk op het ontwerpen van de Fragmenten van de Inhoud, u een voorwerp JSON in het **Weather door het 1&rbrace; gebied van de Verraad &lbrace;toevoegde.** Laten we die gegevens nu ophalen in het locatieinhoudsfragment.

1. In GrahiQL winde, kleef de volgende vraag in het linkerpaneel:

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
           json
         }
         contactInfo {
           phone
           email
         }
         locationImage {
           ... on ImageRef {
             _path
           }
         }
         weatherBySeason
         address {
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   ```

1. Vervolgens plakt u de volgende JSON-tekenreeks in het deelvenster Query-variabelen:

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. Voer de vraag uit. Het resultaat moet er ongeveer als volgt uitzien:

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   Het veld `weatherBySeason` bevat het JSON-object dat in het vorige hoofdstuk is toegevoegd.

## Query uitvoeren voor alle inhoud tegelijk

Tot dusver zijn meerdere query&#39;s uitgevoerd om de mogelijkheden van de AEM GraphQL API te illustreren.

De zelfde gegevens konden met slechts één enkele vraag worden teruggewonnen en deze vraag wordt later gebruikt in de cliënttoepassing om extra informatie zoals plaats, teamnaam, teamleden van een avontuur terug te winnen:

```graphql
query getAdventureDetailsBySlug($slug: String!) {
  adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
    items {
      _path
      title
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        html
        json
      }
      itinerary {
        html
        json
      }
      location {
        _path
        name
        description {
          html
          json
        }
        contactInfo {
          phone
          email
        }
        locationImage {
          ... on ImageRef {
            _path
          }
        }
        weatherBySeason
        address {
          streetAddress
          city
          state
          zipCode
          country
        }
      }
      instructorTeam {
        _metadata {
          stringMetadata {
            name
            value
          }
        }
        teamFoundingDate
        description {
          json
        }
        teamMembers {
          fullName
          contactInfo {
            phone
            email
          }
          profilePicture {
            ... on ImageRef {
              _path
            }
          }
          instructorExperienceLevel
          skills
          biography {
            html
          }
        }
      }
      administrator {
        fullName
        contactInfo {
          phone
          email
        }
        biography {
          html
        }
      }
    }
    _references {
      ... on ImageRef {
        _path
        mimeType
      }
      ... on LocationModel {
        _path
        __typename
      }
    }
  }
}


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## Gefeliciteerd!

Gefeliciteerd! U hebt nu geavanceerde query&#39;s getest om gegevens te verzamelen van de inhoudsfragmenten die u in het vorige hoofdstuk hebt gemaakt.

## Volgende stappen

In het [ volgende hoofdstuk ](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), zult u leren hoe te om de vragen van GraphQL voort te zetten en waarom het beste praktijken is om voortgeduurde vragen in uw toepassingen te gebruiken.
