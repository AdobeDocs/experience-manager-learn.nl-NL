---
title: Adobe CDN - Geavanceerde functies naast caching
description: Leer over geavanceerde eigenschappen van Adobe CDN voorbij caching, zoals het vormen van verkeer bij CDN, CDN foutenpagina's en meer.
version: Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, Architect, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
source-git-commit: cc7fa9bbaa775b0216c839be079504c2b5467878
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---


# Adobe CDN - Geavanceerde functies naast caching

Leer over geavanceerde eigenschappen van Adobe CDN voorbij caching, zoals het vormen van verkeer bij CDN, CDN foutenpagina&#39;s en meer.

Naast het in cache plaatsen van inhoud, biedt Adobe CDN verschillende geavanceerde functies die u kunnen helpen uw websiteprestaties te optimaliseren. Deze functies zijn onder andere:

- Het vormen verkeer bij CDN
- CDN-referenties en verificatie configureren
- CDN-foutpagina&#39;s

Deze eigenschappen zijn **zelfbediening** eigenschappen. Gevormd in het `cdn.yaml` dossier van uw AEM project en opgesteld gebruikend de configuratiepijpleiding van Cloud Manager.

## Het vormen verkeer bij CDN

Begrijp de belangrijkste mogelijkheden met betrekking tot _het Vormen verkeer bij CDN_:

- **de aanvalspreventie van Dos:** Adobe CDN absorbeert de aanvallen van Dos bij het netwerk
laag, zodat deze niet op de oorspronkelijke server terechtkomen.
- **het Beperken van het Tarief:** om uw oorsprongsserver tegen wordt overweldigd met teveel verzoeken te beschermen, kunt u tarief vormen die op CDN beperken.
- **Firewall van de Toepassing van het Web (WAF):** WAF beschermt uw website tegen gemeenschappelijke kwetsbaarheid van de Webtoepassing, zoals SQL injectie, dwars-plaats scripting, en meer. Voor het gebruik van deze functie is de licentie voor uitgebreide beveiliging of WAF-DDoS-beveiliging vereist.
- **transformatie van het Verzoek:** wijzig inkomende verzoeken zoals het plaatsen of het unsetting kopballen, het wijzigen van vraagparameters, koekjes en meer.
- **transformatie van de Reactie:** wijzig uitgaande reacties zoals het plaatsen of het ongedaan maken van kopballen.
- **selectie van de Oorsprong:** verkeer van de Route aan verschillende oorsprongservers (Adobe en niet-Adobe) die op verzoek URL wordt gebaseerd.
- **URL richt opnieuw:** richt verzoeken (HTTP 301/302) aan een verschillende absolute of relatieve URL om.

## CDN-referenties en verificatie configureren

Begrijp de belangrijkste mogelijkheden met betrekking tot _het Vormen CDN geloofsbrieven en authentificatie_:

- **zuiveren API Token**: Laat u toe om uw eigen zuiveringssleutel tot stand te brengen voor het zuiveren van enige of groep of alle middelen van het geheime voorgeheugen.
- **Basisauthentificatie**: Een lichtgewichtauthentificatiemechanisme wanneer u toegang tot uw website of een deel van het wilt beperken. Meestal vereist als onderdeel van verschillende evaluatieprocessen voordat u live gaat.
- **bevestiging van de Kopbal van HTTP**: Gebruikt wanneer een klant beheerde CDN verkeer aan Adobe CDN verplettert. De Adobe-CDN valideert de binnenkomende aanvraag op basis van de headerwaarde `X-AEM-Edge-Key` .
Hiermee kunt u uw eigen waarde voor de koptekst van `X-AEM-Edge-Key` maken.

## CDN-foutpagina&#39;s

Laten wij de belangrijkste mogelijkheden met betrekking tot _CDN foutenpagina&#39;s_ begrijpen:

- **Gemarkeerde foutenpagina&#39;s**: Vertoning een brandde foutenpagina aan uw gebruikers in het _onwaarschijnlijke scenario_ wanneer Adobe CDN uw oorsprongsserver niet kan bereiken.

## Uitvoeren

De implementatie van deze geavanceerde functies omvat twee stappen:

1. **CDN config- dossier van de Update**: Werk het `cdn.yaml` dossier in uw AEM project met de vereiste configuraties bij. De configuraties worden toegevoegd als regels en zij volgen een regelsyntaxis. De regel drie hoofdcomponenten: `name`, `when` en `action` .

2. **stel CDN config- dossier** op: stel het bijgewerkte `cdn.yaml` dossier op gebruikend de Cloud Manager config pijpleiding. Voor meer informatie, zie [ regels opstellen door Cloud Manager ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

### Voorbeeld

In het onderstaande voorbeeld is de WKND-voorbeeldsite geconfigureerd om de `/top3` URL om te leiden naar `/us/en/top3.html` .

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  experimental_redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## Aanvullende bronnen

[ Beschermend websites met de regels van de verkeersfilter ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[ vorm en stel de regel van CDN van de bevestiging van de Kopbal van HTTP op ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[ hoe te om het CDN geheime voorgeheugen ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache) te zuiveren

[ het Vormen Verkeer bij CDN ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[ het Vormen CDN geloofsbrieven en Authentificatie ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

[ Vormend CDN de Pagina&#39;s van de Fout ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-error-pages)




