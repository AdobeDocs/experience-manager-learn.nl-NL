---
title: HTTP/HTTPS verbindingen voor specifiek uitgang IP adres en VPN
description: Leer hoe te om HTTP/HTTPS- verzoeken van AEM as a Cloud Service aan externe Webdiensten te maken die voor Dedicated IP van de Eis adres en VPN lopen
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: aa2d0d4d6e0eb429baa37378907a9dd53edd837d
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# HTTP/HTTPS verbindingen voor specifiek uitgang IP adres en VPN

HTTP/HTTPS-verbindingen moeten worden uitgebreid uit AEM as a Cloud Service, maar ze hebben geen speciale `portForwards` regels, en kan AEM geavanceerde voorzien van een netwerkengebruik `AEM_HTTP_PROXY_HOST`, `AEM_HTTP_PROXY_PORT`, `AEM_HTTPS_PROXY_HOST`, en `AEM_HTTPS_PROXY_PORT`.

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

Zorg ervoor dat de [passend](../advanced-networking.md#advanced-networking) de geavanceerde voorzien van een netwerkconfiguratie is opstelling voorafgaand aan het volgen van dit leerprogramma.

| Geen geavanceerde netwerken | [Flexibele poortuitgang](../flexible-port-egress.md) | [IP-adres van specifiek egress](../dedicated-egress-ip-address.md) | [Virtueel privé netwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Dit codevoorbeeld is alleen bedoeld voor [IP-adres van speciale egress](../dedicated-egress-ip-address.md) en [VPN](../vpn.md). Een vergelijkbaar, maar ander codevoorbeeld is beschikbaar voor [HTTP/HTTPS-verbindingen op niet-standaard poorten voor Flexible Port Egress](./http-on-non-standard-ports-flexible-port-egress.md).

## Codevoorbeeld

Dit Java™ codevoorbeeld is van de dienst OSGi die in AEM as a Cloud Service kan lopen die een verbinding van HTTP met een externe Webserver op 8080 maakt. Verbindingen met HTTPS-webservers gebruiken de `AEM_HTTPS_PROXY_HOST` en `AEM_HTTPS_PROXY_PORT` in plaats van  `AEM_HTTP_PROXY_HOST` en `AEM_HTTP_PROXY_PORT`.

>[!NOTE]
> Het wordt aanbevolen [Java™ 11 HTTP API&#39;s](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) worden gebruikt om HTTP/HTTPS-aanroepen vanuit AEM te maken.

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
