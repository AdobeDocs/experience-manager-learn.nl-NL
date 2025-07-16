---
title: Verzoeken normaliseren
description: Leer hoe te om verzoeken te normaliseren door hen te transformeren gebruikend de regels van de verkeersfilter in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 0%

---

# Verzoeken normaliseren

Leer hoe te om verzoeken te normaliseren door hen te transformeren gebruikend de regels van de verkeersfilter in AEM as a Cloud Service.

## Waarom en wanneer om verzoeken om te zetten

De transformaties van het verzoek zijn nuttig wanneer u inkomend verkeer wilt normaliseren en onnodige variatie verminderen die door onnodige vraagparameters of kopballen wordt veroorzaakt. Deze techniek wordt doorgaans gebruikt om:

- Verbeter caching efficiency door geheim voorgeheugensteunende parameters te schrappen die niet relevant voor de toepassing van AEM zijn.
- Bescherm de oorsprong tegen misbruik door verzoekpermutaties tot een minimum te beperken en onnodige verwerking te beperken.
- Aanvragen ontsmetten of vereenvoudigen voordat ze naar AEM worden doorgestuurd.

Deze transformaties worden typisch toegepast bij de laag CDN, vooral voor AEM publiceer rijen die openbaar verkeer dienen.

## Vereisten

Alvorens te werk te gaan, zorg ervoor u de vereiste opstelling zoals die in [ wordt beschreven hoe te opstellings de filter van het verkeer en de regels van WAF ](../setup.md) leerprogramma hebt voltooid. Ook, hebt u gekloond en opgesteld het [ Project van de Plaatsen van AEM WKND ](https://github.com/adobe/aem-guides-wknd) aan uw milieu van AEM.

## Voorbeeld: de verwijderde queryparameters zijn niet nodig voor de toepassing

In dit voorbeeld, vormt u een regel die **alle vraagparameters behalve** verwijdert `search` en `campaignId` om geheim voorgeheugenfragmentatie te verminderen.

- Voeg de volgende regel toe aan het WKND-projectbestand `/config/cdn.yaml` .

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- Leg de wijzigingen vast en duw deze naar de Cloud Manager Git-opslagplaats.

- Stel de veranderingen in het milieu van AEM op gebruikend de Cloud Manager config pijpleiding [ vroeger gecreeerd ](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Test de regel door de pagina van de WKND-site te openen, bijvoorbeeld `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz` .

- In AEM-logboeken (`aemrequest.log`) moet u controleren of de aanvraag is omgezet in `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar` en wordt de aanvraag `otherParam` verwijderd.

  ![ WKND verzoektransformatie ](../assets/how-to/aemrequest-log-transformation.png)

