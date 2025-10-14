---
title: Werken met het delen van bronnen tussen verschillende oorsprong (CORS) met AEM
description: Met CORS (Cross-Origin Resource Sharing) van Adobe Experience Manager kunnen niet-AEM-webeigenschappen client-side aanroepen naar AEM uitvoeren, zowel geverifieerd als niet geverifieerd, om inhoud op te halen of rechtstreeks te communiceren met AEM.
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: Security, APIs
doc-type: Article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 0%

---

# Werken met het delen van bronnen met verschillende oorsprong ([!DNL CORS])

Het delen van het Middel van het Middel van de Samenkomst van Adobe Experience Manager ([!DNL CORS]) vergemakkelijkt niet-AEM Web-eigenschappen om cliënt-zijvraag aan AEM te maken, zowel voor authentiek verklaard als niet voor authentiek verklaard, om inhoud te halen of direct met AEM in wisselwerking te staan.

De configuratie OSGI die in dit document wordt beschreven is voldoende voor:

1. Delen van bronnen van één oorsprong in AEM Publish
2. Toegang tot AEM-auteur voor CORS

Als de multi-oorsprong toegang van CORS op AEM wordt vereist publiceer, verwijs naar [&#x200B; deze documentatie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=nl-NL#dispatcher-configuration).

## Adobe Granite Cross-Origin Resource Sharing Policy OSGi-configuratie

De configuraties CORS worden beheerd als OSGi- configuratiemachines in AEM, waarbij elk beleid als één geval van de fabriek wordt vertegenwoordigd.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![&#x200B; Adobe Granite het Middel van de Cross-Origin het Delen van Beleid OSGi configuratie &#x200B;](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Beleidsselectie

Een beleid wordt geselecteerd door te vergelijken

* `Allowed Origin` met de `Origin` request header
* en `Allowed Paths` met het aanvraagpad.

Het eerste beleid dat deze waarden afstemt, wordt gebruikt. Als er geen wordt gevonden, wordt een [!DNL CORS] -aanvraag afgewezen.

Als er helemaal geen beleid is geconfigureerd, worden [!DNL CORS] -aanvragen ook niet beantwoord omdat de handler is uitgeschakeld en dus feitelijk wordt geweigerd, zolang geen andere module van de server reageert op [!DNL CORS] .

### Beleidseigenschappen

#### [!UICONTROL Allowed Origins]

* `"alloworigin" <origin> | *`
* Lijst met `origin` -parameters die URI&#39;s opgeven die toegang kunnen krijgen tot de bron. Voor aanvragen zonder referenties kan de server &#42; als jokerteken opgeven, zodat elke oorsprong toegang heeft tot de bron. *het is absoluut niet geadviseerd om `Allow-Origin: *` in productie te gebruiken aangezien het elke buitenlandse (d.w.z. aanvaller) website toestaat om verzoeken te doen die zonder CORS strikt door browsers worden verboden.*

#### [!UICONTROL Allowed Origins (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lijst met reguliere expressies van `regexp` die URI&#39;s opgeven die toegang kunnen krijgen tot de bron. *de regelmatige uitdrukkingen kunnen tot onbedoelde gelijken leiden als niet zorgvuldig gebouwd, toestaand een aanvaller om een naam van het douanedomein te gebruiken die ook het beleid zou aanpassen.* Over het algemeen wordt aangeraden voor elke specifieke oorspronkelijke hostnaam een apart beleid te gebruiken met `alloworigin` , zelfs als dat betekent dat de andere beleidseigenschappen herhaaldelijk moeten worden geconfigureerd. Verschillende afkomst heeft vaak verschillende levenscycli en eisen, en profiteert dus van een duidelijke scheiding.

#### [!UICONTROL Allowed Paths]

* `"allowedpaths" <regexp>`
* Lijst met reguliere expressies van `regexp` die bronpaden aangeven waarop het beleid van toepassing is.

#### [!UICONTROL Exposed Headers]

* `"exposedheaders" <header>`
* Lijst met koptekstparameters die responsheaders aangeven waartoe browsers toegang hebben. Voor CORS-aanvragen (niet vóór de vlucht) worden deze waarden gekopieerd naar de `Access-Control-Expose-Headers` response header als ze niet leeg zijn. De waarden in de lijst (koptekstnamen) worden vervolgens toegankelijk gemaakt voor de browser. Als dit niet het geval is, kunnen deze kopteksten niet worden gelezen door de browser.

#### [!UICONTROL Maximum Age]

* `"maxage" <seconds>`
* Een parameter `seconds` die aangeeft hoe lang de resultaten van een aan de vlucht voorafgaande aanvraag in de cache kunnen worden opgeslagen.

#### [!UICONTROL Supported Headers]

* `"supportedheaders" <header>`
* Lijst met `header` -parameters die aangeven welke HTTP-aanvraagheaders kunnen worden gebruikt bij het uitvoeren van de eigenlijke aanvraag.

#### [!UICONTROL Allowed Methods]

* `"supportedmethods"`
* Lijst met methodeparameters die aangeven welke HTTP-methoden kunnen worden gebruikt bij het uitvoeren van de eigenlijke aanvraag.

#### [!UICONTROL Supports Credentials]

* `"supportscredentials" <boolean>`
* A `boolean` die erop wijst of de reactie op het verzoek aan browser kan worden blootgesteld of niet. Indien gebruikt als onderdeel van een reactie op een verzoek vóór de vlucht, geeft dit aan of het feitelijke verzoek kan worden ingediend met behulp van referenties.

### Voorbeelden van configuraties

Site 1 is een eenvoudig, anoniem toegankelijk, alleen-lezen scenario waarbij inhoud wordt verbruikt via [!DNL GET] -aanvragen:

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
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
    "Access-Control-Request-Headers"
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

## Problemen met het in cache plaatsen van Dispatcher en configuratie {#dispatcher-caching-concerns-and-configuration}

Vanaf Dispatcher 4.1.1+ kunnen responsheaders in cache worden geplaatst. Hierdoor is het mogelijk om [!DNL CORS] headers in cache te plaatsen langs de [!DNL CORS] -requested bronnen, zolang de aanvraag anoniem is.

Over het algemeen kunnen dezelfde overwegingen voor het in cache plaatsen van inhoud in Dispatcher worden toegepast op het in cache plaatsen van CORS-antwoordheaders bij dispatcher. In de volgende tabel wordt gedefinieerd wanneer [!DNL CORS] headers (en dus [!DNL CORS] -aanvragen) in de cache kunnen worden geplaatst.

| Cacheable | Omgeving | Status van verificatie | Toelichting |
|-----------|-------------|-----------------------|-------------|
| Nee | AEM Publiceren | Geverifieerd | Dispatcher caching op AEM Author is beperkt tot statische, niet-authored assets. Hierdoor is het moeilijk en onpraktisch om de meeste bronnen op AEM Author in cache te plaatsen, inclusief HTTP-antwoordheaders. |
| Nee | AEM Publiceren | Geverifieerd | Vermijd het in cache plaatsen van CORS-koppen bij geverifieerde aanvragen. Dit richt zich op de gemeenschappelijke begeleiding van niet caching voor authentiek verklaarde verzoeken, aangezien het moeilijk is om te bepalen hoe de authentificatie/vergunningsstatus van de het verzoeken gebruiker de geleverde middel zal beïnvloeden. |
| Ja | AEM Publiceren | Anoniem | De anonieme verzoeken cache-able bij verzender kunnen hun antwoordkopballen in het voorgeheugen onder brengen ook, ervoor zorgen de toekomstige verzoeken CORS tot de caching inhoud kunnen toegang hebben. Om het even welke CORS configuratieverandering op AEM publiceert **moet** door een ongeldigverklaring van beïnvloede caching middelen worden gevolgd. De beste praktijken dicteren op code of configuratieplaatsingen het verzendeergeheime voorgeheugen wordt gezuiverd, aangezien het moeilijk is om te bepalen welke caching inhoud kan worden uitgevoerd. |

### CORS-aanvraagheaders toestaan

Om de vereiste [&#x200B; HTTP- verzoekkopballen toe te staan om tot AEM voor verwerking &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#specifying-the-http-headers-to-pass-through-clientheaders) over te gaan, moeten zij in de Dispatcher `/clientheaders` configuratie worden toegestaan.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### CORS-responsheaders in cache plaatsen

Als u het in cache plaatsen en serveren van CORS-headers voor in cache opgeslagen inhoud wilt toestaan, voegt u de volgende [&#x200B; /cache/headers-configuratie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#caching-http-response-headers) toe aan het AEM Publish `dispatcher.any` -bestand.

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

Herinner me aan **opnieuw begin de toepassing van de Webserver** na het aanbrengen van veranderingen in het `dispatcher.any` dossier.

Het is waarschijnlijk dat het cachegeheugen volledig moet worden gewist om ervoor te zorgen dat de headers op de juiste wijze in het cachegeheugen worden opgeslagen op het volgende verzoek na een `/cache/headers` configuratieupdate.

## Problemen met CORS oplossen

Logboekregistratie is beschikbaar onder `com.adobe.granite.cors` :

* inschakelen `DEBUG` om te zien waarom een [!DNL CORS] -aanvraag is afgewezen
* laat `TRACE` toe om details over alle verzoeken te zien die door de manager van CORS gaan

### Tips:

* Maak handmatig XHR-verzoeken opnieuw met krullen, maar kopieer alle kopteksten en details, omdat elk verzoek een verschil kan maken. Sommige browserconsoles staan toe om de krullopdracht te kopiëren
* Verifieer of het verzoek door de manager CORS en niet door de authentificatie, het symbolische filter CSRF, verzenders filters, of andere veiligheidslagen werd ontkend
   * Als de handler CORS reageert op 200, maar de header `Access-Control-Allow-Origin` niet aanwezig is in de reactie, controleert u de logboeken op weigeringen onder [!DNL DEBUG] in `com.adobe.granite.cors`
* Als caching van [!DNL CORS] -aanvragen door dispatcher is ingeschakeld
   * Controleer of de `/cache/headers` -configuratie is toegepast op `dispatcher.any` en of de webserver opnieuw is gestart
   * Zorg ervoor dat de cache juist is gewist nadat de configuratie van OSGi of dispatcher.any is gewijzigd.
* Controleer, indien nodig, de aanwezigheid van verificatiegegevens op het verzoek.

## Ondersteunende materialen

* [&#x200B; de fabriek van de Configuratie van AEM OSGi voor het Delen van het Middel van de Cross-Origin Beleid &#x200B;](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [&#x200B; het Delen van het Middel van de dwars-Oorsprong (W3C) &#x200B;](https://www.w3.org/TR/cors/)
* [&#x200B; Controle van de Toegang van HTTP (Mozilla MDN) &#x200B;](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
