---
title: Android-toepassing - Voorbeeld AEM zonder kop
description: Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Er is een Android-toepassing beschikbaar die laat zien hoe inhoud kan worden opgevraagd met de GraphQL-API's van AEM. De Apollo Client Android wordt gebruikt om de GraphQL-query's te genereren en gegevens toe te wijzen aan SWIFT-objecten om de app te activeren. SwiftUI wordt gebruikt om een eenvoudige lijst en detailmening van de inhoud terug te geven.
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 1%

---


# Android-toepassing

Voorbeeldtoepassingen zijn een geweldige manier om de mogelijkheden zonder kop van Adobe Experience Manager (AEM) te verkennen. Er is een Android-toepassing beschikbaar die laat zien hoe inhoud kan worden opgevraagd met de GraphQL-API&#39;s van AEM. De [AEM headless-client voor Java](https://github.com/adobe/aem-headless-client-java) wordt gebruikt om de query&#39;s voor GraphQL uit te voeren en gegevens toe te wijzen aan Java-objecten om de app te activeren.

De weergave van [broncode op GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## Vereisten {#prerequisites}

De volgende gereedschappen moeten lokaal worden geïnstalleerd:

* [Android Studio](https://developer.android.com/studio)
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

1. Starten [Android Studio](https://developer.android.com/studio) en opent u de map `android-app`
1. Het bestand wijzigen `config.properties` om `app/src/main/assets/config.properties` en bijwerken `contentApi.endpoint` om aan uw doel AEM milieu aan te passen:

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. Download een [Virtueel Android-apparaat](https://developer.android.com/studio/run/managing-avds) (min API 28)
1. Ontwikkel en implementeer de app met de Android-emulator.


### Verbinding maken met AEM omgevingen

`10.0.2.2` is een [speciale alias](https://developer.android.com/studio/run/emulator-networking) voor localhost bij gebruik van de emulator. Dus `10.0.2.2:4502` is gelijk aan `localhost:4502`. Als verbinding wordt gemaakt met een AEM publicatieomgeving (aanbevolen), is geen autorisatie vereist en `contentAPi.user` en `contentApi.password` kan leeg worden gelaten.

Als u verbinding maakt met een AEM-ontwerpomgeving [autorisatie](https://github.com/adobe/aem-headless-client-java#using-authorization) is vereist. Standaard is de toepassing ingesteld op het gebruik van basisverificatie met een gebruikersnaam en wachtwoord van `admin:admin`. De [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) biedt de mogelijkheid [tokenverificatie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). Om op teken-gebaseerde cliënt van de authentificatieupdate binnen te gebruiken `AdventureLoader.java` en `AdventuresLoader.java`:

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

### Inhoud ophalen

De [AEM headless-client voor Java](https://github.com/adobe/aem-headless-client-java) wordt gebruikt door de app om de GraphQL-query uit te voeren op AEM en de avontuurlijke inhoud in de app te laden.

`AdventuresLoader.java` Dit is het bestand dat de initiële lijst met avonturen ophaalt en laadt op het beginscherm van de toepassing. Het maakt gebruik van [Blijvende query&#39;s](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) die [voorverpakt](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) met de WKND-referentiesite. Het eindpunt is `/wknd/adventures-all`. `AEMHeadlessClientBuilder` instantieert een nieuwe instantie op basis van het api-eindpunt ingesteld in `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` is het dossier dat de inhoud van het Avontuur voor elk van de detailmeningen ophaalt en laadt. Opnieuw `AEMHeadlessClient` wordt gebruikt om de query uit te voeren. Een regelmatige graphQL vraag wordt uitgevoerd gebaseerd op de weg aan het de inhoudsfragment van het Avontuur. De query wordt bijgehouden in een afzonderlijk bestand met de naam [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` is POJO die met de gegevens JSON van het GraphQL- verzoek wordt geïnitialiseerd.

`RemoteImagesCache.java` is een hulpprogrammaklasse waarmee externe afbeeldingen in een cache kunnen worden voorbereid, zodat deze kunnen worden gebruikt met Android-UI-elementen. De Adventure-inhoud verwijst naar afbeeldingen in AEM Assets via een URL en deze klasse wordt gebruikt om die inhoud weer te geven.

### Weergaven

`AdventureListFragment.java` - als de aangeroepen functie de `AdventuresLoader` en geeft de geretourneerde avonturen weer in een lijst.

`AdventureDetailFragment.java` - initialiseert `AdventureLoader` en geeft de details van één avontuur weer.

## Aanvullende bronnen

* [Aan de slag met AEM headless - GraphQL-zelfstudie](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [AEM headless-client voor Java](https://github.com/adobe/aem-headless-client-java)

