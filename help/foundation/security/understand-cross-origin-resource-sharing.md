---
title: Werken met het delen van bronnen tussen verschillende bronnen (CORS) met AEM
description: Met CORS (Cross-Origin Resource Sharing) van Adobe Experience Manager kunnen niet-AEM wegeigenschappen client-side aanroepen maken naar AEM, zowel geverifieerd als niet geverifieerd, om inhoud op te halen of rechtstreeks met AEM te communiceren.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
feature: Security, APIs
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: 325c0204c33686e09deb82dd159557e0b8743df6
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# Werken met het delen van bronnen die van oorsprong zijn ([!DNL CORS])

Delen van bronnen tussen verschillende oorsprong van Adobe Experience Manager ([!DNL CORS]) vergemakkelijkt niet-AEM wegeigenschappen om cliënt-zijvraag aan AEM te maken, zowel voor authentiek verklaard als niet voor authentiek verklaard, om inhoud te halen of direct met AEM in wisselwerking te staan.

## Adobe Granite Cross-Origin Resource Sharing Policy OSGi-configuratie

De configuraties van CORS worden beheerd als OSGi- configuratiefabrieken in AEM, waarbij elk beleid als één geval van de fabriek wordt vertegenwoordigd.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite Cross-Origin Resource Sharing Policy OSGi-configuratie](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Beleidsselectie

Een beleid wordt geselecteerd door te vergelijken

* `Allowed Origin` met de `Origin` aanvraagkoptekst
* en `Allowed Paths` met het aanvraagpad.

Het eerste beleid dat deze waarden afstemt, wordt gebruikt. Als er geen wordt gevonden, worden alle [!DNL CORS] verzoek wordt afgewezen.

Als geen beleid bij allen wordt gevormd, [!DNL CORS] verzoeken zullen ook niet worden beantwoord aangezien de manager onbruikbaar wordt gemaakt en zo effectief ontkend - zolang geen andere module van de server aan antwoordt [!DNL CORS].

### Beleidseigenschappen

#### [!UICONTROL Allowed Origins]

* `"alloworigin" <origin> | *`
* Lijst van `origin` parameters die URI&#39;s opgeven die toegang kunnen krijgen tot de bron. Voor aanvragen zonder referenties kan de server &#42; als een jokerteken, waarbij elke oorsprong toegang heeft tot de bron. *Het is absoluut niet aan te raden `Allow-Origin: *` in productie, aangezien elke buitenlandse (d.w.z. aanvaller) website verzoeken kan indienen dat browsers het zonder CORS strikt verbieden.*

#### [!UICONTROL Allowed Origins (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lijst van `regexp` reguliere expressies die URI&#39;s opgeven die toegang kunnen krijgen tot de bron. *Reguliere expressies kunnen leiden tot onbedoelde overeenkomsten als deze niet zorgvuldig worden samengesteld, waardoor een aanvaller een aangepaste domeinnaam kan gebruiken die ook met het beleid overeenkomt.* Het wordt over het algemeen aanbevolen voor elke specifieke oorspronkelijke hostnaam een afzonderlijk beleid te gebruiken. `alloworigin`, zelfs als dat herhaalde configuratie van de andere beleidseigenschappen betekent. Verschillende afkomst heeft vaak verschillende levenscycli en eisen, en profiteert dus van een duidelijke scheiding.

#### [!UICONTROL Allowed Paths]

* `"allowedpaths" <regexp>`
* Lijst van `regexp` reguliere expressies die bronpaden aangeven waarop het beleid van toepassing is.

#### [!UICONTROL Exposed Headers]

* `"exposedheaders" <header>`
* Lijst met koptekstparameters die responsheaders aangeven waartoe browsers toegang hebben.

#### [!UICONTROL Maximum Age]

* `"maxage" <seconds>`
* A `seconds` parameter die aangeeft hoe lang de resultaten van een aan de vlucht voorafgaande aanvraag in de cache kunnen worden opgeslagen.

#### [!UICONTROL Supported Headers]

* `"supportedheaders" <header>`
* Lijst van `header` parameters die aangeven welke HTTP-headers kunnen worden gebruikt bij het uitvoeren van de eigenlijke aanvraag.

#### [!UICONTROL Allowed Methods]

* `"supportedmethods"`
* Lijst met methodeparameters die aangeven welke HTTP-methoden kunnen worden gebruikt bij het uitvoeren van de eigenlijke aanvraag.

#### [!UICONTROL Supports Credentials]

* `"supportscredentials" <boolean>`
* A `boolean` die aangeeft of de reactie op het verzoek al dan niet aan de browser kan worden getoond. Indien gebruikt als onderdeel van een reactie op een verzoek vóór de vlucht, geeft dit aan of het feitelijke verzoek kan worden ingediend met behulp van referenties.

### Voorbeelden van configuraties

Site 1 is een eenvoudig, anoniem toegankelijk, alleen-lezen scenario waarbij inhoud via [!DNL GET] verzoeken:

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "OPTIONS"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
  ]
}
```

Site 2 is complexer en vereist geautoriseerde en mutatieverzoeken (POST, PUT, DELETE):

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "OPTIONS",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Problemen met het in cache plaatsen van verzendingen en configuratie {#dispatcher-caching-concerns-and-configuration}

Vanaf Dispatcher 4.1.1+ kunnen responsheaders in cache worden geplaatst. Hierdoor is het mogelijk om in cache te plaatsen [!DNL CORS] koppen langs de [!DNL CORS]- gevraagde middelen, zolang het verzoek anoniem is.

Over het algemeen kunnen dezelfde overwegingen voor het in cache plaatsen van inhoud bij Dispatcher worden toegepast op het in cache plaatsen van CORS-antwoordheaders bij dispatcher. In de volgende tabel wordt gedefinieerd wanneer [!DNL CORS] kopteksten (en dus [!DNL CORS] aanvragen) kunnen in de cache worden opgeslagen.

| Cacheable | Omgeving | Verificatiestatus | Toelichting |
|-----------|-------------|-----------------------|-------------|
| Nee | AEM-publicatie | Geverifieerd | Het in cache plaatsen van verzendingen op AEM-auteur is beperkt tot statische, niet-geschreven elementen. Hierdoor is het moeilijk en onpraktisch om de meeste bronnen in cache te plaatsen op AEM Author, inclusief HTTP response headers. |
| Nee | AEM-publicatie | Geverifieerd | Vermijd het in cache plaatsen van CORS-koppen bij geverifieerde aanvragen. Dit richt zich op de gemeenschappelijke begeleiding van niet caching voor authentiek verklaarde verzoeken, aangezien het moeilijk is om te bepalen hoe de authentificatie/vergunningsstatus van de het verzoeken gebruiker de geleverde middel zal beïnvloeden. |
| Ja | AEM-publicatie | Anoniem | De anonieme verzoeken cache-able bij verzender kunnen hun antwoordkopballen in het voorgeheugen onder brengen ook, ervoor zorgen de toekomstige verzoeken CORS tot de caching inhoud kunnen toegang hebben. Elke wijziging in de CORS-configuratie in AEM-publicatie **moet** gevolgd door een ongeldigverklaring van betrokken in cache opgeslagen bronnen. De beste praktijken dicteren op code of configuratieplaatsingen het verzendeergeheime voorgeheugen wordt gezuiverd, aangezien het moeilijk is om te bepalen welke caching inhoud kan worden uitgevoerd. |

### CORS-aanvraagheaders toestaan

Om de vereiste [HTTP-aanvraagheaders om door te gaan naar AEM voor verwerking](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders), moeten ze in de Disaptcher zijn toegestaan `/clientheaders` configuratie.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### CORS-responsheaders in cache plaatsen

Als u het in cache plaatsen en serveren van CORS-kopteksten wilt toestaan voor inhoud in cache, voegt u het volgende toe [/cache /headers-configuratie](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#caching-http-response-headers) naar de AEM-publicatie `dispatcher.any` bestand.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

Herinneren aan **de webservertoepassing opnieuw starten** na het aanbrengen van wijzigingen in de `dispatcher.any` bestand.

Waarschijnlijk wordt de cache volledig gewist om ervoor te zorgen dat de koppen op de juiste wijze in de cache worden geplaatst na een `/cache/headers` configuratie bijwerken.

## Problemen met CORS oplossen

Logboekregistratie is beschikbaar onder `com.adobe.granite.cors`:

* enable `DEBUG` voor meer informatie over waarom een [!DNL CORS] verzoek afgewezen
* enable `TRACE` om details over alle verzoeken te zien die door de manager CORS gaan

### Tips:

* Maak handmatig XHR-verzoeken opnieuw met krullen, maar zorg dat u alle kopteksten en details kopieert, aangezien elk verzoek een verschil kan maken. sommige browserconsoles staan toe dat de krullopdracht wordt gekopieerd
* Verifieer of het verzoek door de manager CORS en niet door de authentificatie, het symbolische filter CSRF, verzenders filters, of andere veiligheidslagen werd ontkend
   * Als de manager CORS met 200 antwoordt, maar `Access-Control-Allow-Origin` header ontbreekt in de reactie, bekijk de logboeken voor weigeringen onder [!DNL DEBUG] in `com.adobe.granite.cors`
* Indien verzender, caching van [!DNL CORS] aanvragen zijn ingeschakeld
   * Zorg ervoor dat de `/cache/headers` configuratie wordt toegepast op `dispatcher.any` en de webserver is opnieuw gestart
   * Zorg ervoor dat de cache juist is gewist nadat de configuratie van OSGi of dispatcher.any is gewijzigd.
* Controleer, indien nodig, de aanwezigheid van verificatiegegevens op het verzoek.

## Ondersteunende materialen

* [AEM OSGi-fabriek voor beleid voor het delen van bronnen tussen verschillende bronnen](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Delen van bronnen tussen verschillende bronnen (W3C)](https://www.w3.org/TR/cors/)
* [HTTP Access Control (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
