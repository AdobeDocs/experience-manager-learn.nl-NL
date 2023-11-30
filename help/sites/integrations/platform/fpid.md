---
title: Adobe Experience Platform FPID's genereren met AEM Sites
description: Leer hoe u Adobe Experience Platform FPID-cookies kunt genereren of vernieuwen met AEM Sites.
version: Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2022-10-20T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1040'
ht-degree: 0%

---

# Experience Platform-FPID&#39;s genereren met AEM Sites

Voor het integreren van Adobe Experience Manager (AEM) Sites met Adobe Experience Platform (AEP) moet AEM een unieke FPID-cookie (FPID) van de eerste partij genereren en onderhouden om de gebruikersactiviteit op unieke wijze te kunnen volgen.

Lees de ondersteunende documentatie voor [Meer informatie over de manier waarop ID&#39;s van apparaten en Experience Cloud-id&#39;s in eerste instantie samenwerken](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=en).

Hieronder vindt u een overzicht van de werking van FPID&#39;s wanneer u AEM als webhost gebruikt.

![FPID en ECID&#39;s met AEM](./assets/aem-platform-fpid-architecture.png)

## De FPID genereren en behouden met AEM

AEM de publicatieservice optimaliseert prestaties door verzoeken in het voorgeheugen onder te brengen zo veel als mogelijk is, in zowel de CDN- als AEM Dispatcher-cache.

Het is dwingende HTTP- verzoeken die het uniek-per-gebruiker FPID koekje produceren en de waarde terugkeren FPID wordt nooit in het voorgeheugen ondergebracht, en direct gediend van AEM Publish die logica kan uitvoeren om uniciteit te waarborgen.

Vermijd het genereren van het FPID-cookie op aanvragen voor webpagina&#39;s of andere cacheable resources, aangezien de combinatie van de unieke FPID-vereisten deze bronnen ontoegankelijk zou maken.

In het volgende diagram wordt beschreven hoe AEM de publicatieservice FPID&#39;s beheert.

![FPID- en AEM-stroomdiagram](./assets/aem-fpid-flow.png)

1. Webbrowser vraagt een webpagina aan die wordt gehost door AEM. Het verzoek kan worden betekend in de cache van een kopie van de webpagina van CDN of AEM Dispatcher-cache.
1. Als de webpagina niet kan worden aangeboden vanaf CDN of AEM Dispatcher caches, bereikt de aanvraag AEM de publicatieservice, die de gevraagde webpagina genereert.
1. De webpagina wordt vervolgens teruggestuurd naar de webbrowser en vult de caches in die de aanvraag niet konden uitvoeren. Met AEM verwacht u CDN- en AEM Dispatcher-cache van meer dan 90%.
1. De webpagina bevat JavaScript waarmee een niet-cachebaar asynchroon XHR-verzoek (AJAX) wordt ingediend bij een aangepaste FPID-server in AEM publicatieservice. Omdat dit een uncacheable verzoek (door het is willekeurige vraagparameter en Cachebeheer kopballen) is, wordt het nooit in het voorgeheugen ondergebracht door CDN of AEM Dispatcher, en bereikt altijd AEM de Publish dienst om de reactie te produceren.
1. De aangepaste FPID-server in AEM publicatieservice verwerkt de aanvraag, genereert een nieuwe FPID wanneer er geen bestaande FPID-cookie wordt gevonden, of verlengt de levensduur van bestaande FPID-cookie. De servlet retourneert ook de FPID in de hoofdtekst van de reactie voor gebruik door JavaScript op de client. Gelukkig is de aangepaste FPID servlet-logica lichtgewicht, waardoor deze aanvraag geen invloed heeft op AEM serviceprestaties voor publiceren.
1. De reactie voor het XHR- verzoek keert aan browser met het koekje FPID en FPID als JSON in het antwoordlichaam voor gebruik door het Web SDK van het Platform terug.

## Codevoorbeeld

De volgende code en de configuratie kunnen aan AEM de Publish dienst worden opgesteld om een eindpunt tot stand te brengen dat produceert, of het leven van een bestaand FPID koekje uitbreidt en FPID als JSON terugkeert.

### AEM FPID cookie servlet

Een AEM HTTP-eindpunt moet worden gemaakt om een FPID-cookie te genereren of uit te breiden met een [Sling servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ De servlet is gebonden aan `/bin/aem/fpid` omdat verificatie niet vereist is om toegang te krijgen tot de verificatie. Als de authentificatie wordt vereist, verbind aan een Verschuivend middeltype.
+ De servlet accepteert HTTP-GET-aanvragen. De reactie is gemarkeerd met `Cache-Control: no-store` om caching te verhinderen, maar dit eindpunt zou ook moeten worden gevraagd gebruikend unieke geheim voorgeheugensteunende vraagparameters.

Wanneer een HTTP-aanvraag de servlet bereikt, controleert de servlet of er een FPID-cookie bestaat op de aanvraag:

+ Als er een FPID-cookie bestaat, verlengt u de levensduur van de cookie en int u de waarde ervan om naar de reactie te schrijven.
+ Als een FPID-cookie niet bestaat, genereert u een nieuw FPID-cookie en slaat u de waarde op om naar het antwoord te schrijven.

Vervolgens schrijft de servlet de FPID naar de reactie als een JSON-object in de vorm: `{ fpid: "<FPID VALUE>" }`.

Het is belangrijk dat de FPID aan de client in de hoofdtekst wordt doorgegeven, aangezien het FPID-cookie is gemarkeerd `HttpOnly`, wat betekent dat alleen de server de waarde ervan kan lezen en JavaScript op de client niet.

De waarde FPID van het reactielichaam wordt gebruikt om vraag te bepalen gebruikend het Web SDK van het Platform.

Hieronder ziet u de voorbeeldcode van een AEM servlet-eindpunt (beschikbaar via `HTTP GET /bin/aep/fpid`) die een FPID-cookie genereert of vernieuwt en de FPID retourneert als JSON.

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13;
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, Create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists. get its FPID UUID so it's life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the newly generate FPID value, or the extended FPID value to the response
        // Use addHeader(..), as we need to set SameSite=Lax (and addCoookie(..) does not support this)
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");
        
        // Avoid caching the response in any cache
        response.addHeader("Cache-Control", "no-store");

        // Since the FPID is HttpOnly, JavaScript cannot read it (only the server can)
        // Write the FPID to the response as JSON so client JavaScript can access it.
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);
        
        // The JSON `{ fpid: "11111111-2222-3333-4444-55555555" }` is returned in the response
        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}
```

### HTML-script

Er moet een aangepaste JavaScript aan de clientzijde aan de pagina worden toegevoegd om de servlet asynchroon aan te roepen, het FPID-cookie te genereren of te vernieuwen en de FPID in de reactie te retourneren.

Dit JavaScript-script wordt standaard toegevoegd aan de pagina met een van de volgende methoden:

+ [Tags in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [AEM clientbibliotheek](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=en)

De XHR-aanroep naar de aangepaste AEM FPID-server is snel, maar asynchroon, zodat een gebruiker een webpagina kan bezoeken die wordt bediend door AEM en kan wegnavigeren voordat het verzoek kan worden voltooid.
Als dit gebeurt, wordt hetzelfde proces opnieuw gestart bij het laden van de volgende pagina van een webpagina vanuit AEM.

De HTTP-GET naar de AEM FPID-server (`/bin/aep/fpid`) wordt bepaald met een willekeurige vraagparameter om ervoor te zorgen dat om het even welke infrastructuur tussen browser en AEM de Publish dienst niet de reactie van het verzoek in het voorgeheugen onderbrengt.
Op dezelfde manier `Cache-Control: no-store` De aanvraagkoptekst wordt toegevoegd ter ondersteuning van het voorkomen van caching.

Bij een aanroep van de AEM FPID-servlet wordt de FPID opgehaald uit de JSON-reactie en gebruikt door de [Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=en) om het naar Experience Platform APIs te verzenden.

Raadpleeg de documentatie bij het Experience Platform voor meer informatie over [FPID&#39;s gebruiken in identityMap](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html#identityMap)

```javascript
...
<script>
    // Invoke the AEM FPID servlet, and then do something with the response

    fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, { 
            method: 'GET',
            headers: {
                'Cache-Control': 'no-store'
            }
        })
        .then((response) => response.json())
        .then((data) => { 
            // Get the FPID from JSON returned by AEM's FPID servlet
            console.log('My FPID is: ' + data.fpid);

            // Send the `data.fpid` to Experience Platform APIs            
        });
</script>
```

### Dispatcher allow, filter

Ten slotte moeten HTTP-aanvragen bij de aangepaste FPID-server via AEM Dispatcher zijn toegestaan `filter.any` configuratie.

Als deze Dispatcher-configuratie niet correct is geïmplementeerd, vraagt de HTTP-GET om `/bin/aep/fpid` resulteert in een waarde 404.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Bronnen voor Experience Platform

Herzie de volgende documentatie van het Experience Platform voor First-party apparaat IDs (FPIDs) en het beheren van identiteitsgegevens met het Web SDK van het Platform.

+ [Apparaat-id&#39;s van de eerste fabrikant genereren](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html)
+ [Eerste partij apparaat IDs in het Web SDK van het Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html)
+ [Identiteitsgegevens in het Web SDK van het Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html)
