---
title: Interne API's met persoonlijke certificaten aanroepen
description: Leer hoe u interne API's met persoonlijke of zelfondertekende certificaten aanroept.
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
duration: 332
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 0%

---

# Interne API&#39;s met persoonlijke certificaten aanroepen

Leer hoe u HTTPS-aanroepen vanuit AEM naar web-API&#39;s kunt maken met behulp van persoonlijke of zelfondertekende certificaten.

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

Wanneer u probeert een HTTPS-verbinding te maken met een web-API die een zelfondertekend certificaat gebruikt, mislukt de verbinding standaard met de fout:

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

Deze kwestie komt typisch voor wanneer het **SSL van API certificaat niet door een erkende certificaatgezag (CA)** wordt uitgegeven en de toepassing Java™ kan SSL/TLS certificaat niet bevestigen.

Leer hoe te met succes APIs roepen die privé of zelf-ondertekende certificaten door [ Apache HttpClient ](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) en **AEM globale TrustStore** te gebruiken hebben.


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

De code gebruikt [ Apache HttpComponent ](https://hc.apache.org/) [ HttpClient ](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) bibliotheekklassen en hun methodes.


## HttpClient en laad AEM TrustStore-materiaal

Om een API eindpunt te roepen dat _privé of zelf-ondertekend certificaat_ heeft, [ HttpClient ](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) `SSLContextBuilder` moet met AEM TrustStore worden geladen, en worden gebruikt om de verbinding te vergemakkelijken.

Voer de volgende stappen uit:

1. Login aan **de Auteur van AEM** als **beheerder**.
1. Navigeer aan **de Auteur van AEM > Hulpmiddelen > Veiligheid > de Opslag van het Vertrouwen**, en open de **Globale Opslag van het Vertrouwen**. Als u voor het eerst een account opent, stelt u een wachtwoord in voor de Global Trust Store.

   ![ Globale opslag van het Vertrouwen ](assets/internal-api-call/global-trust-store.png)

1. Om een privé certificaat in te voeren, klik **Uitgezochte de knoop van het Dossier van het Certificaat** en selecteer gewenst certificaatdossier met `.cer` uitbreiding. Importeer het door te klikken op **Verzenden** knop.

1. Java™-code bijwerken zoals hieronder. Als u `@Reference` wilt gebruiken om AEM `KeyStoreService` op te halen, moet de aanroepende code een OSGi-component/service zijn of een Sling Model (en wordt `@OsgiService` daar gebruikt).

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

   * Injecteer de OOTB `com.adobe.granite.keystore.KeyStoreService` OSGi-service in uw OSGi-component.
   * Haal de algemene AEM TrustStore op met `KeyStoreService` en `ResourceResolver` , de methode `getAEMTrustStore(...)` doet dat.
   * Creeer een voorwerp van `SSLContextBuilder`, zie Java™ [ API details ](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
   * Laad de algemene AEM TrustStore in `SSLContextBuilder` met de methode `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)` .
   * Geef `null` door voor `TrustStrategy` in de bovenstaande methode, zodat alleen de door AEM vertrouwde certificaten slagen tijdens de uitvoering van de API.


>[!CAUTION]
>
>API-aanroepen met geldige certificaten die door CA zijn uitgegeven, mislukken wanneer ze worden uitgevoerd met de vermelde aanpak. Alleen API-aanroepen met vertrouwde AEM-certificaten zijn toegestaan wanneer deze methode wordt gevolgd.
>
>Gebruik de [ standaardbenadering ](#prototypical-api-invocation-code-using-httpclient) voor het uitvoeren van API vraag van geldige CA-Uitgegeven certificaten, betekenend dat slechts APIs verbonden aan privé certificaten gebruikend de eerder genoemde methode zou moeten worden uitgevoerd.

## Wijzigingen in JVM-sleutelarchief vermijden

Een conventionele benadering om interne APIs met privé certificaten effectief aan te halen impliceert het wijzigen van JVM Keystore. Het wordt bereikt door de privé certificaten in te voeren gebruikend het Java™ [ keytool ](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) bevel.

Nochtans, wordt deze methode niet gericht op veiligheid beste praktijken en AEM biedt een superieure optie door het gebruik van de **Globale opslag van het Vertrouwen** en [ KeyStoreService ](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html) aan.


## Oplossingspakket

Het steekproefproject Node.js dat in de video wordt gedemoed kan van [ hier ](assets/internal-api-call/REST-APIs.zip) worden gedownload.

De servletcode van AEM is beschikbaar in de 0&rbrace; tak van het Project van Plaatsen WKND &lbrace;, [ zie ](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).`tutorial/web-api-invocation`
