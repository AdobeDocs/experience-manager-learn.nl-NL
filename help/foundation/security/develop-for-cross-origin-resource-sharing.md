---
title: Ontwikkelen voor het delen van bronnen tussen verschillende bronnen (CORS) met AEM
description: Een kort voorbeeld van het gebruik van CORS voor toegang tot AEM inhoud van een externe webtoepassing via client-side JavaScript.
version: 6.4, 6.5
topic: Security, Development
role: Developer
level: Beginner
feature: Security
exl-id: 867cf74e-44e7-431b-ac8f-41b63c370635
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---

# Ontwikkelen voor het delen van bronnen tussen verschillende bronnen (CORS)

Een kort voorbeeld van hefboomwerking [!DNL CORS] om via JavaScript op de client toegang te krijgen tot AEM inhoud van een externe webtoepassing.

>[!VIDEO](https://video.tv.adobe.com/v/18837?quality=12&learn=on)

In deze video:

* **www.example.com** maps to localhost via `/etc/hosts`
* **aem-publish.local** maps to localhost via `/etc/hosts`
* SimpleHTTPServer (een wrapper voor [[!DNL Python]SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html)) geeft de pagina HTML via poort 8000 weer.
   * _Niet meer beschikbaar in Mac App Store. Gebruik zoals [Jeeves](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)._
* [!DNL AEM Dispatcher] is ingeschakeld [!DNL Apache HTTP Web Server] 2.4 en verzoek om een reverse-proxying `aem-publish.local` tot `localhost:4503`.

Voor meer informatie raadpleegt u [Werken met het delen van bronnen tussen verschillende bronnen (CORS) in AEM](./understand-cross-origin-resource-sharing.md).

## www.example.com HTML en JavaScript

Deze webpagina heeft de logica dat

1. Wanneer u op de knop klikt
1. Maakt een [!DNL AJAX GET] verzoek om `http://aem-publish.local/content/we-retail/.../experience/_jcr_content.1.json`
1. Hiermee wordt het dialoogvenster `jcr:title` het JSON-antwoord
1. Hiermee wordt het `jcr:title` in het DOM

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

Als u het in cache plaatsen en serveren van CORS-kopteksten wilt toestaan voor inhoud in cache, voegt u het volgende toe [/clientheader-configuratie](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#specifying-the-http-headers-to-pass-through-clientheaders) naar alle ondersteunde publicaties van AEM `dispatcher.any` bestanden.

```
/myfarm { 
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

**De webservertoepassing opnieuw starten** na het aanbrengen van wijzigingen in de `dispatcher.any` bestand.

Waarschijnlijk wordt de cache volledig gewist om ervoor te zorgen dat de koppen op de juiste wijze in de cache worden geplaatst na een `/clientheaders` configuratie bijwerken.

## Ondersteunende materialen {#supporting-materials}

* [AEM OSGi-fabriek voor beleid voor het delen van bronnen tussen verschillende bronnen](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Jeeves voor macOS](https://apps.apple.com/us/app/jeeves-local-http-server/id980824182?mt=12)
* [Python SimpleHTTPServer](https://docs.python.org/2/library/simplehttpserver.html) (compatibel met Windows/macOS/Linux)

* [Werken met het delen van bronnen tussen verschillende bronnen (CORS) in AEM](./understand-cross-origin-resource-sharing.md)
* [Delen van bronnen tussen verschillende bronnen (W3C)](https://www.w3.org/TR/cors/)
* [HTTP Access Control (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
