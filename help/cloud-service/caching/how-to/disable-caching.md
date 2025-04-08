---
title: CDN in cache plaatsen uitschakelen
description: Leer hoe u het in cache plaatsen van HTTP-reacties in AEM as a Cloud Service CDN uitschakelt.
version: Experience Manager as a Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 100
source-git-commit: cf006f24abbc5aa4b91277b91d68538c41d33e15
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# CDN in cache plaatsen uitschakelen

Leer hoe u het in cache plaatsen van HTTP-reacties in AEM as a Cloud Service CDN uitschakelt. Het in cache plaatsen van reacties wordt geregeld door `Cache-Control` -, `Surrogate-Control` - of `Expires` HTTP-antwoordcache-headers.

Deze cacheheaders worden doorgaans ingesteld in AEM Dispatcher-hostconfiguraties met `mod_headers` , maar kunnen ook worden ingesteld in aangepaste Java™-code die wordt uitgevoerd in AEM Publish zelf.

## Standaardgedrag voor caching

Het in cache plaatsen van HTTP-reacties in de CDN van AEM as a Cloud Service wordt bestuurd door de volgende HTTP-antwoordheaders vanaf de oorsprong `Cache-Control`, `Surrogate-Control` of `Expires` .  Oorspronkelijke reacties die `private` , `no-cache` of `no-store` in `Cache-Control` bevatten, worden niet in de cache geplaatst.

Herzie het [ standaardcaching gedrag ](./enable-caching.md#default-caching-behavior) voor AEM publiceren en Auteur wanneer een project van de Archetype van het Project van AEM gebaseerd AEM wordt opgesteld.


## Cache uitschakelen

Als u caching uitschakelt, kan dit negatieve gevolgen hebben voor de prestaties van uw AEM as a Cloud Service-instantie. Wees daarom voorzichtig bij het uitschakelen van het standaardcachegedrag.

Er zijn echter enkele scenario&#39;s waarin u caching kunt uitschakelen, zoals:

- Een nieuwe functie ontwikkelen en de wijzigingen direct bekijken.
- Inhoud is beveiligd (alleen bedoeld voor geverifieerde gebruikers) of dynamisch (winkelwagentje, bestelgegevens) en mag niet in de cache worden opgeslagen.

Als u caching wilt uitschakelen, kunt u de cachekoppen op twee manieren bijwerken.

1. **de gastheerconfiguratie van Dispatcher:** slechts beschikbaar voor AEM publiceert.
1. **de code van Java™ van de Douane:** Beschikbaar voor zowel de Publish als Auteur van AEM.

Laten we elk van deze opties bekijken.

### Dispatcher-hostconfiguratie

Deze optie wordt aanbevolen voor het uitschakelen van caching, maar is alleen beschikbaar voor AEM Publish. Als u de cachekoppen wilt bijwerken, gebruikt u de instructies `mod_headers` module en `<LocationMatch>` in het hostbestand van Apache HTTP Server. De algemene syntaxis ziet er als volgt uit:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the Browser and the CDN to not cache the response.
    Header always set Cache-Control "private"

    # Instructs only the CDN to not cache the response.
    Header always set Surrogate-Control "private"
</LocationMatch>
```

#### Voorbeeld

Om CDN caching van de **CSS inhoudstypes** voor sommige het oplossen van problemendoeleinden onbruikbaar te maken, volg deze stappen.

Als u de bestaande CSS-cache wilt overslaan, moet u het CSS-bestand wijzigen om een nieuwe cachemoets voor het CSS-bestand te genereren.

1. Zoek in uw AEM-project het gewenste vhst-bestand in de map `dispatcher/src/conf.d/available_vhosts` .
1. Werk het vhost-bestand (bijvoorbeeld `wknd.vhost` ) als volgt bij:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the Browser and the CDN to not cache the response.
       Header always set Cache-Control "private"
   </LocationMatch>
   ```

   De gastheerdossiers in `dispatcher/src/conf.d/enabled_vhosts` folder zijn **symlinks** aan de dossiers in `dispatcher/src/conf.d/available_vhosts` folder, zodat zorg ervoor om tot symlinks te leiden als niet aanwezig.
1. Stel de vhost veranderingen in het gewenste milieu van AEM as a Cloud Service op gebruikend [ Cloud Manager - de Pijpleiding van Config van de Rij van het Web ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) of [ RDE bevelen ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Aangepaste Java™-code

Deze optie is zowel beschikbaar voor publiceren in AEM als voor auteur. Gebruik het `SlingHttpServletResponse` -object in de aangepaste Java™-code (Sling servlet, Sling servlet filter) om de cacheheaders bij te werken. De algemene syntaxis ziet er als volgt uit:

```java
response.setHeader("Cache-Control", "private");
```
