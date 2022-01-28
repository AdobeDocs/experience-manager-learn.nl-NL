---
title: Ontdek de AEM GraphQL API - Geavanceerde concepten van AEM zonder kop - GraphQL
description: Verzend vragen GraphQL gebruikend winde GraphiQL. Leer over geavanceerde vragen gebruikend filters, variabelen, en richtlijnen. Vraag naar fragment- en inhoudsverwijzingen, inclusief verwijzingen uit tekstvelden met meerdere regels.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 0%

---

# De AEM GraphQL API verkennen

Met de API GraphQL in AEM kunt u gegevens van inhoudsfragmenten toegankelijk maken voor downstreamtoepassingen. In het vorige [Zelfstudie GraphQL met meerdere stappen](../multi-step/explore-graphql-api.md), verkende u GraphiQL Geïntegreerde Ontwikkelomgeving (winde) waar u bepaalde gemeenschappelijke vragen GraphQL testte en verfijnde. In dit hoofdstuk, zult u IDE GraphiQL gebruiken om geavanceerdere vragen te onderzoeken om gegevens van de Fragments van de Inhoud te verzamelen die u in het vorige hoofdstuk creeerde.

## Vereisten {#prerequisites}

Dit document is onderdeel van een zelfstudie met meerdere onderdelen. Controleer of de vorige hoofdstukken zijn voltooid voordat u verdergaat met dit hoofdstuk.

GraphiQL IDE moet worden geïnstalleerd alvorens dit hoofdstuk te voltooien. Volg de installatie-instructies in het vorige [Zelfstudie GraphQL met meerdere stappen](../multi-step/explore-graphql-api.md) voor meer informatie .

## Doelstellingen {#objectives}

In dit hoofdstuk leert u hoe u:

* Een lijst met inhoudsfragmenten filteren met verwijzingen met behulp van queryvariabelen
* Filter voor inhoud binnen een fragmentverwijzing
* Query voor inline-inhoud en fragmentverwijzingen vanuit een tekstveld met meerdere regels
* Query uitvoeren met instructies
* Query voor het inhoudstype JSON-object

## Een lijst met inhoudsfragmenten filteren met behulp van queryvariabelen

In het vorige [Zelfstudie GraphQL met meerdere stappen](../multi-step/explore-graphql-api.md)hebt u geleerd hoe u een lijst met inhoudsfragmenten kunt filteren. Hier, zult u deze kennis uitbreiden en zult filtreren gebruikend variabelen.

