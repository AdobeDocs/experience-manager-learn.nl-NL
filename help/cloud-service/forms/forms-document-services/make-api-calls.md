---
title: De API voor hulpprogramma's gebruiken
description: Voorbeeldcode voor het toepassen van gebruiksrechten op de geleverde PDF
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# API-aanroep maken

## Gebruiksrechten toepassen

Zodra u het toegangstoken hebt, is de volgende stap een API verzoek om gebruiksrechten op de gespecificeerde PDF toe te passen. Dit impliceert het omvatten van het toegangstoken in de verzoekkopbal om de vraag voor authentiek te verklaren, die veilige en erkende verwerking van het document verzekert.

De volgende functie past de gebruiksrechten toe

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## Uitsplitsing functie:



* **Opstelling API Eindpunt &amp; Payload**
   * Hiermee wordt de API-URL gemaakt met behulp van de opgegeven `endPoint` en een vooraf gedefinieerde `BUCKET` .
   * Definieert een JSON-tekenreeks (`usageRights`) die de rechten opgeeft die moeten worden toegepast, zoals:
      * Opmerkingen
      * Ingesloten bestanden
      * Formulier invullen
      * Formuliergegevens exporteren

* **Laad het Dossier van PDF**
   * Hiermee wordt het bestand `withoutusagerights.pdf` opgehaald uit de map `pdffiles` .
   * Logs an error and exit if the file is not found.

* **bereidt het Verzoek van HTTP voor**
   * Leest het PDF-bestand in een bytearray.
   * Gebruikt `MultipartEntityBuilder` om een meerdelige aanvraag te maken die het volgende bevat:
      * Het PDF-bestand als binaire hoofdtekst.
      * De `usageRights` JSON als tekst.
   * Stelt een HTTP `POST` -aanvraag in met headers:
      * `Authorization: Bearer <accessToken>` voor verificatie.
      * `X-Adobe-Accept-Experimental: 1` (mogelijk vereist voor API-compatibiliteit).

* **verzend het Verzoek &amp; de Reactie van de Handvat**
   * Voert de HTTP-aanvraag uit met `httpClient.execute(httpPost)` .
   * Leest het antwoord (dit is naar verwachting de bijgewerkte PDF met toegepaste gebruiksrechten).
   * Schrijft de ontvangen inhoud van PDF aan **&quot;ReaderExtended.pdf&quot;** bij `SAVE_LOCATION`.

* **de Behandeling &amp; Opruiming van de Fout**
   * Vangt en registreert om het even welke `IOException` fouten.
   * Hiermee zorgt u ervoor dat alle bronnen (streams, HTTP-client, response) op de juiste wijze worden gesloten in het `finally` -blok.

Hier volgt de code main.java die de functie applyUsageRights aanroept

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

De methode `main` initialiseert door `getAccessToken()` from `AccessTokenService` aan te roepen, die een geldig token moet retourneren.

* Vervolgens wordt `applyUsageRights()` vanuit de klasse `DocumentGeneration` aangeroepen en wordt het doorgegeven:
   * De opgehaalde `accessToken`
   * Het API-eindpunt voor het toepassen van gebruiksrechten.


## Volgende stappen

[Het voorbeeldproject implementeren](sample-project.md)
