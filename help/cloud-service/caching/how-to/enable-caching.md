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
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 200
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '637'
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

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

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

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   De hostbestanden in `dispatcher/src/conf.d/enabled_vhosts` directory is **symlinks** naar de bestanden in `dispatcher/src/conf.d/available_vhosts` , zorg er dus voor dat u symlinks maakt als deze niet aanwezig zijn.
1. Implementeer de hostwijzigingen in de gewenste AEM as a Cloud Service omgeving met de [Cloud Manager - Web Tier Config Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) of [RDE-opdrachten](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Als u echter verschillende waarden wilt hebben voor de levensduur van webbrowsers en CDN-cache, kunt u de opdracht `Surrogate-Control` in het bovenstaande voorbeeld. Evenzo kunt u de cache op een bepaalde datum en tijd laten verlopen met de `Expires` header. Gebruik ook de opdracht `stale-while-revalidate` en `stale-if-error` kenmerken, kunt u de manier bepalen waarop de responsinhoud met de status &#39;stale&#39; wordt behandeld. Het AEM WKND-project heeft een [behandeling van de status van referentiestand](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) CDN-cacheconfiguratie.

Op dezelfde manier kunt u de cachekoppen ook bijwerken voor andere inhoudstypen (JSON, JS, CSS en Middelen).

### Aangepaste Java™-code

Deze optie is beschikbaar voor zowel AEM Publiceren als Auteur. Het wordt echter afgeraden caching in AEM Auteur in te schakelen en het standaardgedrag voor caching te behouden.

Als u de cachekoppen wilt bijwerken, gebruikt u de optie `HttpServletResponse` -object in aangepaste Java™-code (Sling servlet, Sling servlet-filter). De algemene syntaxis ziet er als volgt uit:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
