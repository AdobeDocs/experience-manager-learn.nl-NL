---
title: CORS-configuratie voor AEM GraphQL
description: Leer hoe u het delen van bronnen van oorsprong voor gebruik met AEM GraphQL configureert.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2024-03-22T00:00:00Z
duration: 184
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# Delen van bronnen van oorsprong (CORS)

Met CORS (Cross-Origin Resource Sharing) van Adobe Experience Manager as a Cloud Service kunnen niet-AEM wegeigenschappen worden gebruikt om op browser gebaseerde client-side aanroepen te maken naar AEM GraphQL API&#39;s en andere AEM Headless-bronnen.

>[!TIP]
>
> De volgende configuraties zijn voorbeelden. Zorg ervoor dat u de instellingen aanpast en aanpast aan de vereisten van uw project.

## CORS-vereiste

CORS is vereist voor browsergebaseerde verbindingen met AEM GraphQL API&#39;s, wanneer de client die verbinding maakt met AEM NIET vanuit dezelfde oorsprong (ook wel host of domein genoemd) als AEM wordt aangeboden.

| Type client | [App van één pagina (SPA)](../spa.md) | [Webcomponent/JS](../web-component.md) | [Mobiel](../mobile.md) | [Server-naar-server](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Vereist configuratie CORS | ✔ | ✔ | ✘ | ✘ |

## AEM auteur

Het inschakelen van CORS op AEM Auteur-service verschilt van AEM Services voor publiceren en AEM. AEM de dienst van de Auteur vereist een configuratie OSGi die aan de de wijze omslag van de wijze van de AEM dienst van de Auteur moet worden toegevoegd, en gebruikt geen configuratie van de Ontvanger.

### OSGi-configuratie

De AEM OSGi-configuratiefabriek van CORS definieert de toegestane criteria voor het accepteren van HTTP-aanvragen van CORS.

| Client maakt verbinding met | AEM auteur | AEM publiceren | Voorvertoning AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Vereist de configuratie CORS OSGi | ✔ | ✘ | ✘ |


In het onderstaande voorbeeld wordt een OSGi-configuratie voor AEM auteur gedefinieerd (`../config.author/..`) is dus alleen actief op AEM Auteur-service.

De belangrijkste configuratieeigenschappen zijn:

+ `alloworigin` en/of `alloworiginregexp` geeft de oorsprong aan van de client die verbinding maakt met AEM webuitvoering.
+ `allowedpaths` geeft de URL-padpatronen aan die zijn toegestaan vanuit de opgegeven oorsprong.
   + Voeg het volgende patroon toe ter ondersteuning van AEM door GraphQL voortgezette query&#39;s: `/graphql/execute.json.*`
   + Voeg het volgende patroon toe ter ondersteuning van Experience Fragments: `/content/experience-fragments/.*`
+ `supportedmethods` geeft de toegestane HTTP-methoden voor de CORS-aanvragen aan. Om AEM GraphQL persisted query&#39;s (en Experience Fragments) te ondersteunen, voegt u `GET` .
+ `supportedheaders` include `"Authorization"` als verzoeken aan AEM auteur moeten worden toegestaan.
+ `supportscredentials` is ingesteld op `true` als verzoek aan AEM auteur moet worden toegestaan.

[Leer meer over de configuratie CORS OSGi.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html)

In het volgende voorbeeld wordt het gebruik van AEM GraphQL-query&#39;s op AEM auteur ondersteund. Als u door de client gedefinieerde GraphQL-query&#39;s wilt gebruiken, voegt u een GraphQL-eindpunt-URL toe in `allowedpaths` en `POST` tot `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
    "/content/experience-fragments/.*"
  ],
  "supportedheaders": [
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization"
  ],
  "supportedmethods":[
    "GET",
    "HEAD",
    "POST"
  ],
  "maxage:Integer": 1800,
  "supportscredentials": true,
  "exposedheaders":[ "" ]
}
```

#### Voorbeeld OSGi-configuratie

+ [Een voorbeeld van de configuratie OSGi kan in het project worden gevonden WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## AEM publiceren

Het inschakelen van CORS voor AEM publicatieservices (en voorvertoningen) verschilt van de AEM Auteur-service. AEM publicatieservice vereist een AEM Dispatcher-configuratie die moet worden toegevoegd aan de configuratie Dispatcher van AEM publiceren. AEM Publiceren gebruikt geen [OSGi-configuratie](#osgi-configuration).

Zorg bij het configureren van CORS bij AEM publiceren voor:

+ De `Origin` De HTTP- verzoekkopbal kan niet naar AEM de Publish dienst worden verzonden, door te verwijderen `Origin` header (indien eerder toegevoegd) van het project van de AEM Dispatcher `clientheaders.any` bestand. Alle `Access-Control-` Koppen moeten worden verwijderd uit de `clientheaders.any` en Dispatcher beheren deze, niet AEM de publicatieservice.
+ Als u [CORS OSGi-configuraties](#osgi-configuration) ingeschakeld op uw AEM publicatieservice, moet u deze verwijderen en hun configuraties migreren naar de [Dispatcher-hostconfiguratie](#set-cors-headers-in-vhost) hieronder beschreven.

### Dispatcher-configuratie

AEM de Dispatcher van de dienst van de Publicatie (en van de Voorproef) moet worden gevormd om CORS te steunen.

| Client maakt verbinding met | AEM auteur | AEM publiceren | Voorvertoning AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Vereist de configuratie van Dispatcher CORS | ✘ | ✔ | ✔ |

#### CORS-koppen instellen in host

1. Open het dossier van de gastheerconfiguratie voor de AEM publiceer dienst, in uw de configuratieproject van de Ontvanger, typisch bij `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Kopieer de inhoud van het dialoogvenster `<IfDefine ENABLE_CORS>...</IfDefine>` blok hieronder, in uw toegelaten dossier van de gastheerconfiguratie.

   ```{ highlight="17"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'http://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch 'https://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish if this configuration handles the CORS
         RequestHeader unset Origin "expr=reqenv('CORSTrusted') == 'true'"
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. Pas de gewenste Origins aan die tot uw AEM toegang hebben publiceer dienst door de regelmatige uitdrukking in de hieronder lijn bij te werken. Als meerdere oorsprong vereist zijn, dupliceert u deze regel en werkt u deze bij voor elk oorsprong-/oorsprongspatroon.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + Bijvoorbeeld om toegang CORS van oorsprong toe te laten:

      + Willekeurig subdomein op `https://example.com`
      + Elke poort op `http://localhost`

     Vervang de regel door de volgende twee regels:

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Configuratie van voorbeeldverzending

+ [Een voorbeeld van de configuratie van de Verzender kan in het WKND project worden gevonden.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
