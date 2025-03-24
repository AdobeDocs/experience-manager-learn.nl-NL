---
title: Ontwikkelen voor het delen van bronnen tussen verschillende bronnen (CORS) met AEM
description: Een kort voorbeeld van het gebruik van CORS voor toegang tot AEM-inhoud vanuit een externe webtoepassing via client-side JavaScript.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
doc-type: Technical Video
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
duration: 333
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Ontwikkelen voor het delen van bronnen tussen verschillende bronnen (CORS)

Een kort voorbeeld van het gebruik van [!DNL CORS] voor toegang tot AEM-inhoud vanuit een externe webtoepassing via client-side JavaScript. Dit voorbeeld gebruikt de configuratie CORS OSGi om de toegang van CORS op AEM toe te laten. De OSGi-configuratiebenadering is levensvatbaar wanneer:

* Eén oorsprong benadert AEM Publish-inhoud
* Voor AEM-auteur is CORS-toegang vereist

Als de multi-oorsprongstoegang tot AEM wordt vereist, verwijs naar [ dit document ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

In deze video:

* **www.example.com** kaarten aan localhost via `/etc/hosts`
* **aem-publish.local** kaarten aan localhost via `/etc/hosts`
* SimpleHTTPServer (een omslag voor [[!DNL Python] SimpleHTTPServer ](https://docs.python.org/2/library/simplehttpserver.html)) dient de HTML-pagina via poort 8000.
   * _niet meer beschikbaar in Mac App Store. Gebruik gelijkaardig zoals [ Jeeves ](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] wordt uitgevoerd op [!DNL Apache HTTP Web Server] 2.4 en omgekeerd-proxying request to `aem-publish.local` to `localhost:4503` .

Voor meer details, herzie [ Begrijpend Middel dat van de Cross-Origin (CORS) deelt in AEM ](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML en JavaScript

Deze webpagina heeft de logica dat

1. Wanneer u op de knop klikt
1. Maakt een [!DNL AJAX GET] aanvraag aan `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Hiermee wordt de `jcr:title` opgehaald van het JSON-antwoord
1. Injecteert de `jcr:title` in de DOM

```xml
<html>
<head>
<script
  src="https://code.jquery.com/jquery-3.2.1.min.js"
  integrity="sha256-hwg4gsxgFZhOsEEamdOYGBf13FyQuiTwlAQgxVSNgt4="
  crossorigin="anonymous"></script>   
</head>
<body style="width: 960px; margin: 2rem auto; font-size: 2rem;">
    <button style="font-size: 2rem;"
            data="fn-getTitle">Get Title as JSON from AEM</button>
    <pre id="title">The page title AJAX'd in from AEM will injected here</pre>
    
    <script>
    $(function() { 
        
        /** Get Title as JSON **/
        $('body').on('click', '[data="fn-getTitle"]', function(e) { 
            $.get('http://aem-publish.local/content/we-retail/us/en/experience/_jcr_content.1.json', function(data) {
                $('#title').text(data['jcr:title']);
            },'json');
            
            e.preventDefault();
            return false;
        });
    });
    </script>
</body>
</html>
```

## OSGi-fabrieksconfiguratie

De OSGi Configuration factory for [!DNL Cross-Origin Resource Sharing] is beschikbaar via:

* `http://<host>:<port>/system/console/configMgr > [!UICONTROL Adobe Granite Cross-Origin Resource Sharing Policy]`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://www.example.com:8000]"
    alloworiginregexp="[]"
    allowedpaths="[/content/we-retail/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

## Dispatcher-configuratie {#dispatcher-configuration}

### CORS-aanvraagheaders toestaan

Om de vereiste [ HTTP- verzoekkopballen toe te staan om tot AEM voor verwerking ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) over te gaan, moeten zij in de 2} configuratie van Disaptcher worden toegestaan.`/clientheaders`

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### CORS-responsheaders in cache plaatsen

Als u het in cache plaatsen en serveren van CORS-headers voor in cache opgeslagen inhoud wilt toestaan, voegt u de volgende [ /cache/headers-configuratie ](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#caching-http-response-headers) toe aan het AEM Publish `dispatcher.any` -bestand.

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

**herstart de toepassing van de Webserver** na het aanbrengen van veranderingen in het `dispatcher.any` dossier.

Het is waarschijnlijk dat het cachegeheugen volledig moet worden gewist om ervoor te zorgen dat de koppen op de juiste wijze in het cachegeheugen worden opgeslagen op het volgende verzoek na een `/cache /headers` configuratieupdate.

## Ondersteunende materialen {#supporting-materials}

* [ Jeeves voor macOS ](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [ Python SimpleHTTPServer ](https://docs.python.o:qrg/2/library/simplehttpserver.html) (compatibel Windows/macOS/Linux)

* [Werken met het delen van bronnen tussen verschillende oorsprong (CORS) in AEM](./understand-cross-origin-resource-sharing.md)
* [ het Delen van het Middel van de dwars-Oorsprong (W3C) ](https://www.w3.org/TR/cors/)
* [ Controle van de Toegang van HTTP (Mozilla MDN) ](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
