---
title: Toegangstoken genereren
description: Voorbeeldcode voor het genereren van een toegangstoken om de Forms Document Services API aan te roepen
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
source-wordcount: '180'
ht-degree: 0%

---

# Toegangstoken genereren

Een toegangstoken is een referentie die wordt gebruikt om verzoeken aan een REST API voor authentiek te verklaren en toe te laten. Deze wordt doorgaans uitgegeven door een verificatieserver (zoals OAuth 2.0 of OpenID Connect) nadat een gebruiker of toepassing zich met succes heeft aangemeld. Wanneer een beveiligde Forms Document Services-API wordt aangeroepen, wordt een toegangstoken opgenomen in de aanvraagheader om de identiteit van de client te verifiëren.
Hier volgt een voorbeeld van een verzoek om toegangstoken te verzenden

```java
POST /api/data HTTP/1.1
Host: example.com
Authorization: Bearer eyJhbGciOiJIUzI1...
```

De volgende code werd gebruikt om het toegangstoken te produceren

```java
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class AccessTokenService {
    private static final Logger logger = LoggerFactory.getLogger(AccessTokenService.class);
    
    public String getAccessToken() {
        ClassLoader classLoader = AccessTokenService.class.getClassLoader();

        try (InputStream inputStream = classLoader.getResourceAsStream("credentials/server_credentials.json")) {
            if (inputStream == null) {
                logger.error("File not found: credentials/server_credentials.json");
                throw new IllegalArgumentException("Missing credentials file");
            }

            try (InputStreamReader reader = new InputStreamReader(inputStream, StandardCharsets.UTF_8);
                 CloseableHttpClient httpClient = HttpClients.createDefault()) {
                 
                JsonObject jsonObject = JsonParser.parseReader(reader).getAsJsonObject();
                String clientId = jsonObject.get("clientId").getAsString();
                String clientSecret = jsonObject.get("clientSecret").getAsString();
                String adobeIMSV3TokenEndpointURL = jsonObject.get("adobeIMSV3TokenEndpointURL").getAsString();
                String scopes = jsonObject.get("scopes").getAsString();

                HttpPost request = new HttpPost(adobeIMSV3TokenEndpointURL);
                request.setHeader("Content-Type", "application/x-www-form-urlencoded");

                String requestBody = "grant_type=client_credentials&client_id=" + clientId +
                        "&client_secret=" + clientSecret +
                        "&scope=" + scopes;

                request.setEntity(new StringEntity(requestBody, ContentType.APPLICATION_FORM_URLENCODED));

                logger.info("Requesting access token from {}", adobeIMSV3TokenEndpointURL);

                try (CloseableHttpResponse response = httpClient.execute(request)) {
                    String responseString = EntityUtils.toString(response.getEntity());
                    JsonObject jsonResponse = JsonParser.parseString(responseString).getAsJsonObject();
                    
                    if (!jsonResponse.has("access_token")) {
                        logger.error("Access token not found in response: {}", responseString);
                        return null;
                    }

                    String accessToken = jsonResponse.get("access_token").getAsString();
                    logger.info("Successfully obtained access token.");
                    return accessToken;
                }
            }
        } catch (Exception e) {
            logger.error("Error fetching access token", e);
        }
        return null;
    }
}
```

De klasse AccessTokenService is verantwoordelijk voor het ophalen van een OAuth-toegangstoken van de Adobe IMS-verificatieservice. Het leest cliëntgeloofsbrieven van een JSON dossier (server_credentials.json), bouwt een authentificatieverzoek, en verzendt het naar het symbolische eindpunt van Adobe. De reactie wordt dan ontleed om het toegangstoken te halen, dat voor veilige API vraag wordt gebruikt. De klasse omvat correct registreren en fout behandeling om betrouwbaarheid en gemakkelijkere het zuiveren te verzekeren.

## Volgende stappen

[API-aanroepen maken](./make-api-calls.md)