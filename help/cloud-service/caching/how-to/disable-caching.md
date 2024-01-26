---
title: CDN in cache plaatsen uitschakelen
description: Leer hoe te om het in het voorgeheugen onderbrengen van de reacties van HTTP in AEM as a Cloud Service CDN onbruikbaar te maken.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 116
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# CDN in cache plaatsen uitschakelen

Leer hoe te om het in het voorgeheugen onderbrengen van de reacties van HTTP in AEM as a Cloud Service CDN onbruikbaar te maken. Het in cache plaatsen van reacties wordt beheerd door `Cache-Control`, `Surrogate-Control`, of `Expires` HTTP response cache headers.

Deze cachekoppen worden doorgaans ingesteld in AEM Dispatcher-hostconfiguraties met `mod_headers`, maar kan ook worden ingesteld in aangepaste Java™-code die wordt uitgevoerd in AEM Publish zelf.

## Standaardgedrag voor caching

Herzie het standaardcaching gedrag voor AEM Publish en Auteur wanneer een [Projectarchetype AEM](./enable-caching.md#default-caching-behavior) gebaseerd AEM project wordt opgesteld.

## Cache uitschakelen

Het uitschakelen van caching kan een negatief effect hebben op de prestaties van de AEM as a Cloud Service instantie. Wees daarom voorzichtig bij het uitschakelen van het standaardcachegedrag.

Er zijn echter enkele scenario&#39;s waarin u caching kunt uitschakelen, zoals:

- Een nieuwe functie ontwikkelen en de wijzigingen direct bekijken.
- Inhoud is beveiligd (alleen bedoeld voor geverifieerde gebruikers) of dynamisch (winkelwagentje, bestelgegevens) en mag niet in de cache worden opgeslagen.

Als u caching wilt uitschakelen, kunt u de cachekoppen op twee manieren bijwerken.

1. **Hostconfiguratie van Dispatcher:** Alleen beschikbaar voor AEM publiceren.
1. **Aangepaste Java™-code:** Beschikbaar voor zowel AEM Publiceren als Auteur.

Laten we elk van deze opties bekijken.

### Dispatcher-hostconfiguratie

Deze optie is de aanbevolen aanpak voor het uitschakelen van caching, maar is alleen beschikbaar voor AEM Publiceren. Als u de cachekoppen wilt bijwerken, gebruikt u de optie `mod_headers` en `<LocationMatch>` in het hostbestand van Apache HTTP Server. De algemene syntaxis ziet er als volgt uit:

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### Voorbeeld

Als u de CDN-caching van het dialoogvenster **CSS-inhoudssoorten** voor sommige het oplossen van problemendoeleinden, volg deze stappen.

Als u de bestaande CSS-cache wilt overslaan, moet u het CSS-bestand wijzigen om een nieuwe cachemoets voor het CSS-bestand te genereren.

1. Zoek in uw AEM-project het gewenste vhsot-bestand vanuit `dispatcher/src/conf.d/available_vhosts` directory.
1. De vhost bijwerken (bijvoorbeeld `wknd.vhost`), als volgt:

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   De hostbestanden in `dispatcher/src/conf.d/enabled_vhosts` directory is **symlinks** naar de bestanden in `dispatcher/src/conf.d/available_vhosts` , zorg er dus voor dat u symlinks maakt als deze niet aanwezig zijn.
1. Implementeer de hostwijzigingen in de gewenste AEM as a Cloud Service omgeving met de [Cloud Manager - Web Tier Config Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) of [RDE-opdrachten](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### Aangepaste Java™-code

Deze optie is beschikbaar voor zowel AEM Publiceren als Auteur. Als u de cachekoppen wilt bijwerken, gebruikt u de optie `SlingHttpServletResponse` -object in aangepaste Java™-code (Sling servlet, Sling servlet-filter). De algemene syntaxis ziet er als volgt uit:

```java
response.setHeader("Cache-Control", "private");
```
