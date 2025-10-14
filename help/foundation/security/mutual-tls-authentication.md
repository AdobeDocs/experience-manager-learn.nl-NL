---
title: Mutual Transport Layer Security (mTLS)-verificatie van AEM
description: Leer hoe te om vraag HTTPS van AEM aan Web APIs te maken die de Wederzijdse authentificatie van de Veiligheid van de Laag van het Vervoer (mTLS) vereisen.
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-13881
thumbnail: KT-13881.png
doc-type: Article
last-substantial-update: 2023-10-10T00:00:00Z
exl-id: 7238f091-4101-40b5-81d9-87b4d57ccdb2
duration: 495
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# Mutual Transport Layer Security (mTLS)-verificatie van AEM

Leer hoe te om vraag HTTPS van AEM aan Web APIs te maken die de Wederzijdse authentificatie van de Veiligheid van de Laag van het Vervoer (mTLS) vereisen.

>[!VIDEO](https://video.tv.adobe.com/v/3447865?quality=12&learn=on&captions=dut)

De mTLS of bidirectionele authentificatie van TLS verbetert de veiligheid van het protocol TLS door **zowel de cliënt als de server te vereisen om elkaar** voor authentiek te verklaren. Voor deze verificatie worden digitale certificaten gebruikt. Het wordt algemeen gebruikt in scenario&#39;s waar de sterke veiligheid en identiteitscontrole kritiek zijn.

Wanneer u probeert een HTTPS-verbinding te maken met een web-API waarvoor mTLS-verificatie is vereist, mislukt de verbinding standaard met de fout:

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

Dit probleem doet zich voor wanneer de client geen certificaat voor verificatie presenteert.

Leer hoe te met succes APIs roepen die mTLS authentificatie door [&#x200B; Apache HttpClient &#x200B;](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) en **AEM KeyStore en TrustStore** vereisen te gebruiken.


## HttpClient en laad AEM KeyStore-materiaal

Op hoog niveau zijn de volgende stappen vereist om een met mTLS beveiligde API vanuit AEM aan te roepen.

### AEM-certificaatgeneratie

Vraag het AEM-certificaat aan door samen te werken met het beveiligingsteam van uw organisatie. Het beveiligingsteam geeft of vraagt de certificaatgerelateerde details, zoals sleutel, CSR (Certificate signing Request) en het gebruik van CSR, als het certificaat is uitgegeven.

Voor demo-doeleinden genereert u de certificaatgerelateerde details, zoals sleutel, CSR (Certificate Signing Request). In het onderstaande voorbeeld wordt een zelfondertekende CA gebruikt om het certificaat uit te geven.

- Eerst genereert u het certificaat van de interne certificeringsinstantie (CA).

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- Genereer het AEM-certificaat.

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- Converteer AEM Private Key naar DER-indeling. AEM KeyStore vereist de persoonlijke sleutel in DER-indeling.

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>De zelfondertekende CA-certificaten worden alleen gebruikt voor ontwikkelingsdoeleinden. Voor productie, gebruik een vertrouwde op Instantie van het Certificaat (CA) om het certificaat uit te geven.


### Certificaatuitwisseling

Als u een zelfondertekende CA voor het AEM-certificaat gebruikt, zoals hierboven, verzendt u het certificaat of het certificaat van de interne certificeringsinstantie (CA) naar de API-provider.

Als de API-provider een zelfondertekend CA-certificaat gebruikt, ontvangt u het certificaat of het certificaat van de interne certificeringsinstantie (CA) van de API-provider.

### Certificaat importeren

Voer de volgende stappen uit om een AEM-certificaat te importeren:

1. Login aan **de Auteur van AEM** als **beheerder**.

1. Navigeer aan **Auteur van AEM > Hulpmiddelen > Veiligheid > Gebruikers > creëren of selecteren een bestaande gebruiker**.

   ![&#x200B; creeer of selecteer een bestaande gebruiker &#x200B;](assets/mutual-tls-authentication/create-or-select-user.png)

   Voor demo-doeleinden wordt een nieuwe gebruiker met de naam `mtl-demo-user` gemaakt.

1. Om de **Eigenschappen van de Gebruiker** te openen, klik de gebruikersnaam.

1. Klik **Keystore** lusje en klik dan **creëren Keystore** knoop. Dan in de **Vastgestelde dialoog van het Wachtwoord van de Toegang KeyStore**, plaats een wachtwoord voor keystore van deze gebruiker en klik sparen.

   ![&#x200B; creeer Keystore &#x200B;](assets/mutual-tls-authentication/create-keystore.png)

1. In het nieuwe scherm, onder **VOEG PRIVATE SLEUTEL VAN DER DOSSIER** sectie TOE, volg de hieronder stappen:

   1. Alias invoeren

   1. Importeer de bovenstaande AEM Private Key in de indeling DER.

   1. Importeer de bovenstaande certificaatkettingbestanden.

   1. Klik op Verzenden

      ![&#x200B; de Privé Sleutel van AEM van de Invoer &lbrace;](assets/mutual-tls-authentication/import-aem-private-key.png)

1. Controleer of het certificaat is geïmporteerd.

   ![&#x200B; Geïmporteerde de Privé Sleutel en Certificaat van AEM &#x200B;](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

Als de API leverancier een zelfondertekend certificaat van CA gebruikt, voer het ontvangen certificaat in AEM TrustStore in, volg de stappen van [&#x200B; hier &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html?lang=nl-NL#httpclient-and-load-aem-truststore-material).

En als AEM een zelfondertekend CA-certificaat gebruikt, vraagt u de API-provider om dit te importeren.

### Prototypische mTLS API-aanroepcode met gebruik van HttpClient

Java™-code bijwerken zoals hieronder. Als u `@Reference` -annotatie wilt gebruiken om AEM `KeyStoreService` -service te krijgen, moet de aanroepende code een OSGi-component/service zijn of een Sling Model (en wordt `@OsgiService` daar gebruikt).


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
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
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
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

- Injecteer de OOTB `com.adobe.granite.keystore.KeyStoreService` OSGi-service in uw OSGi-component.
- Haal de AEM KeyStore van de gebruiker op met `KeyStoreService` en `ResourceResolver` , de methode `getAEMKeyStore(...)` doet dat.
- Als de API-provider een zelfondertekend CA-certificaat gebruikt, haalt u de algemene AEM TrustStore op, de methode `getAEMTrustStore(...)` doet dat.
- Creeer een voorwerp van `SSLContextBuilder`, zie Java™ [&#x200B; API details &#x200B;](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
- Laad de AEM KeyStore van de gebruiker in `SSLContextBuilder` gebruikend `loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)` methode.
- Het keystore wachtwoord is het wachtwoord dat toen het creëren van keystore werd geplaatst, zou het in OSGi config moeten worden opgeslagen, zie {de Waarden van de Configuratie 0} Geheime [&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=nl-NL#secret-configuration-values).

## Wijzigingen in JVM-sleutelarchief vermijden

Een conventionele benadering om mTLS APIs met privé certificaten effectief aan te halen impliceert het wijzigen van JVM Keystore. Het wordt bereikt door de privé certificaten in te voeren gebruikend het Java™ [&#x200B; keytool &#x200B;](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) bevel.

Nochtans, wordt deze methode niet gericht op veiligheid beste praktijken en AEM biedt een superieure optie door het gebruik van **User-specific KeyStores en Global TrustStore** en [&#x200B; KeyStoreService &#x200B;](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html) aan.

## Oplossingspakket

Het steekproefproject Node.js dat in de video wordt gedemoed kan van [&#x200B; hier &#x200B;](assets/internal-api-call/REST-APIs.zip) worden gedownload.

De servletcode van AEM is beschikbaar in de 0&rbrace; tak van het Project van Plaatsen WKND &lbrace;, [&#x200B; zie &#x200B;](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).`tutorial/web-api-invocation`
