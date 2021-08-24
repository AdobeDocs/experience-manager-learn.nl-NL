---
title: Ontwikkelen voor het delen van bronnen tussen verschillende bronnen (CORS) met AEM
description: Een kort voorbeeld van het gebruik van CORS voor toegang tot AEM inhoud van een externe webtoepassing via client-side JavaScript.
version: 6.3, 6,4, 6.5
topic: Veiligheid, ontwikkeling
role: Developer
level: Beginner
feature: Beveiliging
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 0%

---


# Ontwikkelen voor het delen van bronnen tussen verschillende bronnen (CORS)

Een kort voorbeeld van het gebruiken van [!DNL CORS] om tot AEM inhoud van een externe Webtoepassing via cliënt-kant JavaScript toegang te hebben.

>[!VIDEO](https://video.tv.adobe.com/v/18837/?quality=12&learn=on)

In deze video:

* **www.example.** commaps to localhost via  `/etc/hosts`
* **aem-publish.** localmaps to localhost via  `/etc/hosts`
* [SimpleHTTPServer](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)  (een wrapper voor  [[!DNL Python]&#39;s SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) geeft de HTML-pagina via poort 8000.
* [!DNL AEM Dispatcher] wordt uitgevoerd op  [!DNL Apache HTTP Web Server] 2.4 en omgekeerd proxying verzoek  `aem-publish.local` aan  `localhost:4503`.

Voor meer details, herzie [Begrijpen van het Middel van de Samenhang van de Herkomst (CORS) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML en JavaScript

Deze webpagina heeft de logica dat

1. Wanneer u op de knop klikt
1. Maakt een [!DNL AJAX GET] verzoek aan `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Hiermee wordt de `jcr:title` opgehaald uit het JSON-antwoord
1. Hiermee wordt de `jcr:title` in het DOM geïnjecteerd

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

De OSGi-configuratiefabriek voor [!DNL Cross-Origin Resource Sharing] is beschikbaar via:

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

Om het in cache plaatsen en het dienen van kopballen CORS op caching inhoud toe te staan, voeg het volgende [/clientheaders configuratie](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) aan allen die AEM toe publiceer `dispatcher.any` dossiers.

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

**Start de** toepassing van de webserver opnieuw nadat u wijzigingen in het  `dispatcher.any` bestand hebt aangebracht.

Waarschijnlijk wordt het cachegeheugen volledig gewist om ervoor te zorgen dat de koppen op de juiste wijze in het cachegeheugen worden opgeslagen op het volgende verzoek na een `/clientheaders` configuratieupdate.

## Ondersteunende materialen {#supporting-materials}

* [AEM OSGi-fabriek voor beleid voor het delen van bronnen tussen verschillende bronnen](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [SimpleHTTPServer voor macOS](https://itunes.apple.com/us/app/simple-http-server/id441002840?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)  (compatibel met Windows/macOS/Linux)

* [Werken met het delen van bronnen tussen verschillende bronnen (CORS) in AEM](./understand-cross-origin-resource-sharing.md)
* [Delen van bronnen tussen verschillende bronnen (W3C)](https://www.w3.org/TR/cors/)
* [HTTP Access Control (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

