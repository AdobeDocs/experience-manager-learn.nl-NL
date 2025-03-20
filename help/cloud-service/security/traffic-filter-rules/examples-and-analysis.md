---
title: Voorbeelden en resultaatanalyse van verkeersfilterregels inclusief WAF-regels
description: Leer verschillende regels van de Filter van het Verkeer met inbegrip van WAF regelvoorbeelden. Ook, hoe te om de resultaten te analyseren gebruikend de logboeken van CDN van AEM as a Cloud Service (AEMCS).
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 49becbcb-7965-4378-bb8e-b662fda716b7
duration: 532
source-git-commit: 67091c068634e6c309afaf78942849db626128f6
workflow-type: tm+mt
source-wordcount: '1472'
ht-degree: 0%

---

# Voorbeelden en resultaatanalyse van verkeersfilterregels, inclusief WAF-regels

Leer hoe te om diverse types van de regels van de verkeersfilter te verklaren en de resultaten te analyseren gebruikend de logboeken van CDN van Adobe Experience Manager as a Cloud Service (AEMCS) en dashboard tooling.

In deze sectie, zult u praktische voorbeelden van de regels van de verkeersfilter, met inbegrip van de regels van WAF onderzoeken. U zult leren om, verzoeken te registreren toe te staan en te blokkeren die op URI (of weg), IP adres, het aantal verzoeken, en verschillende aanvalstypes worden gebaseerd die het [ Project van de Plaatsen van AEM WKND ](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) gebruiken.

Bovendien zult u ontdekken hoe te om dashboardtooling te gebruiken die de logboeken van AEMCS CDN opneemt om essentiële metriek door Adobe te visualiseren verstrekte steekproefdashboards.

Om aan uw specifieke vereisten te richten, kunt u verbeteren en douanedashboards tot stand brengen, waarbij diepgaande inzichten en optimaliserend de regelconfiguraties voor uw plaatsen van AEM worden verkregen.

