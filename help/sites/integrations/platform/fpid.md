---
title: Adobe Experience Platform FPID's genereren met AEM Sites
description: Leer hoe u Adobe Experience Platform FPID-cookies kunt genereren of vernieuwen met AEM Sites.
version: Experience Manager as a Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2024-10-09T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
duration: 266
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1054'
ht-degree: 0%

---

# Experience Platform FPID&#39;s genereren met AEM Sites

Als u Adobe Experience Manager (AEM)-sites die via AEM Publish worden geleverd, integreert met Adobe Experience Platform (AEP), moet AEM een unieke FPID-cookie (FPID) van de eerste partij genereren en onderhouden om de gebruikersactiviteit op unieke wijze bij te houden.

Het FPID-cookie moet worden ingesteld door de server (AEM Publish) in plaats van JavaScript te gebruiken om een client-side cookie te maken. Dit komt omdat moderne browsers, zoals Safari en Firefox, cookies die door JavaScript zijn gegenereerd, kunnen blokkeren of snel verlopen.

Lees de ondersteunende documentatie aan [ leren over de details van hoe eerste-deel apparaat IDs en Experience Cloud IDs samenwerken ](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=nl-NL).

Hieronder vindt u een overzicht van de werking van FPID&#39;s wanneer u AEM als webhost gebruikt.

![ FPID en ECIDs met AEM ](./assets/aem-platform-fpid-architecture.png)

## De FPID genereren en behouden met AEM

De AEM-publicatieservice optimaliseert de prestaties door de aanvragen zoveel mogelijk in het cachegeheugen op te slaan, zowel in de CDN-cache als in de AEM Dispatcher-cache.

Het is absoluut noodzakelijk dat HTTP-verzoeken die het unieke-per-gebruiker FPID-cookie genereren en de FPID-waarde retourneren, nooit in de cache worden geplaatst en rechtstreeks worden verzonden vanuit AEM Publish dat logica kan implementeren om uniciteit te garanderen.

Vermijd het genereren van het FPID-cookie op aanvragen voor webpagina&#39;s of andere cacheable resources, aangezien de combinatie van de unieke FPID-vereisten deze bronnen ontoegankelijk zou maken.

In het volgende diagram wordt beschreven hoe de AEM-publicatieservice FPID&#39;s beheert.

![ FPID en AEM stroomdiagram ](./assets/aem-fpid-flow.png)

1. Webbrowser vraagt een webpagina aan die wordt gehost door AEM. Het verzoek kan worden betekend of meegedeeld met behulp van een kopie van de webpagina in de cache van CDN of AEM Dispatcher.
1. Als de webpagina niet kan worden aangeboden vanuit CDN- of AEM Dispatcher-caches, bereikt de aanvraag de AEM Publish-service, die de gevraagde webpagina genereert.
1. De webpagina wordt vervolgens teruggestuurd naar de webbrowser en vult de caches in die de aanvraag niet konden uitvoeren. Verwacht dat CDN- en AEM Dispatcher-cachesnelheden groter zijn dan 90% bij AEM.
1. De webpagina bevat JavaScript dat een ongeoorloofd asynchroon XHR-verzoek (AJAX) indient bij een aangepaste FPID-server in de AEM-publicatieservice. Omdat dit een uncacheable verzoek (door het is willekeurige vraagparameter en Cachebeheer kopballen) is, wordt het nooit in het voorgeheugen ondergebracht door CDN of AEM Dispatcher, en bereikt altijd de dienst van de Publicatie van AEM om de reactie te produceren.
1. De aangepaste FPID-server in de AEM-publicatieservice verwerkt de aanvraag, genereert een nieuwe FPID wanneer er geen bestaande FPID-cookie wordt gevonden, of verlengt de levensduur van bestaande FPID-cookie. De servlet retourneert ook de FPID in de antwoordinstantie voor gebruik door client-side JavaScript. Gelukkig is de aangepaste FPID servlet-logica lichtgewicht, waardoor deze aanvraag geen invloed kan hebben op de prestaties van de AEM-publicatieservice.
1. De reactie voor het XHR-verzoek retourneert naar de browser met het FPID-cookie en de FPID als JSON in de antwoordhoofdtekst voor gebruik door de Platform Web SDK.

## Codevoorbeeld

De volgende code en configuratie kunnen aan de publicatieservice van AEM worden opgesteld om een eindpunt tot stand te brengen dat, het leven van een bestaand FPID koekje produceert of uitbreidt en FPID als JSON terugkeert.

### AEM Publish FPID cookie servlet

Een AEM publiceert HTTP- eindpunt moet worden gecreeerd om een koekje te produceren of uit te breiden FPID, gebruikend a [ het Schipen servlet ](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1).

+ De servlet is gebonden aan `/bin/aem/fpid` aangezien de authentificatie niet wordt vereist om tot het toegang te hebben. Als de authentificatie wordt vereist, verbind aan een Verschuivend middeltype.
+ De servlet accepteert HTTP GET-aanvragen. De reactie wordt duidelijk met `Cache-Control: no-store` om caching te verhinderen, maar dit eindpunt zou ook moeten worden gevraagd gebruikend unieke cache-opstellende vraagparameters.

Wanneer een HTTP-aanvraag de servlet bereikt, controleert de servlet of er een FPID-cookie bestaat op de aanvraag:

+ Als er een FPID-cookie bestaat, verlengt u de levensduur van de cookie en int u de waarde ervan om naar de reactie te schrijven.
+ Als een FPID-cookie niet bestaat, genereert u een nieuw FPID-cookie en slaat u de waarde op om naar het antwoord te schrijven.

Vervolgens schrijft de servlet de FPID naar de reactie als een JSON-object in de vorm: `{ fpid: "<FPID VALUE>" }` .

