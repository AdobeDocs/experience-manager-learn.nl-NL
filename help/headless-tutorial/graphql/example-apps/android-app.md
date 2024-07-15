---
title: Android-toepassing - voorbeeld zonder kop AEM
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze Android-toepassing laat zien hoe u inhoud kunt opvragen met de GraphQL API's van AEM.
version: Cloud Service
mini-toc-levels: 2
jira: KT-10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM, hoofdloos as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 160
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---

# Android App

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze Android-toepassing laat zien hoe u inhoud kunt opvragen met de GraphQL API&#39;s van AEM. De [ AEM Zwaarloze Cliënt voor Java ](https://github.com/adobe/aem-headless-client-java) wordt gebruikt om de vragen van GraphQL uit te voeren en gegevens in kaart te brengen aan de voorwerpen van Java om app te drijven.

![ Android Java app met AEM Headless ](./assets/android-java-app/android-app.png)


Bekijk de [ broncode op GitHub ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [ de Studio van Android ](https://developer.android.com/studio)
+ [ Git ](https://git-scm.com/)

## AEM

De Android-toepassing werkt met de volgende AEM implementatieopties. Alle plaatsingen vereisen de [ Plaats van WKND v3.0.0+ ](https://github.com/adobe/aem-guides-wknd/releases/latest) om worden geïnstalleerd.

+ [ AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

De toepassing van Android wordt ontworpen om met een __AEM het milieu van Publish__ te verbinden, nochtans kan het inhoud van AEM Auteur als de authentificatie in de configuratie van de toepassing van Android wordt verstrekt.

## Hoe wordt het gebruikt

1. De gegevensopslagruimte `adobe/aem-guides-wknd-graphql` klonen:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Open [ Studio van Android ](https://developer.android.com/studio) en open de omslag `android-app`
1. Wijzig het bestand `config.properties` bij `app/src/main/assets/config.properties` en werk het `contentApi.endpoint` bij zodat dit overeenkomt met de AEM van het doel:

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __Basisauthentificatie__

   Met `contentApi.user` en `contentApi.password` kunt u een lokale AEM verifiëren met toegang tot WKND GraphQL-inhoud.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=my-special-android-app-user
   contentApi.password=password123
   ```

1. Download een [ Virtuele Apparaat van Android ](https://developer.android.com/studio/run/managing-avds) (minimum API 28).
1. Ontwikkel en implementeer de app met de Android-emulator.


### Verbinding maken met AEM omgevingen

Als het verbinden met een AEM auteursmilieu [ vergunning ](https://github.com/adobe/aem-headless-client-java#using-authorization) wordt vereist. [ AEMHeadlessClientBuilder ](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) verstrekt de capaciteit om [ op teken-gebaseerde authentificatie ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html) te gebruiken. Om op een token gebaseerde verificatie-update-clientbuilder te gebruiken in `AdventureLoader.java` en `AdventuresLoader.java` :

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## De code

Hieronder volgt een korte samenvatting van de belangrijke bestanden en code die worden gebruikt om de toepassing te besturen. De volledige code kan op [ GitHub ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app) worden gevonden.

### Blijvende query&#39;s

Na AEM Beste praktijken zonder hoofd, gebruikt de toepassing van iOS AEM GraphQL voortgezette vragen om avontuurgegevens te vragen. De toepassing gebruikt twee voortgeduurde vragen:

+ `wknd/adventures-all` persisted query, die alle avonturen in AEM met een verkorte set eigenschappen retourneert. Deze hardnekkige vraag drijft de aanvankelijke lijst van het avontuur van de mening.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _dynamicUrl
                _path
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` persisted query, die één avontuur retourneert van `slug` (een aangepaste eigenschap die een avontuur op unieke wijze identificeert) met een volledige set eigenschappen. Dit bleef vraagbevoegdheden de meningen van het avontuurdetail.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
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
          _dynamicUrl
          _path
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
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

AEM blijvende vragen worden uitgevoerd over de GET van HTTP en zo, wordt de [ AEM Zwaardeloze cliënt voor Java ](https://github.com/adobe/aem-headless-client-java) gebruikt om de voortgezette vragen van GraphQL tegen AEM uit te voeren en de avontuurinhoud in app te laden.

Elke voortgeduurde vraag heeft een overeenkomstige &quot;lader&quot;klasse, die asynchroon het AEM eindpunt van de GET van HTTP roept, en de avontuurgegevens terugkeert gebruikend de douane bepaalde [ gegevensmodel ](#data-models).

+ `loader/AdventuresLoader.java`

  Hiermee wordt de lijst met avonturen opgehaald op het beginscherm van de toepassing met de `wknd-shared/adventures-all` permanente query.

+ `loader/AdventureLoader.java`

  Hiermee wordt één avontuur opgehaald dat het via de parameter `slug` selecteert en de query `wknd-shared/adventure-by-slug` persisted gebruikt.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### GraphQL-responsgegevensmodellen{#data-models}

`Adventure.java` is een Java POJO die wordt geïnitialiseerd met de JSON-gegevens uit de GraphQL-aanvraag en een avontuur modelleert voor gebruik in de weergaven van de Android-toepassing.

### Weergaven

De Android-toepassing gebruikt twee weergaven om de avontuurgegevens in de mobiele ervaring te presenteren.

+ `AdventureListFragment.java`

  Roept `AdventuresLoader` aan en geeft de geretourneerde avonturen in een lijst weer.

+ `AdventureDetailFragment.java`

  Roept `AdventureLoader` aan gebruikend de `slug` param die via de avontuurselectie op de `AdventureListFragment` mening wordt overgegaan, en toont de details van één enkel avontuur.

### Externe afbeeldingen

`loader/RemoteImagesCache.java` is een hulpprogrammaklasse waarmee externe afbeeldingen in een cache kunnen worden voorbereid, zodat ze kunnen worden gebruikt met Android UI-elementen. De avontuurinhoud verwijst naar afbeeldingen in AEM Assets via een URL en deze klasse wordt gebruikt om die inhoud weer te geven.

## Aanvullende bronnen

+ [ Begonnen het worden met AEM Zwaartepunt - het Leerprogramma van GraphQL ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [ AEM Zwaarloze Cliënt voor Java ](https://github.com/adobe/aem-headless-client-java)
