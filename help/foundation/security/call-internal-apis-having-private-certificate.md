---
title: Interne API's met persoonlijke certificaten aanroepen
description: Leer hoe u interne API's met persoonlijke of zelfondertekende certificaten aanroept.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Interne API&#39;s met persoonlijke certificaten aanroepen

Leer hoe te om vraag HTTPS van AEM aan Web APIs te maken gebruikend privé of zelf-ondertekende certificaten.

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

Wanneer u probeert een HTTPS-verbinding te maken met een web-API die een zelfondertekend certificaat gebruikt, mislukt de verbinding standaard met de fout:

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

Dit probleem doet zich doorgaans voor wanneer de **Het SSL-certificaat van de API wordt niet uitgegeven door een erkende certificeringsinstantie (CA)** en Java™-toepassing kan het SSL/TLS-certificaat niet valideren.

Leer hoe te met succes APIs roepen die privé of zelf-ondertekende certificaten door te gebruiken hebben [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) en **AEM global TrustStore**.


## Prototypische API-aanroepcode met gebruik van HttpClient

De volgende code maakt een HTTPS-verbinding met een web-API:

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

De code gebruikt de [Apache HttpComponent](https://hc.apache.org/)s [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) bibliotheekklassen en de bijbehorende methoden.


## HttpClient en load AEM TrustStore-materiaal

Om een API eindpunt te roepen dat heeft _persoonlijk of zelfondertekend certificaat_ de [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)s `SSLContextBuilder` moet worden geladen met AEM TrustStore en worden gebruikt om de verbinding te vergemakkelijken.

Voer de volgende stappen uit:

1. Aanmelden bij **AEM auteur** als **beheerder**.
1. Navigeren naar **AEM Auteur > Gereedschappen > Beveiliging > Betrouwbaarheidswinkel** en opent u de **Global Trust Store**. Als u voor het eerst een account opent, stelt u een wachtwoord in voor de Global Trust Store.

   ![Global Trust Store](assets/internal-api-call/global-trust-store.png)

1. Als u een privé-certificaat wilt importeren, klikt u op **Certificaatbestand selecteren** en selecteer het gewenste certificaatbestand met `.cer` extensie. Importeren door erop te klikken **Verzenden** knop.

1. Java™-code bijwerken zoals hieronder. Let op: gebruik `@Reference` AEM `KeyStoreService` de roepende code moet een component OSGi/dienst, of een Model van de Schelling zijn (en `@OsgiService` wordt daar gebruikt).

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * Injecteer de OOTB `com.adobe.granite.keystore.KeyStoreService` De dienst OSGi in uw component OSGi.
   * De algemene AEM TrustStore ophalen met `KeyStoreService` en `ResourceResolver`de `getAEMTrustStore(...)` methode doet dat.
   * Een object maken van `SSLContextBuilder`, zie Java™ [API-details](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
   * Laad de algemene AEM TrustStore in `SSLContextBuilder` gebruiken `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)` methode.
   * Voldoende `null` for `TrustStrategy` in bovenstaande methode zorgt deze ervoor dat alleen AEM vertrouwde certificaten slagen tijdens het uitvoeren van de API.


>[!CAUTION]
>
>API-aanroepen met geldige certificaten die door CA zijn uitgegeven, mislukken wanneer ze worden uitgevoerd met de vermelde aanpak. Alleen API-aanroepen met AEM vertrouwde certificaten zijn toegestaan wanneer deze methode wordt gevolgd.
>
>Gebruik de [standaardbenadering](#prototypical-api-invocation-code-using-httpclient) voor het uitvoeren van API-aanroepen van geldige certificaten die door CA zijn uitgegeven, wat betekent dat alleen API&#39;s die zijn gekoppeld aan persoonlijke certificaten moeten worden uitgevoerd met behulp van de eerder genoemde methode.

## Wijzigingen in JVM-sleutelarchief vermijden

Een conventionele benadering om interne APIs met privé certificaten effectief aan te halen impliceert het wijzigen van JVM Keystore. Dit wordt bereikt door de persoonlijke certificaten te importeren met behulp van de Java™ [sleutelgereedschap](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) gebruiken.

Deze methode is echter niet afgestemd op best practices op het gebied van beveiliging en AEM biedt een superieure optie via het gebruik van de **Global Trust Store** en [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).


## Oplossingspakket

Het voorbeeldproject Node.js dat in de video is gedemodeerd, kan worden gedownload van [hier](assets/internal-api-call/REST-APIs.zip).

De AEM servlet-code is beschikbaar in de projecten van het WKND-project `tutorial/web-api-invocation` vertakking, [zie](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