>[!VIDEO](https://video.tv.adobe.com/v/3425404?quality=12&learn=on)

## Voorbeelden

Laten we verschillende voorbeelden bekijken van verkeersfilterregels, waaronder WAF-regels. Zorg ervoor u het vereiste opstellingsproces zoals die in vroeger [ wordt beschreven hoe te opstelling ](./how-to-setup.md) hoofdstuk hebt voltooid en dat u het [ Project van de Plaatsen van AEM WKND ](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) hebt gekloond.

### Registratieverzoeken

Begin door **het registreren verzoeken van login WKND en logout wegen** tegen de publicatiedienst van AEM.

- Voeg de volgende regel toe aan het WKND-projectbestand `/config/cdn.yaml` .

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
      - name: publish-auth-requests
        when:
          allOf:
            - reqProperty: tier
              matches: publish
            - reqProperty: path
              in:
                - /system/sling/login/j_security_check
                - /system/sling/logout
        action: log
```

- Leg de wijzigingen vast en duw deze naar de Cloud Manager Git-opslagplaats.

- Stel de veranderingen in het milieu van AEM Dev op gebruikend de Cloud Manager `Dev-Config` configuratiepijplijn [ vroeger gecreeerd ](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![ Cloud Manager Config Pipeline ](./assets/cloud-manager-config-pipeline.png)

- Test de regel door u aan te melden en af te melden bij de WKND-site van uw programma op de service Publiceren (bijvoorbeeld `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html` ). U kunt `asmith/asmith` gebruiken als gebruikersnaam en wachtwoord.

  ![ Login WKND ](./assets/wknd-login.png)

#### Analyseren{#analyzing}

Analyseer de resultaten van de `publish-auth-requests` regel door de logboeken AEMCS CDN van Cloud Manager te downloaden en het [ dashboard tooling ](how-to-setup.md#analyze-results-using-elk-dashboard-tool) te gebruiken, die u opstelling in het vroegere hoofdstuk.

- Van [ Cloud Manager ](https://my.cloudmanager.adobe.com/) **de kaart van Milieu&#39;s**, download de **publiceren** CDN van de dienst van AEMCS logboeken.

  ![ Cloud Manager CDN Logs Downloads ](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    Het kan tot 5 minuten duren voor de nieuwe verzoeken om in de CDN- logboeken te verschijnen.

- Kopieer het gedownloade logbestand (bijvoorbeeld `publish_cdn_2023-10-24.log` in de onderstaande schermafbeelding) naar de map `logs/dev` van het project voor het gereedschap Elastisch dashboard.

  ![ de Omslag van de Logboeken van het Hulpmiddel ELK ](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Vernieuw de gereedschapspagina Elastic dashboard.
   - In de hoogste **Globale filter** sectie, geef de `aem_env_name.keyword` filter uit en selecteer de `dev` milieuwaarde.

     ![ Globale Filter van het Hulpmiddel ELK ](./assets/elk-tool-global-filter.png)

   - Als u het tijdsinterval wilt wijzigen, klikt u op het kalenderpictogram in de rechterbovenhoek en selecteert u het gewenste tijdinterval.

     {het Interval van de Tijd van het Hulpmiddel 0} ELK ](./assets/elk-tool-time-interval.png)![

- Herzie de bijgewerkte versie van het dashboard **geanalyseerde verzoeken**, **Vervroegingen met vlag**, en **Gegrafeerde verzoeken details** panelen. Voor passende CDN logboekingangen, zou het de waarden van de cliëntIP van elke ingang (cli_ip), gastheer, url, actie (waf_action), en regel-naam (waf_match) moeten tonen.

  ![ het Dashboard van het Hulpmiddel van het ELK ](./assets/elk-tool-dashboard.png)


### Blokkeringsaanvragen

In dit voorbeeld, voegen een pagina in een _interne_ omslag bij de weg `/content/wknd/internal` in het opgestelde project WKND toe. Dan verklaar een regel van de verkeersfilter dat **verkeer** aan subpages van overal buiten een gespecificeerd IP adres blokkeert dat uw organisatie (bijvoorbeeld, collectieve VPN) aanpast.

U kunt of uw eigen interne pagina (bijvoorbeeld, `demo-page.html`) tot stand brengen of het [ pakket in bijlage ](./assets/demo-internal-pages-package.zip) gebruiken.

- Voeg de volgende regel toe in het bestand `/config/cdn.yaml` van het WKND-project:

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...

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

- Stel de veranderingen in het milieu van AEM Dev op gebruikend de [ vroeger gecreeerd ](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` configuratiepijplijn in Cloud Manager.

- Test de regel door de interne pagina van de WKND-site te openen, bijvoorbeeld `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` of met de onderstaande CURL-opdracht:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Herhaal de bovenstaande stap van zowel het IP-adres dat u in de regel hebt gebruikt als een ander IP-adres (bijvoorbeeld met uw mobiele telefoon).

#### Analyseren

Om de resultaten van de `block-internal-paths` regel te analyseren, volg de zelfde stappen zoals die in het [ vroegere voorbeeld ](#analyzing) worden beschreven.

Nochtans, zou dit keer u de **Geblokkeerde verzoeken** en overeenkomstige waarden in cliëntIP (cli_ip), gastheer, URL, actie (waf_action), en regel-name (waf_match) kolommen moeten zien.

![ Geblokkeerd Verzoek van het Dashboard van het Hulpmiddel van het ELK ](./assets/elk-tool-dashboard-blocked.png)


### DoS-aanvallen voorkomen

Laat **aanvallen van Dos** verhinderen door verzoeken van een IP adres te blokkeren die 100 verzoeken per seconde maken, die het veroorzaken om voor 5 minuten worden geblokkeerd.

- Voeg de volgende [ regel van de de filterfilter van de tariefgrens ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) in het 2} dossier van het WKND project {toe.`/config/cdn.yaml`

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
```

>[!WARNING]
>
>Voor uw productieomgeving werkt u samen met uw webbeveiligingsteam om de juiste waarden voor `rateLimit` te bepalen.

- Leg, duw, en stel veranderingen zoals vermeld in [ vroegere voorbeelden ](#logging-requests) vast.

- Om de aanval van Dos te simuleren, gebruik het volgende [ bevel van Vegeta ](https://github.com/tsenart/vegeta).

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=60s | vegeta report
  ```

  Dit bevel maakt 120 verzoeken om 5 seconden en output een rapport. Zoals u kunt zien, is het succestarief 32.5%; een 406 de reactiecode van HTTP wordt ontvangen voor de rest, die aantoont dat het verkeer werd geblokkeerd.

  ![ Vegeta Dos aanval ](./assets/vegeta-dos-attack.png)

#### Analyseren

Om de resultaten van de `prevent-dos-attacks` regel te analyseren, volg de zelfde stappen zoals die in het [ vroegere voorbeeld ](#analyzing) worden beschreven.

Deze tijd zou u vele **Geblokkeerde verzoeken** en overeenkomstige waarden in cliëntIP (cli_ip), gastheer, url, actie (waf_action), en regel-name (waf_match) kolommen moeten zien.

![ het Dashboard van het Hulpmiddel van het Hulpmiddel verzoekt ](./assets/elk-tool-dashboard-dos.png)

Ook, tonen de **Hoogste 100 aanvallen door cliëntIP, land, en gebruiker-agent** panelen extra details, die kunnen worden gebruikt om de regelconfiguratie verder te optimaliseren.

![ het Dashboard van het Hulpmiddel van het Hulpmiddel 100 verzoeken ](./assets/elk-tool-dashboard-dos-top-100.png)

Voor meer informatie over hoe te om aanvallen te verhinderen DoS en DDoS, herzie het [ Blokkeren Dos en de aanvallen van DDoS gebruikend de regels van de verkeersfilter ](../blocking-dos-attack-using-traffic-filter-rules.md) leerprogramma.

### WAF-regels

De de regelvoorbeelden van de verkeersfilter tot nu toe kunnen door alle Sites en klanten van Forms worden gevormd.

Daarna, onderzoeken wij de ervaring voor een klant die een vergunning van de Bescherming van de Verbeterde Veiligheid of van WAF-DDoS heeft verworven, die hen geavanceerde regels laat vormen om websites tegen verfijnde aanvallen te beschermen.

Alvorens verder te gaan, laat de bescherming van WAF-DDoS voor uw programma toe, zoals die in de documentatie van de verkeersfilterregels [ opstellingsstappen ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup) wordt beschreven.

#### Zonder WAFFlags

Laten we eerst de ervaring zien, zelfs voordat WAF Rules wordt gedeclareerd. Wanneer WAF-DDoS op uw programma wordt toegelaten, registreert uw CDN door gebrek om het even welke gelijken van kwaadwillig verkeer, zodat hebt u de juiste informatie om omhoog met aangewezen regels te komen.

Laten we eerst de WKND-site aanvallen zonder een WAF-regel toe te voegen (of de eigenschap `wafFlags` te gebruiken) en de resultaten analyseren.

- Om een aanval te simuleren, gebruik het [ Nikto ](https://github.com/sullo/nikto) bevel hieronder, dat het ongeveer 700 kwaadwillige verzoeken in 6 minuten verzendt.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![ Nikto de Simulatie van de Aanval ](./assets/nikto-attack.png)

  Om over aanvalsimulatie te leren, herzie [ Nikto - Scannen ](https://github.com/sullo/nikto/wiki/Scan-Tuning) documentatie, die u vertelt hoe te om het type van testaanvallen te specificeren om te omvatten of uit te sluiten.

##### Analyseren

Om de resultaten van aanvalssimulatie te analyseren, volg de zelfde stappen zoals die in het [ vroegere voorbeeld ](#analyzing) worden beschreven.

Nochtans, zou dit keer u de **Gemarkeerde verzoeken** en overeenkomstige waarden in cliëntIP (cli_ip), gastheer, url, actie (waf_action), en regel-name (waf_match) kolommen moeten zien. Met deze informatie kunt u de resultaten analyseren en de regelconfiguratie optimaliseren.

![ het Dashboard van het Hulpmiddel van het KIEZEN WAF Gemarkeerd Verzoek ](./assets/elk-tool-dashboard-waf-flagged.png)

Merk op hoe de **distributie van de Vlaggen van WAF** en **Hoogste aanvallen** panelen extra details tonen, die kunnen worden gebruikt om de regelconfiguratie verder te optimaliseren.

![ het Dashboard van het Hulpmiddel van het Hulpmiddel WAF het Verzoek van de Aanvallen van de Vlaggen ](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![ het Dashboard van het Hulpmiddel van het Hulpmiddel WAF het Hoogste Aanvraag van Aanvallen ](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Met WAFFlags

Voeg nu een regel van WAF toe die `wafFlags` bezit als deel van het `action` bezit bevat en **blokkeer de gesimuleerde aanvalsverzoeken**.

Vanuit syntaxisoogpunt zijn de WAF-regels vergelijkbaar met die van eerdere versies, maar de eigenschap `action` verwijst naar een of meer `wafFlags` -waarden. Meer over `wafFlags` leren, herzie de [ sectie van de Lijst van de Vlaggen van WAF ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list).

- Voeg de volgende regel toe in het WKND-projectbestand `/config/cdn.yaml` . De regel `block-waf-flags` bevat enkele wafFlags die in de dashboardwerkset waren weergegeven toen deze met gesimuleerd kwaadaardig verkeer werden aangevallen. Het is inderdaad een goede praktijk om logboeken te analyseren om te bepalen welke nieuwe regels moeten worden verklaard, aangezien het bedreigingslandschap evolueert.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
```

- Leg, duw, en stel veranderingen zoals vermeld in [ vroegere voorbeelden ](#logging-requests) vast.

- Om een aanval te simuleren, gebruik het zelfde [ Nikto ](https://github.com/sullo/nikto) bevel zoals voordien.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Analyseren

Herhaal de zelfde stappen zoals die in het [ worden beschreven vroeger voorbeeld ](#analyzing).

Dit keer zou u ingangen onder **Geblokkeerde verzoeken** en de overeenkomstige waarden in cliëntIP (cli_ip), gastheer, url, actie (waf_action), en regel-name (waf_match) kolommen moeten zien.

![ het Dashboard van het Hulpmiddel van het Hulpmiddel WAF Geblokkeerd Verzoek ](./assets/elk-tool-dashboard-waf-blocked.png)

Ook, tonen de **distributie van de Vlaggen van WAF** en **Hoogste aanvallen** panelen extra details.

![ het Dashboard van het Hulpmiddel van het Hulpmiddel WAF het Verzoek van de Aanvallen van de Vlaggen ](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![ het Dashboard van het Hulpmiddel van het Hulpmiddel WAF het Hoogste Aanvraag van Aanvallen ](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Uitgebreide analyse

In de bovengenoemde _analyse_ secties, leerde u hoe te om de resultaten van specifieke regels te analyseren gebruikend het dashboardhulpmiddel. U kunt de resultaten verder analyseren met andere dashboarddeelvensters, zoals:


- Geanalyseerde, gemarkeerde en geblokkeerde aanvragen
- WAF-vlagverdeling in de tijd
- Regels voor gestuurde verkeersfilters in de loop der tijd
- Bovenste aanvallen door WAF Flag ID
- Bovenste getriggerde verkeersfilter
- De hoogste 100 aanvallers door cliëntIP, land, en gebruiker-agent

](./assets/elk-tool-dashboard-comprehensive-analysis-1.png) Uitgebreide Analyse van het Dashboard van het Hulpmiddel 0} ELK {![

](./assets/elk-tool-dashboard-comprehensive-analysis-2.png) Uitgebreide Analyse van het Dashboard van het Hulpmiddel 0} ELK {![


## Volgende stap

Krijg vertrouwd met geadviseerde [ beste praktijken ](./best-practices.md) om het risico van veiligheidsbreuken te verminderen.

## Aanvullende bronnen

[ Syntaxis van de Regels van de Filter van het Verkeer ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[ CDN het Formaat van het Logboek ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)

