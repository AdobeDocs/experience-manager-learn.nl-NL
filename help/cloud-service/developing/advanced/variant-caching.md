---
title: Paginavarianten in cache plaatsen met AEM as a Cloud Service
description: Leer hoe u AEM instelt en gebruikt als cloudservice voor het ondersteunen van paginamarges.
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
duration: 149
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 0%

---

# Paginariabelen in cache plaatsen

Leer hoe u AEM instelt en gebruikt als cloudservice voor het ondersteunen van paginamarges.

## Voorbeelden van gebruiksgevallen

+ Om het even welke dienstverlener die een verschillende reeks dienstenaanbod en overeenkomstige prijsstellingsopties aanbiedt die op de geo plaats van de gebruiker en het geheime voorgeheugen van pagina&#39;s met dynamische inhoud worden gebaseerd zou bij CDN en Dispatcher moeten worden beheerd.

+ Een detailhandelaar heeft winkels over het land en elke opslag heeft verschillende aanbiedingen gebaseerd op waar zij worden gevestigd en het geheime voorgeheugen van pagina&#39;s met dynamische inhoud zou bij CDN en Dispatcher moeten worden beheerd.

## Overzicht van oplossing

+ Identificeer de variantsleutel en het aantal waarden het kan hebben. In ons voorbeeld variëren we per staat in de VS, dus het maximale aantal is 50. Dit is klein genoeg om geen problemen met de variantgrenzen bij CDN te veroorzaken. [ sectie van de de variantbeperkingen van het Overzicht {](#variant-limitations).

+ AEM code moet het koekje __&quot;x-aem-variant&quot;__ aan de aangewezen staat van de bezoeker (b.v. `Set-Cookie: x-aem-variant=NY` ) op de initiële HTTP-aanvraag die overeenkomt met de HTTP-respons.

+ Volgende aanvragen van de bezoeker verzenden dat cookie (bijvoorbeeld `"Cookie: x-aem-variant=NY"`) en de cookie wordt op CDN-niveau omgezet in een vooraf gedefinieerde header (d.w.z. `x-aem-variant:NY` ) die wordt doorgegeven aan de dispatcher.

+ Een Apache-regel voor herschrijven wijzigt het aanvraagpad zodat de koptekstwaarde in de pagina-URL wordt opgenomen als een Apache Sling-kiezer (bijvoorbeeld `/page.variant=NY.html`). Hierdoor kan AEM Publish verschillende inhoud leveren op basis van de kiezer en de verzender één pagina per variant in cache plaatsen.

+ De reactie die door AEM Dispatcher wordt verzonden, moet een HTTP-antwoordheader `Vary: x-aem-variant` bevatten. Dit instrueert CDN om verschillende geheim voorgeheugenexemplaren voor verschillende kopbalwaarden op te slaan.

>[!TIP]
>
>Wanneer een cookie wordt ingesteld (bijvoorbeeld Set-Cookie: x-aem-variant=NY) de reactie zou niet cacheable moeten zijn (zou Cachecontrole moeten hebben: privé of Cachecontrole: no-cache)

## HTTP-aanvraagstroom

![ de Stroom van het Verzoek van het Geheime voorgeheugen van de Variant ](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>De eerste HTTP-aanvraagstroom hierboven moet plaatsvinden voordat inhoud wordt aangevraagd die varianten gebruikt.

## Gebruik

1. Om de eigenschap aan te tonen, zullen wij [ implementatie van WKND ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) als voorbeeld gebruiken.

1. Voer a [ SlingServletFilter ](https://sling.apache.org/documentation/the-sling-engine/filters.html) in AEM uit om `x-aem-variant` koekje op de reactie van HTTP, met een variantwaarde te plaatsen.

1. Als u CDN AEM, wordt de `x-aem-variant` -cookie automatisch getransformeerd naar een HTTP-header met dezelfde naam.

1. Voeg een Apache de servermod_rewrite regel van het Web aan uw `dispatcher` project toe, die het verzoekweg wijzigt om de variantselecteur te omvatten.

1. Implementeer het filter en herschrijf de regels met Cloud Manager.

1. Test de algemene aanvraagstroom.

## Codevoorbeelden

+ Voorbeeld van SlingServletFilter om `x-aem-variant` cookie in te stellen met een waarde in AEM.

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

+ De steekproef herschrijft regel in het __dispatcher/src/conf.d/rewrite.rules__ dossier dat als broncode in Git wordt beheerd, en opgesteld gebruikend Cloud Manager.

  ```
  ...
  
  RewriteCond %{REQUEST_URI} ^/us/.*  
  RewriteCond %{HTTP:x-aem-variant} ^.*$  
  RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
  
  ...
  ```

## Variantbeperkingen

+ AEM CDN kan maximaal 200 variaties beheren. Dat betekent dat de header van `x-aem-variant` maximaal 200 unieke waarden kan hebben. Voor meer informatie, herzie de [ CDN configuratiegrenzen ](https://docs.fastly.com/en/guides/resource-limits).

+ Zorg ervoor dat de gekozen variantsleutel dit aantal nooit overschrijdt.  Een gebruikers-id is bijvoorbeeld geen goede sleutel omdat deze voor de meeste websites gemakkelijk 200 waarden overschrijdt, terwijl de staten/gebieden in een land beter geschikt zijn als er minder dan 200 staten in dat land zijn.

>[!NOTE]
>
>Als de varianten groter zijn dan 200, reageert de CDN met de reactie &quot;Te veel varianten&quot; in plaats van met de pagina-inhoud.
