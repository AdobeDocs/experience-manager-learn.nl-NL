---
title: Paginavarianten in cache plaatsen met AEM as a Cloud Service
description: Leer hoe u AEM instelt en gebruikt als cloudservice voor het ondersteunen van paginamarges.
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%

---

# Paginariabelen in cache plaatsen

Leer hoe u AEM instelt en gebruikt als cloudservice voor het ondersteunen van paginamarges.

## Voorbeelden van gebruiksgevallen

+ Om het even welke dienstverlener die een verschillende reeks dienstenaanbod en overeenkomstige prijsstellingsopties aanbiedt die op de geo plaats van de gebruiker en het geheime voorgeheugen van pagina&#39;s met dynamische inhoud worden gebaseerd zou bij CDN en Verzender moeten worden beheerd.

+ Een detailhandelaar heeft opslag over het land en elke opslag heeft verschillende aanbiedingen gebaseerd op waar zij worden gevestigd en het geheime voorgeheugen van pagina&#39;s met dynamische inhoud zou bij CDN en Dispatcher moeten worden beheerd.

## Overzicht van oplossing

+ Identificeer de variantsleutel en het aantal waarden het kan hebben. In ons voorbeeld variëren we per staat in de VS, dus het maximale aantal is 50. Dit is klein genoeg om geen problemen met de variantgrenzen bij CDN te veroorzaken. [Sectie met beperkingen voor variant controleren](#variant-limitations).

+ AEM code moet het cookie instellen __&quot;x-aem-variant&quot;__ naar de voorkeursstatus van de bezoeker (bijv. `Set-Cookie: x-aem-variant=NY`) over de overeenkomstige HTTP-respons van de oorspronkelijke HTTP-aanvraag.

+ Volgende aanvragen van de bezoeker verzenden dat cookie (bijv. `"Cookie: x-aem-variant=NY"`) en wordt het cookie op CDN-niveau omgezet in een vooraf gedefinieerde header (d.w.z. `x-aem-variant:NY`) die aan de verzender wordt doorgegeven.

+ Een Apache-regel voor herschrijven wijzigt het aanvraagpad zodat de koptekstwaarde in de pagina-URL wordt opgenomen als een Apache Sling-kiezer (bijvoorbeeld `/page.variant=NY.html`). Hierdoor kan AEM-publicatie verschillende inhoud leveren op basis van de kiezer en kan de verzender één pagina per variant in cache plaatsen.

+ De reactie die wordt verzonden door AEM Dispatcher moet een HTTP-antwoordheader bevatten `Vary: x-aem-variant`. Dit instrueert CDN om verschillende geheim voorgeheugenexemplaren voor verschillende kopbalwaarden op te slaan.

>[!TIP]
>
>Wanneer een cookie wordt ingesteld (bijvoorbeeld Set-Cookie: x-aem-variant=NY) de reactie zou niet cacheable moeten zijn (zou Cachebeheer moeten hebben: private of cache-control: no-cache)

## HTTP-aanvraagstroom

![Variant cache-aanvraagstroom](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>De eerste HTTP-aanvraagstroom hierboven moet plaatsvinden voordat inhoud wordt aangevraagd die varianten gebruikt.

## Gebruik

1. Om de functie te demonstreren, gebruiken we [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)De implementatie als voorbeeld.

1. Een [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) in AEM in te stellen `x-aem-variant` cookie op de HTTP-respons, met een variantwaarde.

1. CDN wordt automatisch getransformeerd AEM `x-aem-variant` cookie naar een HTTP-header met dezelfde naam.

1. Voeg een Apache-webservermod_rewrite-regel toe aan uw `dispatcher` project, dat de verzoekweg wijzigt om de variantselecteur te omvatten.

1. Implementeer het filter en herschrijf de regels met gebruik van Cloud Manager.

1. Test de algemene aanvraagstroom.

## Codevoorbeelden

+ Monster nemen van SlingServletFilter `x-aem-variant` cookie met een waarde in AEM.

   ```
   package com.adobe.aem.guides.wknd.core.servlets.filters;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.api.SlingHttpServletResponse;
   import org.apache.sling.servlets.annotations.SlingServletFilter;
   import org.apache.sling.servlets.annotations.SlingServletFilterScope;
   import org.osgi.service.component.annotations.Component;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   
   // Invoke filter on  HTTP GET /content/wknd.*.foo|bar.html|json requests.
   // This code and scope is for example purposes only, and will not interfere with other requests.
   @Component
   @SlingServletFilter(scope = {SlingServletFilterScope.REQUEST},
           resourceTypes = {"cq:Page"},
           pattern = "/content/wknd/.*",
           extensions = {"html", "json"},
           methods = {"GET"})
   public class PageVariantFilter implements Filter {
       private static final Logger log = LoggerFactory.getLogger(PageVariantFilter.class);
       private static final String VARIANT_COOKIE_NAME = "x-aem-variant";
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException { }
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           SlingHttpServletResponse slingResponse = (SlingHttpServletResponse) servletResponse;
           SlingHttpServletRequest slingRequest = (SlingHttpServletRequest) servletRequest;
   
           // Check is the variant was previously set
           final String existingVariant = slingRequest.getCookie(VARIANT_COOKIE_NAME).getValue();
   
           if (existingVariant == null) {
               // Variant has not been set, so set it now
               String newVariant = "NY"; // Hard coding as an example, but should be a calculated value
               slingResponse.setHeader("Set-Cookie", VARIANT_COOKIE_NAME + "=" + newVariant + "; Path=/; HttpOnly; Secure; SameSite=Strict");
               log.debug("x-aem-variant cookie is set with the value {}", newVariant);
           } else {
               log.debug("x-aem-variant previously set with value {}", existingVariant);
           }
   
           filterChain.doFilter(servletRequest, slingResponse);
       }
   
       @Override
       public void destroy() { }
   }
   ```

+ Voorbeeld van herschrijfregel in het dialoogvenster __dispatcher/src/conf.d/rewrite.rules__ bestand dat wordt beheerd als broncode in Git en wordt geïmplementeerd met Cloud Manager.

   ```
   ...
   
   RewriteCond %{REQUEST_URI} ^/us/.*  
   RewriteCond %{HTTP:x-aem-variant} ^.*$  
   RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
   
   ...
   ```

## Beperkingen van varianten

+ AEM CDN kan maximaal 200 variaties beheren. Dat betekent dat `x-aem-variant` header kan maximaal 200 unieke waarden hebben . Voor meer informatie raadpleegt u de [CDN-configuratielimieten](https://docs.fastly.com/en/guides/resource-limits).

+ Zorg ervoor dat de gekozen variantsleutel dit aantal nooit overschrijdt.  Een gebruikers-id is bijvoorbeeld geen goede sleutel omdat deze voor de meeste websites gemakkelijk 200 waarden overschrijdt, terwijl de staten/gebieden in een land beter geschikt zijn als er minder dan 200 staten in dat land zijn.

>[!NOTE]
>
>Als de varianten groter zijn dan 200, reageert de CDN met de reactie &quot;Te veel varianten&quot; in plaats van met de pagina-inhoud.
