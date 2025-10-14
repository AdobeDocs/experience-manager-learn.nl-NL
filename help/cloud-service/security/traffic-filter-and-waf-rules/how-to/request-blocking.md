---
title: Toegang beperken
description: Leer hoe te om toegang te beperken door specifieke verzoeken te blokkeren gebruikend de regels van de verkeersfilter in AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---

# Toegang beperken

Leer hoe te om toegang te beperken door specifieke verzoeken te blokkeren gebruikend de regels van de verkeersfilter in AEM as a Cloud Service.

Dit leerprogramma toont aan hoe te **blokverzoeken aan interne wegen van openbare IPs** in AEM de publicatiedienst publiceren.

## Waarom en wanneer verzoeken te blokkeren

Het blokkeren van verkeershulp dwingt organisatorisch veiligheidsbeleid af door toegang tot gevoelige middelen of URLs onder bepaalde voorwaarden te verhinderen. Vergeleken met het registreren, is het blokkeren een striktere actie en zou moeten worden gebruikt wanneer u zeker bent dat het verkeer uit specifieke bronnen onbevoegd of ongewenst is.

Veelvoorkomende scenario&#39;s waarbij blokkering passend is, zijn:

- Toegang tot `internal` - of `confidential` -pagina&#39;s beperken tot alleen interne IP-bereiken (bijvoorbeeld achter een bedrijfsVPN).
- Het blokkeren van zowel verkeer, geautomatiseerde scanners, als bedreigende actoren die door IP of geolocatie worden geïdentificeerd.
- Toegang tot afgekeurde of onbeveiligde eindpunten voorkomen tijdens gefaseerde migratie.
- Het beperken van toegang tot auteurshulpmiddelen of admin routes in Publish lagen.

## Vereisten

Alvorens te werk te gaan, zorg ervoor u de vereiste opstelling zoals die in [&#x200B; wordt beschreven hoe te opstellings de filter van het verkeer en de regels van WAF &#x200B;](../setup.md) leerprogramma hebt voltooid. Ook, hebt u gekloond en opgesteld het [&#x200B; Project van de Plaatsen van AEM WKND &#x200B;](https://github.com/adobe/aem-guides-wknd) aan uw milieu van AEM.

## Voorbeeld: interne paden van openbare IP&#39;s blokkeren

In dit voorbeeld, vormt u een regel om externe toegang tot een interne pagina WKND, zoals `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, van openbare IP adressen te blokkeren. Alleen gebruikers binnen een vertrouwd IP-bereik (zoals een collectief VPN) hebben toegang tot deze pagina.

U kunt of uw eigen interne pagina (bijvoorbeeld, `demo-page.html`) tot stand brengen of het [&#x200B; pakket in bijlage &#x200B;](../assets/how-to/demo-internal-pages-package.zip) gebruiken.

- Voeg de volgende regel toe aan het WKND-projectbestand `/config/cdn.yaml` .

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
    - name: block-internal-paths
      when:
        allOf:
          - reqProperty: path
            matches: /content/wknd/internal
          - reqProperty: clientIp
            notIn: [192.150.10.0/24]
      action: block    
```

- Leg de wijzigingen vast en duw deze naar de Cloud Manager Git-opslagplaats.

- Stel de veranderingen in het milieu van AEM op gebruikend de Cloud Manager config pijpleiding [&#x200B; vroeger gecreeerd &#x200B;](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Test de regel door de interne pagina van de WKND-site te openen, bijvoorbeeld `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` of met de onderstaande CURL-opdracht:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Herhaal de bovenstaande stap van zowel het IP-adres dat u in de regel hebt gebruikt als een ander IP-adres (bijvoorbeeld met uw mobiele telefoon).

## Analyseren

Om de resultaten van de `block-internal-paths` regel te analyseren, volg de zelfde stappen zoals die in [&#x200B; worden beschreven opstellingsleerprogramma &#x200B;](../setup.md#cdn-logs-ingestion)

U zou de **Geblokkeerde verzoeken** en overeenkomstige waarden in cliëntIP (cli_ip), gastheer, URL, actie (waf_action), en regel-naam (waf_match) kolommen moeten zien.

![&#x200B; Geblokkeerd Verzoek van het Dashboard van het Hulpmiddel van het ELK &#x200B;](../assets/how-to/elk-tool-dashboard-blocked.png)
