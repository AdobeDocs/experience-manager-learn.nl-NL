---
title: Android-toepassing - Voorbeeld AEM zonder kop
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
badgeVersions: label="AEM zonder hoofd as a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 190
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---

# Android-toepassing

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Deze Android-toepassing laat zien hoe u inhoud kunt opvragen met de GraphQL API&#39;s van AEM. De [AEM headless-client voor Java](https://github.com/adobe/aem-headless-client-java) wordt gebruikt om GraphQL-query&#39;s uit te voeren en gegevens toe te wijzen aan Java-objecten om de app te activeren.

![Android Java-app met AEM headless](./assets/android-java-app/android-app.png)


De weergave [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM

De Android-toepassing werkt met de volgende AEM implementatieopties. Alle implementaties vereisen de [WKND-site v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) te installeren.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

De Android-toepassing is ontworpen voor verbinding met een __AEM publiceren__ -omgeving, maar de toepassing kan inhoud van AEM auteur aanmaken als de configuratie van de Android-toepassing verificatie bevat.

## Hoe wordt het gebruikt

1. Klonen met `adobe/aem-guides-wknd-graphql` opslagplaats:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Starten [Android Studio](https://developer.android.com/studio) en opent u de map `android-app`
1. Het bestand wijzigen `config.properties` om `app/src/main/assets/config.properties` en bijwerken `contentApi.endpoint` om aan uw doel AEM milieu aan te passen:

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __Basisverificatie__

   De `contentApi.user` en `contentApi.password` Een lokale AEM verifiëren met toegang tot WKND GraphQL-inhoud.

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Download een [Virtueel Android-apparaat](https://developer.android.com/studio/run/managing-avds) (minimum API 28).
1. De app ontwikkelen en implementeren met de Android-emulator.


### Verbinding maken met AEM omgevingen

Als u verbinding maakt met een AEM-ontwerpomgeving [autorisatie](https://github.com/adobe/aem-headless-client-java#using-authorization) is vereist. De [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) biedt de mogelijkheid [tokenverificatie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Om op teken-gebaseerde cliënt van de authentificatieupdate binnen te gebruiken `AdventureLoader.java` en `AdventuresLoader.java`:

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

Hieronder volgt een korte samenvatting van de belangrijke bestanden en code die worden gebruikt om de toepassing te besturen. U vindt de volledige code op [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

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

+ `wknd/adventure-by-slug` persistente query, die één avontuur retourneert door `slug` (een douanebezit dat uniek een avontuur identificeert) met een volledige reeks eigenschappen. Dit bleef vraagbevoegdheden de meningen van het avontuurdetail.

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

AEM voortgeduurde vragen worden uitgevoerd over de GET van HTTP en zo, [AEM headless-client voor Java](https://github.com/adobe/aem-headless-client-java) wordt gebruikt om de aanhoudend GraphQL-query&#39;s uit te voeren tegen AEM en de avontuurlijke inhoud in de app te laden.

Elke persistente query heeft een corresponderende &quot;loader&quot;-klasse, die asynchroon het eindpunt van de AEM HTTP-GET aanroept en de avontuurgegevens retourneert met behulp van de aangepaste definitie [gegevensmodel](#data-models).

+ `loader/AdventuresLoader.java`

  Hiermee wordt de lijst met avonturen opgehaald op het beginscherm van de toepassing met de functie `wknd-shared/adventures-all` voortgezette query.

+ `loader/AdventureLoader.java`

  Hiermee wordt één avontuur opgehaald en geselecteerd via de `slug` parameter, gebruiken `wknd-shared/adventure-by-slug` voortgezette query.

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

`Adventure.java` is een Java POJO die wordt geïnitialiseerd met de JSON-gegevens van de GraphQL-aanvraag en een avontuur modelleert voor gebruik in de weergaven van de Android-toepassing.

### Weergaven

De Android-toepassing gebruikt twee weergaven om de avontuurgegevens in de mobiele ervaring te presenteren.

+ `AdventureListFragment.java`

  Roept de `AdventuresLoader` en geeft de geretourneerde avonturen weer in een lijst.

+ `AdventureDetailFragment.java`

  Roept de `AdventureLoader` met de `slug` param werd via de avontuurselectie op de `AdventureListFragment` bekijken, en toont de details van één enkel avontuur.

### Externe afbeeldingen

`loader/RemoteImagesCache.java` is een hulpprogrammaklasse waarmee externe afbeeldingen in een cache kunnen worden voorbereid, zodat deze kunnen worden gebruikt met Android-UI-elementen. De avontuurinhoud verwijst naar afbeeldingen in AEM Assets via een URL en deze klasse wordt gebruikt om die inhoud weer te geven.

## Aanvullende bronnen

+ [Aan de slag met AEM headless - zelfstudie voor GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [AEM headless-client voor Java](https://github.com/adobe/aem-headless-client-java)
