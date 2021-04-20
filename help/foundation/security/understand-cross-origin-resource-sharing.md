---
title: Werken met het delen van bronnen tussen verschillende bronnen (CORS) met AEM
description: Met CORS (Cross-Origin Resource Sharing) van Adobe Experience Manager kunnen niet-AEM wegeigenschappen client-side aanroepen maken naar AEM, zowel geverifieerd als niet geverifieerd, om inhoud op te halen of rechtstreeks met AEM te communiceren.
version: 6.3, 6,4, 6.5
sub-product: stichting, inhoudsdiensten, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---


# Werken met het delen van bronnen van verschillende oorsprong ([!DNL CORS])

Het delen van het Middel van het Middel van de Samenkomst van Adobe Experience Manager ([!DNL CORS]) vergemakkelijkt niet-AEM Web-eigenschappen om cliënt-zijvraag aan AEM te maken, zowel voor authentiek verklaard als niet voor authentiek verklaard, om inhoud te halen of direct met AEM in wisselwerking te staan.

## Adobe Granite Cross-Origin Resource Sharing Policy OSGi-configuratie

De configuraties van CORS worden beheerd als OSGi- configuratiefabrieken in AEM, waarbij elk beleid als één geval van de fabriek wordt vertegenwoordigd.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite Cross-Origin Resource Sharing Policy OSGi-configuratie](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Beleidsselectie

Een beleid wordt geselecteerd door te vergelijken

* `Allowed Origin` met de  `Origin` aanvraagkoptekst
* en `Allowed Paths` met het aanvraagpad.

Het eerste beleid dat aan deze waarden voldoet, wordt gebruikt. Als geen wordt gevonden, zal om het even welk [!DNL CORS] verzoek worden ontkend.

Als er helemaal geen beleid is geconfigureerd, worden [!DNL CORS]-verzoeken ook niet beantwoord omdat de handler wordt uitgeschakeld en dus feitelijk wordt geweigerd - zolang geen andere module van de server reageert op [!DNL CORS].

### Beleidseigenschappen

#### [!UICONTROL Allowed Origins]

* `"alloworigin" <origin> | *`
* Lijst met `origin`-parameters die URI&#39;s opgeven die toegang kunnen krijgen tot de bron. Voor verzoeken zonder geloofsbrieven, kan de server * als vervanging specificeren, daardoor toestaand om het even welke oorsprong om tot het middel toegang te hebben. *Het is absoluut niet aan te bevelen om  `Allow-Origin: *` in productie te gebruiken aangezien het elke buitenlandse (d.w.z. aanvaller) website toestaat verzoeken te doen die zonder CORS strikt door browsers worden verboden.*

#### [!UICONTROL Allowed Origins (Regexp)]

* `"alloworiginregexp" <regexp>`
* Lijst met reguliere expressies `regexp` die URI&#39;s opgeven die toegang hebben tot de bron. *Reguliere expressies kunnen leiden tot onbedoelde overeenkomsten als deze niet zorgvuldig worden samengesteld, waardoor een aanvaller een aangepaste domeinnaam kan gebruiken die ook met het beleid overeenkomt.* Het wordt over het algemeen geadviseerd om afzonderlijk beleid voor elke specifieke oorsprong te hebben hostname, gebruikend  `alloworigin`, zelfs als dat herhaalde configuratie van de andere beleidseigenschappen betekent. Verschillende afkomst heeft vaak verschillende levenscycli en eisen, en profiteert dus van een duidelijke scheiding.

#### [!UICONTROL Allowed Paths]

* `"allowedpaths" <regexp>`
* Lijst met reguliere expressies `regexp` die bronpaden aangeven waarop het beleid van toepassing is.

#### [!UICONTROL Exposed Headers]

* `"exposedheaders" <header>`
* Lijst met koptekstparameters die aanvraagheaders aangeven waartoe browsers toegang hebben.

#### [!UICONTROL Maximum Age]

* `"maxage" <seconds>`
* Een parameter `seconds` die aangeeft hoe lang de resultaten van een aan de vlucht voorafgaand verzoek in de cache kunnen worden opgeslagen.

#### [!UICONTROL Supported Headers]

* `"supportedheaders" <header>`
* Lijst met parameters `header` die aangeven welke HTTP-headers kunnen worden gebruikt bij het uitvoeren van de eigenlijke aanvraag.

#### [!UICONTROL Allowed Methods]

* `"supportedmethods"`
* Lijst met methodeparameters die aangeven welke HTTP-methoden kunnen worden gebruikt bij het uitvoeren van de eigenlijke aanvraag.

#### [!UICONTROL Supports Credentials]

* `"supportscredentials" <boolean>`
* Een `boolean` die erop wijst of de reactie op het verzoek aan browser kan worden blootgesteld of niet. Indien gebruikt als onderdeel van een reactie op een verzoek vóór de vlucht, geeft dit aan of het feitelijke verzoek kan worden ingediend met behulp van referenties.

### Voorbeelden van configuraties

Site 1 is een eenvoudig, anoniem toegankelijk, alleen-lezen scenario waarbij inhoud wordt verbruikt via [!DNL GET]-verzoeken:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

Site 2 is complexer en vereist geautoriseerde en onveilige verzoeken:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Problemen met het in cache plaatsen van verzendingen en configuratie {#dispatcher-caching-concerns-and-configuration}

Vanaf Dispatcher 4.1.1+ kunnen responsheaders in cache worden geplaatst. Dit maakt het mogelijk om [!DNL CORS] kopballen langs de [!DNL CORS]-gevraagde middelen in het voorgeheugen onder te brengen, zolang het verzoek anoniem is.

Over het algemeen kunnen dezelfde overwegingen voor het in cache plaatsen van inhoud bij Dispatcher worden toegepast op het in cache plaatsen van CORS-antwoordheaders bij dispatcher. De volgende lijst bepaalt wanneer [!DNL CORS] kopballen (en zo [!DNL CORS] verzoeken) in het voorgeheugen kunnen worden opgeslagen.

| Cacheable | Omgeving | Verificatiestatus | Toelichting |
|-----------|-------------|-----------------------|-------------|
| Nee | AEM-publicatie | Geverifieerd | Het in cache plaatsen van verzendingen op AEM-auteur is beperkt tot statische, niet-geschreven elementen. Hierdoor is het moeilijk en onpraktisch om de meeste bronnen in cache te plaatsen op AEM Author, inclusief HTTP response headers. |
| Nee | AEM-publicatie | Geverifieerd | Vermijd het in cache plaatsen van CORS-koppen bij geverifieerde aanvragen. Dit richt zich op de gemeenschappelijke begeleiding van niet caching voor authentiek verklaarde verzoeken, aangezien het moeilijk is om te bepalen hoe de authentificatie/vergunningsstatus van de het verzoeken gebruiker de geleverde middel zal beïnvloeden. |
| Ja | AEM-publicatie | Anoniem | De anonieme verzoeken cache-able bij verzender kunnen hun antwoordkopballen in het voorgeheugen onder brengen ook, ervoor zorgen de toekomstige verzoeken CORS tot de caching inhoud kunnen toegang hebben. Elke wijziging in de CORS-configuratie op AEM-publicaties **must** wordt gevolgd door een ongeldigverklaring van de betrokken bronnen in de cache. De beste praktijken dicteren op code of configuratieplaatsingen het verzendeergeheime voorgeheugen wordt gezuiverd, aangezien het moeilijk is om te bepalen welke caching inhoud kan worden uitgevoerd. |

Als u het in cache plaatsen van CORS-headers wilt toestaan, voegt u de volgende configuratie toe aan alle ondersteunde AEM Publish dispatcher.any-bestanden.

```
/cache { 
  ...
  /clientheaders {
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

**Start de webservertoepassing opnieuw** nadat u wijzigingen hebt aangebracht in het `dispatcher.any`-bestand.

Het zal waarschijnlijk het geheime voorgeheugen volledig worden ontruimd zal worden vereist om de kopballen geschikt in het voorgeheugen onder te brengen op het volgende verzoek na een `/clientheaders` configuratiestupdate.

## Problemen met CORS oplossen

Logboekregistratie is beschikbaar onder `com.adobe.granite.cors`:

* laat `DEBUG` toe om details te zien over waarom een [!DNL CORS] verzoek werd ontkend
* laat `TRACE` toe om details over alle verzoeken te zien die door de manager CORS gaan

### Tips:

* Maak handmatig XHR-verzoeken opnieuw met krullen, maar zorg dat u alle kopteksten en details kopieert, aangezien elk verzoek een verschil kan maken. sommige browserconsoles staan toe dat de krullopdracht wordt gekopieerd
* Verifieer of het verzoek door de manager CORS en niet door de authentificatie, het symbolische filter CSRF, verzenders filters, of andere veiligheidslagen werd ontkend
   * Als de manager van CORS met 200 antwoordt, maar `Access-Control-Allow-Origin` kopbal in de reactie ontbreekt, herzie de logboeken voor ontkenning onder [!DNL DEBUG] in `com.adobe.granite.cors`
* Als de verzender caching van [!DNL CORS] verzoeken wordt toegelaten
   * Controleer of de `/clientheaders`-configuratie is toegepast op `dispatcher.any` en of de webserver opnieuw is gestart
   * Zorg ervoor dat de cache juist is gewist nadat de configuratie van OSGi of dispatcher.any is gewijzigd.
* Controleer, indien nodig, de aanwezigheid van verificatiegegevens op het verzoek.

## Ondersteunende materialen

* [AEM OSGi-fabriek voor beleid voor het delen van bronnen tussen verschillende bronnen](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Delen van bronnen tussen verschillende bronnen (W3C)](https://www.w3.org/TR/cors/)
* [HTTP Access Control (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