Wanneer het ontwikkelen van cliënttoepassingen, in de meeste gevallen zult u Inhoudsfragmenten moeten filtreren die op dynamische argumenten worden gebaseerd. Met de AEM GraphQL-API kunt u deze argumenten als variabelen in een query doorgeven om te voorkomen dat tekenreeksen op de client worden samengesteld tijdens runtime. Voor meer informatie over variabelen GraphQL, zie [documentatie van GraphQL](https://graphql.org/learn/queries/#variables).

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

   De `listPersonBySkill` query hierboven accepteert één variabele (`skillFilter`) is vereist `String`. Deze query voert een zoekopdracht uit tegen alle Person Content Fragments en filtert deze op basis van de `skills` veld en de tekenreeks die wordt doorgegeven `skillFilter`.

   Let op: `listPersonBySkill` bevat de `contactInfo` eigenschap, die een fragmentverwijzing is naar het contactinfo-model dat in de vorige hoofdstukken is gedefinieerd. Het model Contactinfo bevat `phone` en `email` velden. U moet minstens één van deze gebieden in de vraag voor het correct uitvoeren omvatten.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. Laten we nu definiëren `skillFilter` en krijg alle Instructeurs die in het skiën bekwaam zijn. Plak de volgende JSON-tekenreeks in het deelvenster Query-variabelen in de GraphiQL IDE:

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
               "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer.\nBorn in Baltimore, Maryland, Stacey is the youngest of six children. Her father was a lieutenant colonel in the US Navy and her mother was a modern dance instructor. Her family moved frequently with her father’s duty assignments, and she took her first pictures when he was stationed in Thailand. This is also where Stacey learned to rock climb."
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

## Filter voor inhoud binnen een fragmentverwijzing

Met de AEM GraphQL API kunt u zoeken naar geneste inhoudsfragmenten. In het vorige hoofdstuk hebt u drie nieuwe fragmentverwijzingen toegevoegd aan een Adventure Content-fragment: `location`, `instructorTeam`, en `administrator`. Nu, filter alle avonturen voor om het even welke Beheerder die een bepaalde naam heeft.

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
         adventureTitle
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

   De `getAdventureAdministratorDetailsByAdministratorName` query filtert all Adventures for any `administrator` van `fullName` &quot;Jacob Wester&quot;, die informatie van over twee genestelde Fragments van de Inhoud terugkeert: Adventure en instructeur.

1. Voer de vraag uit. Het resultaat moet er ongeveer als volgt uitzien:

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "adventureTitle": "Yosemite Backpacking",
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
                         "value": "Jacob Wester has been coordinating backpacking adventures for 3 years."
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

Met de AEM GraphQL API kunt u zoeken naar inhoud- en fragmentverwijzingen in tekstvelden met meerdere regels. In het vorige hoofdstuk hebt u beide referentietypen toegevoegd aan het dialoogvenster **Beschrijving** veld van het Yosemite Team Content Fragment. Laten we deze referenties nu ophalen.

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

   De `getTeamByAdventurePath` query filtert alle avonturen per pad en retourneert gegevens voor de `instructorTeam` fragmentverwijzing van een specifiek avontuur.

   `_references` is een door het systeem gegenereerd veld dat wordt gebruikt om verwijzingen weer te geven, inclusief die welke in tekstvelden met meerdere regels zijn ingevoegd.

   De `getTeamByAdventurePath` query haalt meerdere verwijzingen op. Eerst wordt de ingebouwde `ImageRef` object om het `_path` en `__typename` van afbeeldingen die als inhoudsverwijzingen zijn ingevoegd in het tekstveld met meerdere regels. Vervolgens wordt `LocationModel` om de gegevens op te halen van het Locatie-inhoudfragment dat in hetzelfde veld wordt ingevoegd.

   Merk op dat de vraag ook omvat `_metadata` veld. Hierdoor kunt u de naam van het Team Content Fragment ophalen en later in de WKND-app weergeven.

1. Vervolgens plakt u de volgende JSON-tekenreeks in het deelvenster Query-variabelen om het Yosemite Backpackaging Adventure op te halen:

   ```json
   {
   	    "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   De `_references` in het veld wordt zowel de afbeelding met het logo als het fragment met de inhoud van de Yosemite-vallei weergegeven dat in het **Beschrijving** veld.


## Query uitvoeren met instructies

Soms moet u bij het ontwikkelen van clienttoepassingen de structuur van uw query&#39;s voorwaardelijk wijzigen. In dit geval, staat AEM GraphQL API u toe om richtlijnen te gebruiken GraphQL om het gedrag van uw vragen te veranderen die op de verstrekte criteria worden gebaseerd. Voor meer informatie over GraphQL-richtlijnen raadpleegt u de [documentatie van GraphQL](https://graphql.org/learn/queries/#directives).

In de [vorige sectie](#query-rte-reference)hebt u geleerd hoe u query&#39;s kunt uitvoeren voor inline-verwijzingen binnen tekstvelden met meerdere regels. De inhoud is opgehaald uit het dialoogvenster `description` ingediend in `plaintext` gebruiken. Daarna, breiden die vraag uit en gebruiken een richtlijn om voorwaardelijk terug te winnen `description` in de `json` ook de notatie.

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

   De bovenstaande query accepteert nog een variabele (`includeJson`) is vereist `Boolean`, ook wel bekend als de queryrichtlijn. Een richtlijn kan worden gebruikt om voorwaardelijk gegevens van te omvatten `description` in het `json` formaat dat op boolean wordt gebaseerd die binnen wordt overgegaan `includeJson`.

1. Vervolgens plakt u de volgende JSON-tekenreeks in het deelvenster Query-variabelen:

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. Voer de vraag uit. U krijgt hetzelfde resultaat als in de vorige sectie over [hoe u query&#39;s kunt uitvoeren voor inline-verwijzingen in tekstvelden met meerdere regels](#query-rte-reference).

1. Werk de `includeJson` richtlijn `true` en voer de vraag opnieuw uit. Het resultaat moet er ongeveer als volgt uitzien:

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
                         "path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
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
                         "href": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## Query voor het inhoudstype JSON-object

Vergeet niet dat u in het vorige hoofdstuk over het ontwerpen van inhoudsfragmenten een JSON-object hebt toegevoegd aan het dialoogvenster **Weer na seizoen** veld. Laten we die gegevens nu ophalen in het locatieinhoudsfragment.

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
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park"
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
                     "value": "Yosemite National Park is in California’s Sierra Nevada mountains. It’s famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
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

   De `weatherBySeason` bevat het JSON-object dat in het vorige hoofdstuk is toegevoegd.

## Query uitvoeren voor alle inhoud tegelijk

Tot nu toe zijn er meerdere query&#39;s uitgevoerd om de mogelijkheden van de AEM GraphQL API te illustreren. Dezelfde gegevens kunnen met slechts één query worden opgehaald:

```graphql
query getAllAdventureDetails($fragmentPath: String!) {
  adventureByPath(_path: $fragmentPath){
    item {
      _path
      adventureTitle
      adventureActivity
      adventureType
      adventurePrice
      adventureTripLength
      adventureGroupSize
      adventureDifficulty
      adventurePrice
      adventurePrimaryImage{
        ...on ImageRef{
          _path
          mimeType
          width
          height
        }
      }
      adventureDescription {
        html
        json
      }
      adventureItinerary {
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
        contactInfo{
          phone
          email
        }
        locationImage{
          ...on ImageRef{
            _path
          }
        }
        weatherBySeason
        address{
            streetAddress
            city
            state
            zipCode
            country
        }
      }
      instructorTeam {
        _metadata{
            stringMetadata{
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
            profilePicture{
                ...on ImageRef {
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
        ...on ImageRef {
            _path
        mimeType
        }
        ...on LocationModel {
            _path
                __typename
        }
    }
  }
}

# in Query Variables
{
  "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
}
```

## Aanvullende query&#39;s voor de WKND-app

De volgende query&#39;s worden weergegeven om alle gegevens op te halen die nodig zijn in de WKND-app. Deze vragen tonen geen nieuwe concepten aan en worden slechts verstrekt als verwijzing om u te helpen uw implementatie bouwen.

1. **Krijg teamleden voor een specifiek avontuur**:

   ```graphql
   query getTeamMembersByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath ) {
       item {
         instructorTeam {
           teamMembers{
             fullName
             contactInfo{
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
             biography{
               plaintext
             }
           }
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **Krijg de weg van de Plaats voor een specifiek avontuur**

   ```graphql
   query getLocationPathByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath){
       item {
         location{
           _path  
         } 
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **De teamlocatie ophalen via het pad**

   ```graphql
   query getTeamLocationByLocationPath ($fragmentPath: String!){
     locationByPath (_path: $fragmentPath) {
       item {
         name
         description{
           json
         }
         contactInfo{
           phone
           email
         }
           address{
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge"
   }
   ```

## Gefeliciteerd!

Gefeliciteerd! U hebt nu geavanceerde query&#39;s getest om gegevens te verzamelen van de inhoudsfragmenten die u in het vorige hoofdstuk hebt gemaakt.

## Volgende stappen

In de [volgende hoofdstuk](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md), zult u leren hoe te om vragen te blijven GraphQL en waarom het beste praktijken is om voortgeduurde vragen in uw toepassingen te gebruiken.