---
title: AEM-publicatieservice in cache plaatsen
description: Algemeen overzicht van het in cache plaatsen van de AEM as a Cloud Service-publicatieservice.
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: 1a1accbe-7706-4f9b-bf63-755090d03c4c
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 0%

---

# AEM Publiceren

De AEM-publicatieservice heeft twee primaire cachelagen: de AEM as a Cloud Service CDN en AEM Dispatcher. Naar keuze kan een klant beheerde CDN vóór AEM as a Cloud Service CDN worden geplaatst. De AEM as a Cloud Service CDN biedt randlevering van inhoud en zorgt ervoor dat gebruikers over de hele wereld met een lage latentie van ervaringen worden voorzien. AEM Dispatcher biedt caching direct vóór AEM Publish en wordt gebruikt om onnodige belasting op AEM Publish zelf te beperken.

![&#x200B; AEM publiceert het caching overzichtsdiagram &#x200B;](./assets/publish/publish-all.png){align="center"}

## CDN

Het in cache plaatsen van CDN voor AEM as a Cloud Service wordt beheerd door HTTP response cache headers en is bedoeld om inhoud in cache te plaatsen om de balans tussen versheid en prestaties te optimaliseren. De CDN bevindt zich tussen de eindgebruiker en de AEM Dispatcher, en wordt gebruikt om inhoud zo dicht mogelijk bij de eindgebruiker in cache te plaatsen, zodat de prestaties gegarandeerd zijn.

![&#x200B; AEM publiceert CDN &#x200B;](./assets/publish/publish-cdn.png){align="center"}

Het vormen hoe CDN inhoud in het voorgeheugen onderbrengt is beperkt tot het plaatsen van geheim voorgeheugenkopballen op de reacties van HTTP. Deze cacheheaders worden doorgaans ingesteld in AEM Dispatcher-hostconfiguraties met `mod_headers` , maar kunnen ook worden ingesteld in aangepaste Java™-code die wordt uitgevoerd in AEM Publish zelf.

### Wanneer worden HTTP-aanvragen/reacties in cache geplaatst?

AEM as a Cloud Service CDN plaatst alleen HTTP-reacties in cache en aan alle volgende criteria moet worden voldaan:

+ De status van de HTTP-reactie is `2xx` of `3xx`
+ HTTP-aanvraagmethode is `GET` of `HEAD`
+ Ten minste een van de volgende HTTP-responsheaders is aanwezig: `Cache-Control`, `Surrogate-Control` of `Expires`
+ De HTTP-reactie kan elk inhoudstype zijn, zoals HTML, JSON, CSS, JS en binaire bestanden.

Door gebrek, hebben de reacties van HTTP niet in het voorgeheugen ondergebracht door [&#x200B; AEM Dispatcher &#x200B;](#aem-dispatcher) automatisch om het even welke kopballen van het de reactiecache van HTTP verwijderd om caching bij CDN te vermijden. Dit gedrag kan indien nodig zorgvuldig worden overschreven met `mod_headers` met de aanwijzing `Header always set ...` .

### Wat is in cache geplaatst?

AEM as a Cloud Service CDN slaat het volgende in cache op:

+ HTTP-responsinstantie
+ HTTP-antwoordheaders

Een HTTP-aanvraag/reactie voor één URL wordt doorgaans als één object in de cache opgeslagen. De CDN kan echter meerdere objecten voor één URL in cache plaatsen wanneer de header `Vary` wordt ingesteld op het HTTP-antwoord. Vermijd het opgeven van `Vary` op headers waarvan de waarden geen strak gecontroleerde reeks waarden hebben, aangezien dit kan leiden tot veel cachevervallen, waardoor de hit-verhouding van de cache afneemt. Om caching van variërende verzoeken bij AEM Dispatcher te steunen, [&#x200B; herzie de variant caching documentatie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html?lang=nl-NL).

### Levensduur cache{#cdn-cache-life}

De AEM Publish CDN is op TTL (tijd-aan-levende) gebaseerd, betekenend dat het geheim voorgeheugenleven door `Cache-Control` wordt bepaald, `Surrogate-Control`, of `Expires` HTTP- reactiekoppen. Als de reactie van HTTP in het voorgeheugen onderbrengende kopballen niet door het project worden geplaatst, en de [&#x200B; geschiktheidscriteria &#x200B;](#when-are-http-requestsresponses-cached) worden voldaan, plaatst Adobe een standaardgeheim voorgeheugenleven is 10 minuten (600 seconden).

Hier is hoe de geheim voorgeheugenkopballen het CDN geheim voorgeheugenleven beïnvloeden:

+ [`Cache-Control` &#x200B;](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) HTTP- antwoordkopbal instrueert Webbrowser en CDN hoe lang om de reactie in het voorgeheugen onder te brengen. De waarde is in seconden. `Cache-Control: max-age=3600` geeft de webbrowser bijvoorbeeld de opdracht de reactie gedurende een uur in cache te plaatsen. Deze waarde wordt genegeerd door de CDN als `Surrogate-Control` HTTP response header ook aanwezig is.
+ [`Surrogate-Control` &#x200B;](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) HTTP-antwoordheader instrueert de AEM CDN hoe lang de reactie in cache moet worden geplaatst. De waarde is in seconden. `Surrogate-Control: max-age=3600` geeft de CDN bijvoorbeeld de opdracht de reactie gedurende een uur in cache te plaatsen.
+ [`Expires` &#x200B;](https://developer.fastly.com/reference/http/http-headers/Expires/) HTTP response header instrueert de AEM CDN (en de webbrowser) hoe lang de reactie in de cache geldig is. De waarde is een datum. `Expires: Sat, 16 Sept 2023 09:00:00 EST` geeft de webbrowser bijvoorbeeld de opdracht om de reactie in cache te plaatsen tot de opgegeven datum en tijd.

Gebruik `Cache-Control` om de levensduur van de cache te bepalen wanneer deze hetzelfde is voor zowel de browser als de CDN. Gebruik `Surrogate-Control` wanneer de webbrowser de reactie voor een andere duur in cache moet plaatsen dan de CDN.

#### Standaardcache-levensduur

Als een reactie van HTTP voor AEM Dispatcher in het voorgeheugen onderbrengend [&#x200B; per bovengenoemde bepalers &#x200B;](#when-are-http-requestsresponses-cached) kwalificeert, zijn het volgende de standaardwaarden tenzij de douaneconfiguratie aanwezig is.

| Inhoudstype | Standaardlevensduur van CDN-cache |
|:------------ |:---------- |
| [&#x200B; HTML/JSON/XML &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#html-text) | 5 minuten |
| [&#x200B; Assets (beelden, video&#39;s, documenten, etc.) &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#images) | 10 minuten |
| [&#x200B; Blijven vragen (JSON) &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=nl-NL&publish-instances) | 2 uur |
| [&#x200B; de bibliotheken van de Cliënt (JS/CSS) &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#client-side-libraries) | dertig dagen |
| [&#x200B; Andere &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#other-content) | Niet in cache geplaatst |

### Cacheregels aanpassen

[&#x200B; Vormend hoe CDN inhoud &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#disp) in het voorgeheugen onderbrengt beperkt is tot het plaatsen van geheim voorgeheugenkopballen op de reacties van HTTP. Deze cacheheaders worden doorgaans ingesteld in AEM Dispatcher `vhost` -configuraties met `mod_headers` , maar kunnen ook worden ingesteld in aangepaste Java™-code die wordt uitgevoerd in AEM Publish zelf.

## AEM Dispatcher

![&#x200B; AEM publiceert AEM Dispatcher &#x200B;](./assets/publish/publish-dispatcher.png){align="center"}

### Wanneer worden HTTP-aanvragen/reacties in cache geplaatst?

HTTP-reacties voor corresponderende HTTP-aanvragen worden in de cache geplaatst wanneer aan alle volgende criteria wordt voldaan:

+ HTTP-aanvraagmethode is `GET` of `HEAD`
   + `HEAD` HTTP-aanvragen slaan alleen de HTTP-antwoordheaders in de cache op. Ze hebben geen reactieorganen.
+ De status van de HTTP-reactie is `200`
+ De reactie van HTTP is NIET voor een binair dossier.
+ Het URL-pad van de HTTP-aanvraag eindigt met een extensie, bijvoorbeeld: `.html` , `.json` , `.css` , `.js` , enzovoort.
+ HTTP-aanvraag bevat geen autorisatie en wordt niet geverifieerd door AEM.
   + Nochtans, kan het caching van voor authentiek verklaarde verzoeken [&#x200B; globaal &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#caching-when-authentication-is-used) of selectief via [&#x200B; toestemming worden toegelaten gevoelig caching &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=nl-NL).
+ HTTP-aanvraag bevat geen queryparameters.
   + Nochtans, het vormen [&#x200B; Genegeerde vraagparameters &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#ignoring-url-parameters) staat HTTP- verzoeken met de genegeerde vraagparameters toe om van het geheime voorgeheugen worden in cache worden opgeslagen/worden gediend.
+ De weg van HTTP- verzoek [&#x200B; past een regel van Dispatcher toe, en past geen ontkent regel &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#specifying-the-documents-to-cache) aan.
+ HTTP-respons heeft geen van de volgende HTTP-responsheaders die door AEM Publish zijn ingesteld:

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### Wat is in cache geplaatst?

AEM Dispatcher plaatst het volgende in cache:

+ HTTP-responsinstantie
+ De de reactiekopballen van HTTP die in de Dispatcher [&#x200B; configuratie van de geheim voorgeheugenkopballen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#caching-http-response-headers) worden gespecificeerd. Zie de standaardconfiguratie die schepen met het [&#x200B; Archetype van het Project van AEM &#x200B;](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113).
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### Levensduur cache

AEM Dispatcher plaatst HTTP-reacties in cache met de volgende methoden:

+ Totdat de validatie wordt geactiveerd door mechanismen zoals het publiceren of ongedaan maken van de publicatie van de inhoud.
+ TTL (tijd-aan-levende) wanneer uitdrukkelijk [&#x200B; in de configuratie van Dispatcher &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#configuring-time-based-cache-invalidation-enablettl) wordt gevormd. Zie de standaardconfiguratie in het [&#x200B; Archetype van het Project van AEM &#x200B;](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) door de `enableTTL` configuratie te herzien.

#### Standaardcache-levensduur

Als een reactie van HTTP voor AEM Dispatcher in het voorgeheugen onderbrengend [&#x200B; per bovengenoemde bepalers &#x200B;](#when-are-http-requestsresponses-cached-1) kwalificeert, zijn het volgende de standaardwaarden tenzij de douaneconfiguratie aanwezig is.

| Inhoudstype | Standaardlevensduur van CDN-cache |
|:------------ |:---------- |
| [&#x200B; HTML/JSON/XML &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#html-text) | Tot ongeldigmaking |
| [&#x200B; Assets (beelden, video&#39;s, documenten, etc.) &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#images) | Nooit |
| [&#x200B; Blijven vragen (JSON) &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=nl-NL&publish-instances) | 1 minuut |
| [&#x200B; de bibliotheken van de Cliënt (JS/CSS) &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#client-side-libraries) | dertig dagen |
| [&#x200B; Andere &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=nl-NL#other-content) | Tot ongeldigmaking |

### Cacheregels aanpassen

Het geheime voorgeheugen van AEM Dispatcher kan via de [&#x200B; configuratie van Dispatcher &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=nl-NL#configuring-the-dispatcher-cache-cache) met inbegrip van worden gevormd:

+ Wat is in cache geplaatst
+ Welke delen van de cache ongeldig worden gemaakt bij publiceren/verwijderen
+ Welke parameters van de HTTP-aanvraagquery worden genegeerd bij de evaluatie van de cache
+ Welke HTTP-antwoordheaders worden in cache geplaatst
+ Het in cache plaatsen van TTL inschakelen of uitschakelen
+ ... en nog veel meer

Als u `mod_headers` gebruikt om cacheheaders in te stellen, heeft de `vhost` -configuratie geen invloed op Dispatcher caching (op basis van TTL), aangezien deze worden toegevoegd aan de HTTP-respons nadat AEM Dispatcher de reactie heeft verwerkt. Om Dispatcher-caching via HTTP-antwoordheaders te beïnvloeden, is aangepaste Java™-code vereist die wordt uitgevoerd in AEM Publish en die de juiste HTTP-antwoordheaders instelt.
