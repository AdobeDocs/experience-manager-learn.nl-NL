---
title: CDN-caching inschakelen
description: Leer hoe te om het in cache plaatsen van de reacties van HTTP in AEM as a Cloud Service CDN toe te laten.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 0%

---


# CDN-caching inschakelen

Leer hoe te om het in cache plaatsen van de reacties van HTTP in AEM as a Cloud Service CDN toe te laten. Het in cache plaatsen van reacties wordt beheerd door `Cache-Control`, `Surrogate-Control`, of `Expires` HTTP response cache headers.

Deze cachekoppen worden doorgaans ingesteld in AEM Dispatcher-hostconfiguraties met `mod_headers`, maar kan ook worden ingesteld in aangepaste Java™-code die wordt uitgevoerd in AEM Publish zelf.

## Standaardgedrag voor caching

Wanneer aangepaste configuraties NIET aanwezig zijn, worden de standaardwaarden gebruikt. In de volgende schermafbeelding ziet u het standaardgedrag voor het in cache plaatsen van AEM Publiceren en Auteur wanneer een [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype) gebaseerd `mynewsite` AEM project wordt uitgevoerd.

![Standaardgedrag voor caching](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Controleer de [AEM publiceren - standaardlevensduur van cache](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) en [AEM auteur - Standaardlevensduur cache](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) voor meer informatie .

Samengevat worden de meeste inhoudstypen (HTML, JSON, JS, CSS en Elementen) in AEM as a Cloud Service cache opgeslagen in AEM Publiceren en enkele inhoudstypen (JS, CSS) in AEM Auteur.

## Opslaan in cache inschakelen

Als u het standaardgedrag voor het in cache plaatsen wilt wijzigen, kunt u de cachekoppen op twee manieren bijwerken.

1. **Hostconfiguratie van Dispatcher:** Alleen beschikbaar voor AEM publiceren.
1. **Aangepaste Java™-code:** Beschikbaar voor zowel AEM Publiceren als Auteur.

Laten we elk van deze opties bekijken.

### Dispatcher-hostconfiguratie

Deze optie is de aanbevolen methode voor het inschakelen van caching, maar is alleen beschikbaar voor AEM Publiceren. Als u de cachekoppen wilt bijwerken, gebruikt u de optie `mod_headers` en `<LocationMatch>` in het hostbestand van Apache HTTP Server. De algemene syntaxis ziet er als volgt uit:

    &quot;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    # Verwijdert de antwoordkopbal van deze naam, als het bestaat. Als er meerdere koppen met dezelfde naam zijn, worden alle koppen verwijderd.
    Cache-control voor header ongedaan maken
    Niet-ingestelde substitutiebesturing van koptekst
    Oningestelde koptekst verloopt
    
    # Instrueert Webbrowser en CDN om de reactie voor &quot;maximum-age&quot;waarde (XXX) seconden in het voorgeheugen onder te brengen. De kenmerken &#39;stale-while-revalidate&#39; en &#39;stale-if-error&#39; bepalen de manier waarop de staat van de schaal wordt verwerkt op de CDN-laag.
    Koptekenset Cache-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # instrueert CDN de reactie voor &quot;maximum-age&quot;waarde (XXX) seconden in het voorgeheugen onder te brengen. De kenmerken &#39;stale-while-revalidate&#39; en &#39;stale-if-error&#39; bepalen de manier waarop de staat van de schaal wordt verwerkt op de CDN-laag.
    Header-set Surrogate-control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # instrueert Webbrowser en CDN om de reactie tot de gespecificeerde datum en de tijd in het voorgeheugen onder te brengen.
    Koptekenset vervalt op 31 december 2023 23:59:59 GMT&quot;
    &lt;/locationmatch>
    &quot;

Hieronder wordt het doel van elk **header** en van toepassing **attributes** voor de koptekst.

|                     | Webbrowser | CDN | Beschrijving |
|---------------------|:-----------:|:---------:|:-----------:|
| Cachebeheer | ✔ | ✔ | Deze header bepaalt de webbrowser en de CDN-cache. |
| Surrogaatcontrole | ✘ | ✔ | Deze header bestuurt het CDN-cache-leven. |
| Verloopt | ✔ | ✔ | Deze header bepaalt de webbrowser en de CDN-cache. |


- **maximale leeftijd**: This attribute controls the TTL or &quot;time to live&quot; of the response content in seconds.
- **stale-while-revalidate**: This attribute controls the _staat van stijl_ behandeling van de reactieinhoud bij laag CDN wanneer het ontvangen verzoek binnen de gespecificeerde periode in seconden is. De _staat van stijl_ is de tijdsperiode nadat de TTL is verlopen en voordat de reactie opnieuw wordt gevalideerd.
- **stale-if-error**: This attribute controls the _staat van stijl_ behandeling van de reactieinhoud bij laag CDN wanneer de oorsprongsserver niet beschikbaar is en ontvangen verzoek binnen de gespecificeerde periode in seconden is.

Controleer de [elastheid en herbevestiging](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) voor meer informatie .

#### Voorbeeld

De webbrowser en de CDN-cache langer in het **HTML-inhoudstype** tot _10 minuten_ zonder behandeling van de staat van de schaal, volg deze stappen:

1. Zoek in uw AEM-project het gewenste vhsot-bestand vanuit `dispatcher/src/conf.d/available_vhosts` directory.
1. De vhost bijwerken (bijvoorbeeld `wknd.vhost`), als volgt:

       &quot;conf
       &lt;locationmatch content=&quot;&quot;>*\.(html)$&quot;>
       # Verwijdert de antwoordkopbal indien aanwezig
       Cache-control voor header ongedaan maken
       
       # instrueert Webbrowser en CDN om de reactie voor maximum-leeftijdswaarde (600) seconden in het voorgeheugen onder te brengen.
       Koptekstset Cache-control &quot;max-age=600&quot;
       &lt;/locationmatch>
       &quot;
   De hostbestanden in `dispatcher/src/conf.d/enabled_vhosts` directory is **symlinks** naar de bestanden in `dispatcher/src/conf.d/available_vhosts` , zorg er dus voor dat u symlinks maakt als deze niet aanwezig zijn.
1. Implementeer de hostwijzigingen in de gewenste AEM as a Cloud Service omgeving met de [Cloud Manager - Web Tier Config Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) of [RDE-opdrachten](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Als u echter verschillende waarden wilt hebben voor de levensduur van webbrowsers en CDN-cache, kunt u de opdracht `Surrogate-Control` in het bovenstaande voorbeeld. Evenzo kunt u de cache op een bepaalde datum en tijd laten verlopen met de `Expires` header. Gebruik ook de opdracht `stale-while-revalidate` en `stale-if-error` kenmerken, kunt u de manier bepalen waarop de responsinhoud met de status &#39;stale&#39; wordt behandeld. Het AEM WKND-project heeft een [behandeling van de status van referentiestand](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) CDN-cacheconfiguratie.

Op dezelfde manier kunt u de cachekoppen ook bijwerken voor andere inhoudstypen (JSON, JS, CSS en Middelen).

### Aangepaste Java™-code

Deze optie is beschikbaar voor zowel AEM Publiceren als Auteur. Het wordt echter afgeraden caching in AEM Auteur in te schakelen en het standaardgedrag voor caching te behouden.

Als u de cachekoppen wilt bijwerken, gebruikt u de optie `HttpServletResponse` -object in aangepaste Java™-code (Sling servlet, Sling servlet-filter). De algemene syntaxis ziet er als volgt uit:

    &quot;java
    // Instrueert de webbrowser en CDN de reactie voor de waarde &#39;max-age&#39; (XXX) seconden in cache op te slaan. De kenmerken &#39;stale-while-revalidate&#39; en &#39;stale-if-error&#39; bepalen de manier waarop de staat van de schaal wordt verwerkt op de CDN-laag.
    response.setHeader(&quot;Cache-Control&quot;, &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;);
    
    // Instrueert de CDN om de reactie voor de waarde XXX (max-age) seconden in cache op te slaan. De kenmerken &#39;stale-while-revalidate&#39; en &#39;stale-if-error&#39; bepalen de manier waarop de staat van de schaal wordt verwerkt op de CDN-laag.
    response.setHeader(&quot;Surrogate-Control&quot;, &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;);
    
    // Instrueert de webbrowser en CDN de reactie in cache op te slaan tot de opgegeven datum en tijd.
    response.setHeader(&quot;Expires&quot;, &quot;Sun, 31 dec 2023 23:59:59 GMT&quot;);
    &quot;
