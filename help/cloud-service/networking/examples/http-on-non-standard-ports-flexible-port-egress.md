---
title: HTTP/HTTPS-verbindingen op niet-standaardpoorten voor flexibel poortcontact
description: Leer hoe u HTTP/HTTPS-aanvragen kunt maken van AEM as a Cloud Service aan externe webservices die op niet-standaard poorten voor Flexible Port Egress worden uitgevoerd.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# HTTP/HTTPS-verbindingen op niet-standaardpoorten voor flexibel poortcontact

HTTP/HTTPS-verbindingen op niet-standaardpoorten (niet 80/443) moeten worden uitgebreid uit AEM as a Cloud Service, maar hebben geen speciale `portForwards` regels, en kan AEM geavanceerde voorzien van een netwerkengebruik `AEM_PROXY_HOST` en een gereserveerde proxypoort `AEM_HTTP_PROXY_HOST` of `AEM_HTTPS_PROXY_HOST` afhankelijk van is de bestemming HTTP/HTTPS is.

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

Zorg ervoor dat de [passend](../advanced-networking.md#advanced-networking) de geavanceerde voorzien van een netwerkconfiguratie is opstelling voorafgaand aan het volgen van dit leerprogramma.

| Geen geavanceerde netwerken | [Flexibele poortuitgang](../flexible-port-egress.md) | [IP-adres van specifiek egress](../dedicated-egress-ip-address.md) | [Virtueel privé netwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Dit codevoorbeeld is alleen bedoeld voor [Flexibele poortuitgang](../flexible-port-egress.md). Een vergelijkbaar, maar ander codevoorbeeld is beschikbaar voor [HTTP/HTTPS-verbindingen op niet-standaardpoorten voor specifiek IP-adres en VPN](./http-on-non-standard-ports.md).

## Codevoorbeeld

Dit Java™ codevoorbeeld is van de dienst OSGi die in AEM as a Cloud Service kan lopen die een verbinding van HTTP met een externe Webserver op 8080 maakt. Verbindingen met HTTPS-webservers gebruiken de omgevingsvariabelen `AEM_PROXY_HOST` en `AEM_HTTPS_PROXY_PORT` (standaard `proxy.tunnel:3128` in AEM releases &lt; 6094).

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
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_HOST") 
        // or System.getenv("AEM_HTTPS_PROXY_HOST"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
            // The explicit fallback of 3128 will be obsoleted in Jan 2022, and only the AEM_HTTP_PROXY_PORT/AEM_HTTPS_PROXY_PORT variable will be required
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", "3128"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
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
