---
title: HTTP/HTTPS-verbindingen op niet-standaardpoorten voor flexibel poortcontact
description: Leer hoe u HTTP/HTTPS-aanvragen van AEM as a Cloud Service naar externe webservices kunt uitvoeren op niet-standaard poorten voor Flexible Port Egress.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
duration: 86
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 0%

---

# HTTP/HTTPS-verbindingen op niet-standaardpoorten voor flexibel poortcontact

HTTP/HTTPS-verbindingen op niet-standaardpoorten (niet 80/443) moeten vanuit AEM as a Cloud Service worden vernieuwd, maar ze hebben geen speciale `portForwards` -regels nodig en kunnen gebruik maken van de geavanceerde AEM-netwerkvoorzieningen `AEM_PROXY_HOST` en een gereserveerde proxypoort `AEM_HTTP_PROXY_PORT` of `AEM_HTTPS_PROXY_PORT` , afhankelijk van de bestemming is HTTP/HTTPS.

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

Verzeker [ aangewezen ](../advanced-networking.md#advanced-networking) geavanceerde voorzien van een netwerkconfiguratie voorafgaand aan het volgen van dit leerprogramma is opstelling.

| Geen geavanceerde netwerken | [ Flexibele havenuitgang ](../flexible-port-egress.md) | [ Dedicated egress IP adres ](../dedicated-egress-ip-address.md) | [ Virtueel Privé Netwerk ](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Dit codevoorbeeld is slechts voor [ de Flexibele Groepen van de Haven ](../flexible-port-egress.md). Een gelijkaardig, maar verschillend codevoorbeeld is beschikbaar voor [ verbindingen HTTP/HTTPS op niet-standaardhavens voor het Specifieke IP van de Gastheer adres en VPN ](./http-dedicated-egress-ip-vpn.md).

## Codevoorbeeld

Dit Java™ codevoorbeeld is van de dienst OSGi die in AEM as a Cloud Service kan lopen die een verbinding van HTTP met een externe Webserver op 8080 maakt. Verbindingen met HTTPS-webservers gebruiken de omgevingsvariabelen `AEM_PROXY_HOST` en `AEM_HTTPS_PROXY_PORT` (standaard `proxy.tunnel:3128` in AEM-releases &lt; 6094).

>[!NOTE]
> Het wordt geadviseerd [ Java™ 11 HTTP APIs ](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) wordt gebruikt om HTTP/HTTPS vraag van AEM te maken.

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

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

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