Het is belangrijk dat u de FPID aan de client in de hoofdtekst opgeeft, aangezien het FPID-cookie is gemarkeerd `HttpOnly` . Dit betekent dat alleen de server de waarde ervan kan lezen en dat JavaScript dat niet kan. Om te voorkomen dat de FPID onnodig wordt vernieuwd bij elke pagina die wordt geladen, wordt ook een `FPID_CLIENT` -cookie ingesteld die aangeeft dat de FPID is gegenereerd en die de waarde voor gebruik toegankelijk maakt voor de client-side JavaScript.

De waarde FPID wordt gebruikt om vraag te bepalen gebruikend het Web SDK van het Platform.

Hieronder ziet u voorbeeldcode van een AEM servlet-eindpunt (beschikbaar via `HTTP GET /bin/aep/fpid` ) waarmee een FPID-cookie wordt gegenereerd of vernieuwd en de FPID als JSON wordt geretourneerd.

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
    private static final String CLIENT_COOKIE_NAME = "FPID_CLIENT";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13; // 13 months
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists, get its FPID UUID so its life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the FPID value to the response, either newly generated or the extended one
        // This can be read by the Server (AEM Publish) due to HttpOnly flag.
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");

        // Also set FPID_CLIENT cookie to avoid further server-side FPID generation
        // This can be read by the client-side JavaScript to check if FPID is already generated
        // or if it needs to be requested from server (AEM Publish)
        response.addHeader("Set-Cookie",
                CLIENT_COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "Secure; " + 
                        "SameSite=Lax");

        // Avoid caching the response
        response.addHeader("Cache-Control", "no-store");

        // Return FPID in the response as JSON for client-side access
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);

        response.setContentType("application/json");
        response.getWriter().write(json.toString());
```

### HTML-script

Een aangepaste client-side JavaScript moet aan de pagina worden toegevoegd om de servlet asynchroon aan te roepen, het FPID-cookie te genereren of te vernieuwen en de FPID in de reactie te retourneren.

Dit JavaScript-script wordt doorgaans op een van de volgende manieren aan de pagina toegevoegd:

+ [ Markeringen in Adobe Experience Platform ](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=nl-NL)
+ [ de Bibliotheek van de Cliënt van AEM ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=nl-NL)

De XHR-aanroep naar de aangepaste AEM FPID-server is snel, maar asynchroon, zodat een gebruiker een webpagina kan bezoeken die door AEM wordt aangeboden en kan wegnavigeren voordat het verzoek kan worden voltooid.
Als dit gebeurt, wordt hetzelfde proces opnieuw gestart bij het laden van de volgende pagina van een webpagina vanuit AEM.

HTTP GET aan de server van AEM FPID (`/bin/aep/fpid`) wordt parameters bepaald met een willekeurige vraagparameter om ervoor te zorgen dat om het even welke infrastructuur tussen browser en de dienst van de Publicatie van AEM niet de reactie van het verzoek in het voorgeheugen onderbrengt.
Op dezelfde manier wordt de aanvraagheader van `Cache-Control: no-store` toegevoegd om caching te voorkomen.

Op een aanroeping van AEM FPID servlet, wordt FPID teruggewonnen van de reactie JSON en door het [ Web SDK van het Platform ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=nl-NL) gebruikt om het naar Experience Platform APIs te verzenden.

Zie de documentatie van Experience Platform voor meer informatie over [ gebruikend FPIDs in identityMap ](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html?lang=nl-NL#identityMap)

```javascript
...
<script>
    // Wrap in anonymous function to avoid global scope pollution

    (function() {
        // Utility function to get a cookie value by name
        function getCookie(name) {
            const value = `; ${document.cookie}`;
            const parts = value.split(`; ${name}=`);
            if (parts.length === 2) return parts.pop().split(';').shift();
        }

        // Async function to handle getting the FPID via fetching from AEM, or reading an existing FPID_CLIENT cookie
        async function getFpid() {
            let fpid = getCookie('FPID_CLIENT');
            
            // If FPID can be retrieved from FPID_CLIENT then skip fetching FPID from server
            if (!fpid) {
                // Fetch FPID from the server if no FPID_CLIENT cookie value is present
                try {
                    const response = await fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, {
                        method: 'GET',
                        headers: {
                            'Cache-Control': 'no-store'
                        }
                    });
                    const data = await response.json();
                    fpid = data.fpid;
                } catch (error) {
                    console.error('Error fetching FPID:', error);
                }
            }

            console.log('My FPID is: ', fpid);
            return fpid;
        }

        // Invoke the async function to fetch or skip FPID
        const fpid = await getFpid();

        // Add the fpid to the identityMap in the Platform Web SDK
        // and/or send to AEP via AEP tags or direct AEP Web SDK calls (alloy.js)
    })();
</script>
```

### Dispatcher allow, filter

Ten slotte moeten HTTP GET-aanvragen voor de aangepaste FPID-server zijn toegestaan via de AEM Dispatcher `filter.any` -configuratie.

Als deze Dispatcher-configuratie niet correct is geïmplementeerd, resulteert de HTTP GET-aanvraag voor `/bin/aep/fpid` in een 404-resultaat.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platform-bronnen

Bekijk de volgende Experience Platform-documentatie voor FPID&#39;s (First-Party Device ID&#39;s) en het beheer van identiteitsgegevens met Platform Web SDK.

+ [ produceer eerste-partijapparaat IDs ](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=nl-NL)
+ [ Eerste-partij apparaat IDs in het Web SDK van het Platform ](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html?lang=nl-NL)
+ [ Gegevens van de Identiteit in het Web SDK van het Platform ](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=nl-NL)
