---
title: CORS-configuratie voor AEM GraphQL
description: Leer hoe te om het delen van middelen van oorsprong voor gebruik met AEM GraphQL te vormen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---


# Delen van bronnen van oorsprong (CORS)

Met CORS (Cross-Origin Resource Sharing) van Adobe Experience Manager as a Cloud Service kunnen niet-AEM wegeigenschappen worden gebruikt om op browser gebaseerde client-side aanroepen te maken naar AEM GraphQL API&#39;s.

>[!TIP]
>
> De volgende configuraties zijn voorbeelden. Zorg ervoor dat u de instellingen aanpast aan de vereisten van uw project.

## CORS-vereiste

CORS is vereist voor op browser-gebaseerde verbindingen met AEM GraphQL APIs, wanneer de cliënt die met AEM verbindt NIET wordt gediend van de zelfde oorsprong (die ook als gastheer of domein wordt bekend) zoals AEM.

| Type client | [App van één pagina (SPA)](../spa.md) | [Webcomponent/JS](../web-component.md) | [Mobiel](../mobile.md) | [Server-naar-server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Vereist configuratie CORS | ✔ | ✔ | ✘ | ✘ |

## OSGi-configuratie

De AEM OSGi-configuratiefabriek van CORS definieert de toegestane criteria voor het accepteren van HTTP-aanvragen van CORS.

| Client maakt verbinding met | AEM-auteur | AEM-publicatie | Voorvertoning AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Vereist de configuratie CORS OSGi | ✔ | ✔ | ✔ |


In het onderstaande voorbeeld wordt een OSGi-configuratie voor AEM-publicatie gedefinieerd (`../config.publish/..`), maar kan worden toegevoegd aan [elke ondersteunde runmodusmap](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

De belangrijkste configuratieeigenschappen zijn:

+ `alloworigin` en/of `alloworiginregexp` geeft de oorsprong aan van de client die verbinding maakt met AEM webuitvoering.
+ `allowedpaths` geeft de URL-padpatronen aan die zijn toegestaan vanuit de opgegeven oorsprong. Om AEM GraphQL voortgeduurde vragen te steunen, gebruik het volgende patroon: `"/graphql/execute.json.*"`
+ `supportedmethods` geeft de toegestane HTTP-methoden voor de CORS-aanvragen aan. Toevoegen `GET`, om AEM GraphQL voortgeduurde vragen te steunen.

[Leer meer over de configuratie CORS OSGi.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)


Deze voorbeeldconfiguratie steunt gebruik van AEM GraphQL voortgeduurde vragen. Om cliënt-bepaalde vragen te gebruiken GraphQL, voeg een eindpuntURL GraphQL in `allowedpaths` en `POST` tot `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
    "http://localhost:.*"
  ],
  "allowedpaths": [
    "/graphql/execute.json.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers"
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": false,
  "exposedheaders":[ "" ]
}
```

### Geautoriseerde AEM GraphQL API-aanvragen

Wanneer het toegang tot AEM GraphQL APIs die vergunning (typisch auteur AEM of beschermde inhoud op publiceren AEM) vereisen zorg ervoor de configuratie CORS OSGi de extra waarden heeft:

+ `supportedheaders` ook lijsten `"Authorization"`
+ `supportscredentials` is ingesteld op `true`

De geautoriseerde verzoeken om GraphQL APIs te AEM die configuratie CORS vereisen zijn ongebruikelijk, aangezien zij typisch in de context van voorkomen [server-naar-server apps](../server-to-server.md) en dus, vereist geen configuratie CORS. Op browsers gebaseerde toepassingen waarvoor CORS-configuraties vereist zijn, zoals [apps van één pagina](../spa.md) of [Webcomponenten](../web-component.md), gebruikt doorgaans geen autorisatie omdat het moeilijk is om de gegevens te beveiligen.

Deze twee instellingen worden bijvoorbeeld als volgt ingesteld in een `CORSPolicyImpl` OSGi-fabrieksconfiguratie:

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{ 
  ...
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  ...
  "supportscredentials": true,
  ...
}
```

#### Voorbeeld OSGi-configuratie

+ [Een voorbeeld van de configuratie OSGi kan in het project worden gevonden WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Dispatcher-configuratie

De Dispatcher van de AEM-service Publiceren (en Voorvertoning) moet zijn geconfigureerd om CORS te ondersteunen.

| Client maakt verbinding met | AEM-auteur | AEM-publicatie | Voorvertoning AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Vereist de configuratie van Dispatcher CORS | ✘ | ✔ | ✔ |

### CORS-koppen toestaan bij HTTP-aanvragen

Werk de `clientheaders.any` bestand om HTTP-aanvraagheaders toe te staan `Origin`,  `Access-Control-Request-Method`, en `Access-Control-Request-Headers` door te geven aan AEM, zodat de HTTP-aanvraag kan worden verwerkt door de [AEM CORS-configuratie](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

#### Configuratie van voorbeeldverzending

+ [Een voorbeeld van de Dispatcher _clientkoppen_ De configuratie kan in het WKND- project worden gevonden.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any#L10-L12)


### CoRS HTTP-responsheaders leveren

Distributiefarm configureren om in cache te plaatsen **CORS HTTP-responsheaders** om ervoor te zorgen zij inbegrepen zijn wanneer AEM GraphQL voortgezette vragen van het geheime voorgeheugen van de Verzender door toe te voegen worden gediend `Access-Control-...` kopteksten aan de lijst van geheim voorgeheugenkopballen.

+ `dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm`

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

#### Configuratie van voorbeeldverzending

+ [Een voorbeeld van de Dispatcher _CORS HTTP-responsheaders_ De configuratie kan in het WKND- project worden gevonden.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/available_farms/wknd.farm#L109-L114)
