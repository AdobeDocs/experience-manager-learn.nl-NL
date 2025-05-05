---
title: CDN-caching inschakelen
description: Leer hoe u het in cache plaatsen van HTTP-reacties in AEM as a Cloud Service CDN inschakelt.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 174
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# CDN-caching inschakelen

Leer hoe u het in cache plaatsen van HTTP-reacties in AEM as a Cloud Service CDN inschakelt. Het in cache plaatsen van reacties wordt geregeld door `Cache-Control` -, `Surrogate-Control` - of `Expires` HTTP-antwoordcache-headers.

Deze cacheheaders worden doorgaans ingesteld in AEM Dispatcher-hostconfiguraties met `mod_headers` , maar kunnen ook worden ingesteld in aangepaste Java™-code die wordt uitgevoerd in AEM Publish zelf.

## Standaardgedrag voor caching

Wanneer aangepaste configuraties NIET aanwezig zijn, worden de standaardwaarden gebruikt. In het volgende schermafbeelding, kunt u het standaardcaching gedrag voor AEM zien publiceren en Auteur wanneer een [&#128279;](https://github.com/adobe/aem-project-archetype) gebaseerd `mynewsite` AEM-project van de Archetype van het Project van AEM  wordt opgesteld.

![ Standaard caching gedrag ](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

Herzie [ AEM publiceren - Het leven van het standaardgeheime voorgeheugen ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) en [ Auteur van AEM - Het leven van het standaardgeheime voorgeheugen ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) voor meer informatie.

Samengevat plaatst AEM as a Cloud Service de meeste inhoudstypen (HTML, JSON, JS, CSS en Assets) in AEM Publish en een aantal inhoudstypen (JS, CSS) in AEM Author.

## Opslaan in cache inschakelen

Als u het standaardgedrag voor het in cache plaatsen wilt wijzigen, kunt u de cachekoppen op twee manieren bijwerken.

1. **de gastheerconfiguratie van Dispatcher:** slechts beschikbaar voor AEM publiceert.
1. **de code van Java™ van de Douane:** Beschikbaar voor zowel de Publish als Auteur van AEM.

Laten we elk van deze opties bekijken.

### Dispatcher-hostconfiguratie

Deze optie is de aanbevolen methode voor het inschakelen van caching, maar is alleen beschikbaar voor publicatie in AEM. Als u de cachekoppen wilt bijwerken, gebruikt u de instructies `mod_headers` module en `<LocationMatch>` in het hostbestand van Apache HTTP Server. De algemene syntaxis ziet er als volgt uit:

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

Het volgende vat het doel van elke **kopbal** en toepasselijke **attributen** voor de kopbal samen.

|                     | Webbrowser | CDN | Beschrijving |
|---------------------|:-----------:|:---------:|:-----------:|
| Cachebeheer | ✔ | ✔ | Deze header bepaalt de webbrowser en de CDN-cache. |
| Surrogaatcontrole | ✘ | ✔ | Deze header bestuurt het CDN-cache-leven. |
| Verloopt | ✔ | ✔ | Deze header bepaalt de webbrowser en de CDN-cache. |


- **max-age**: Dit attribuut controleert TTL of &quot;tijd om&quot;van de reactieinhoud in seconden te leven.
- **schaal-terwijl-revalidate**: Dit attribuut controleert de _stapelstaat_ behandeling van de antwoordinhoud bij CDN laag wanneer ontvangen verzoek binnen de gespecificeerde periode in seconden is. De _stapelstaat_ is de tijdperiode nadat TTL is verlopen en alvorens de reactie opnieuw wordt bevestigd.
- **schaal-als-fout**: Dit attribuut controleert de _stapelstaat_ behandeling van de antwoordinhoud bij CDN laag wanneer de oorsprongsserver niet beschikbaar is en ontvangen verzoek binnen de gespecificeerde periode in seconden is.

Herzie de [ staleness en herbevestiging ](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) details voor meer informatie.

#### Voorbeeld

Om het Webbrowser en CDN geheim voorgeheugenleven van het **inhoudstype van HTML** aan _10 minuten_ zonder de behandeling van de stapelstaat te verhogen, volg deze stappen:

1. Zoek in uw AEM-project het gewenste vhst-bestand in de map `dispatcher/src/conf.d/available_vhosts` .
1. Werk het vhost-bestand (bijvoorbeeld `wknd.vhost` ) als volgt bij:

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   De gastheerdossiers in `dispatcher/src/conf.d/enabled_vhosts` folder zijn **symlinks** aan de dossiers in `dispatcher/src/conf.d/available_vhosts` folder, zodat zorg ervoor om tot symlinks te leiden als niet aanwezig.
1. Stel de vhost veranderingen in het gewenste milieu van AEM as a Cloud Service op gebruikend [ Cloud Manager - de Pijpleiding van Config van de Rij van het Web ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) of [ RDE bevelen ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

Als u echter verschillende waarden wilt hebben voor de webbrowser en de levensduur van de CDN-cache, kunt u de header `Surrogate-Control` in het bovenstaande voorbeeld gebruiken. Op dezelfde manier kunt u de header `Expires` gebruiken om de cache op een bepaalde datum en tijd te laten verlopen. Met de kenmerken `stale-while-revalidate` en `stale-if-error` kunt u ook de manier bepalen waarop de status van de reactie-inhoud in de schaal wordt verwerkt. Het project van AEM WKND heeft de de staatsbehandeling van de a [ verwijzingssteen ](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) CDN geheim voorgeheugenconfiguratie.

Op dezelfde manier kunt u de cachekoppen ook bijwerken voor andere inhoudstypen (JSON, JS, CSS en Assets).

### Aangepaste Java™-code

Deze optie is zowel beschikbaar voor publiceren in AEM als voor auteur. Het wordt echter afgeraden caching in te schakelen in AEM Author en het standaardgedrag voor caching te behouden.

Gebruik het `HttpServletResponse` -object in de aangepaste Java™-code (Sling servlet, Sling servlet filter) om de cacheheaders bij te werken. De algemene syntaxis ziet er als volgt uit:

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
