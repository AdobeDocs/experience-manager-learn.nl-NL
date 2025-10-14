---
title: HTTP/HTTPS verbindingen voor specifiek uitgang IP adres en VPN
description: Leer hoe te om HTTP/HTTPS- verzoeken van AEM as a Cloud Service aan externe Webdiensten te maken die voor Dedicated IP van de Eis adres en VPN lopen
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
duration: 70
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# HTTP/HTTPS verbindingen voor specifiek uitgang IP adres en VPN

HTTP/HTTPS-verbindingen worden automatisch vanuit AEM as a Cloud Service met een toegewezen IP-adres of VPN opgewekt en hebben geen speciale `portForwards` -regels nodig.

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

Verzeker het [&#x200B; specifieke IP adres van de uitgang of VPN &#x200B;](../advanced-networking.md#advanced-networking) geavanceerde voorzien van een netwerkconfiguratie opstelling voorafgaand aan het volgen van dit leerprogramma is geweest.

| Geen geavanceerde netwerken | [&#x200B; Flexibele havenuitgang &#x200B;](../flexible-port-egress.md) | [&#x200B; Dedicated egress IP adres &#x200B;](../dedicated-egress-ip-address.md) | [&#x200B; Virtueel Privé Netwerk &#x200B;](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Dit codevoorbeeld is slechts voor [&#x200B; Speciale IP van de Eis adres &#x200B;](../dedicated-egress-ip-address.md) en [&#x200B; VPN &#x200B;](../vpn.md). Een gelijkaardig, maar verschillend codevoorbeeld is beschikbaar voor [&#x200B; verbindingen HTTP/HTTPS op niet-standaardhavens voor de Flexibele Nauw van de Haven &#x200B;](./http-on-non-standard-ports-flexible-port-egress.md).

## Codevoorbeeld

Dit Java™ codevoorbeeld is van de dienst OSGi die in AEM as a Cloud Service kan lopen die een verbinding van HTTP met een externe Webserver op 8080 maakt. De HTTPS (of HTTP) verbindingen worden automatisch verlengd uit AEM as a Cloud Service, en vereisen geen speciale ontwikkeling.

>[!NOTE]
> Het wordt geadviseerd [&#x200B; Java™ 11 HTTP APIs &#x200B;](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) wordt gebruikt om HTTP/HTTPS vraag van AEM te maken.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
