---
title: CORS-configuratie voor AEM GraphQL
description: Leer hoe te om het delen van middelen van oorsprong voor gebruik met AEM GraphQL te vormen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: 56831a30a442388e34d8388546787ed22e7577f5
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---


# Delen van bronnen van oorsprong (CORS)

Met CORS (Cross-Origin Resource Sharing) van Adobe Experience Manager as a Cloud Service kunnen niet-AEM wegeigenschappen worden gebruikt om op browser gebaseerde client-side aanroepen te maken naar AEM GraphQL API&#39;s.

>[!TIP]
>
> De volgende configuraties zijn voorbeelden. Zorg ervoor dat u de instellingen aanpast aan de vereisten van uw project.



## CORS-vereiste

CORS is vereist voor op browser-gebaseerde verbindingen met AEM GraphQL APIs, wanneer de cliënt die met AEM verbindt NIET wordt gediend van de zelfde oorsprong (die ook als gastheer of domein wordt bekend) zoals AEM.

| Type client | App van één pagina (SPA) | Webcomponent/JS | Mobiel | Server naar server |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Vereist configuratie CORS | ✔ | ✔ | ✘ | ✘ |

## OSGi-configuratie

De AEM OSGi-configuratiefabriek van CORS definieert de toegestane criteria voor het accepteren van HTTP-aanvragen van CORS.

| Client maakt verbinding met | AEM-auteur | AEM-publicatie | Voorvertoning AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Vereist de configuratie CORS OSGi | ✔ | ✔ | ✔ |


In het onderstaande voorbeeld wordt een OSGi-configuratie voor AEM-publicatie gedefinieerd (`../config.publish/..`), maar kan worden toegevoegd aan [elke ondersteunde runmode-map](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#runmode-resolution).

De belangrijkste configuratieeigenschappen zijn:

+ `alloworigin` en/of `alloworiginregexp` geeft de oorsprong(en) aan van de client die verbinding maakt met AEM webversie.
+ `allowedpaths` Hiermee geeft u het URL-padpatroon (de URL&#39;s) op die zijn toegestaan vanuit de opgegeven oorsprong. Om AEM GraphQL voortgeduurde vragen te steunen, gebruik het volgende patroon: `"/graphql/execute.json.*"`
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


## Dispatcher-configuratie

De Dispatcher van de AEM-service Publiceren (en Voorvertoning) moet zijn geconfigureerd om CORS te ondersteunen.

| Client maakt verbinding met | AEM-auteur | AEM-publicatie | Voorvertoning AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Vereist de configuratie van Dispatcher CORS | ✘ | ✔ | ✔ |

### CORS-koppen toestaan bij HTTP-aanvragen

Werk de `clientheaders.any` bestand om HTTP-aanvraagheaders toe te staan `Origin`,  `Access-Control-Request-Method` en `Access-Control-Request-Headers` door te geven aan AEM, zodat de HTTP-aanvraag kan worden verwerkt door de [AEM CORS-configuratie](#osgi-configuration).

`dispatcher/src/conf.dispatcher.d/clientheaders/clientheaders.any`

```
# Allowing CORS headers to be passed through to the publish tier to support headless and SPA Editor use cases.
# https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

"Origin"
"Access-Control-Request-Method"
"Access-Control-Request-Headers"

$include "./default_clientheaders.any"
```

### CoRS HTTP-responsheaders leveren

Verzendingsbedrijf configureren om in cache te plaatsen **CORS HTTP-responsheaders** om ervoor te zorgen zij inbegrepen zijn wanneer AEM GraphQL voortgezette vragen van het geheime voorgeheugen van de Verzender door toe te voegen worden gediend `Access-Control-...` kopteksten aan de lijst van geheim voorgeheugenkopballen.

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
