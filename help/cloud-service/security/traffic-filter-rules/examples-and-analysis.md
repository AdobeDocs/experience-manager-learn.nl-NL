---
title: Voorbeelden en resultaatanalyse van de regels van de Filter van het Verkeer met inbegrip van de regels van WAF
description: Leer diverse regels van de Filter van het Verkeer met inbegrip van de regelvoorbeelden van WAF. Ook, hoe te om de resultaten te analyseren gebruikend AEM as a Cloud Service (AEMCS) CDN- logboeken.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: 3752e22455020b58d23524f7e6a99414e773422d
workflow-type: tm+mt
source-wordcount: '1510'
ht-degree: 0%

---


# Voorbeelden en resultaatanalyse van verkeersfilterregels inclusief WAF-regels

Leer hoe te om diverse types van de regels van de verkeersfilter te verklaren en de resultaten te analyseren gebruikend de logboeken van CDN van Adobe Experience Manager as a Cloud Service (AEMCS) en dashboard tooling.

In deze sectie, verkent u praktische voorbeelden van de regels van de verkeersfilter, met inbegrip van de regels van WAF. U leert hoe te om, verzoeken te registreren toe te staan en te blokkeren die op URI (of weg), IP adres, het aantal verzoeken, en verschillende aanvalstypes worden gebaseerd gebruikend [AEM WKND-siteproject](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

Bovendien ontdekt u hoe te om dashboardtooling te gebruiken die logboeken opneemt AEMCS CDN om essentiële metriek door Adobe te visualiseren verstrekte steekproefdashboards.

Om zich aan uw specifieke vereisten te richten, kunt u verbeteren en douanedashboards tot stand brengen waarbij diepgaande inzichten en het optimaliseren van de regelconfiguraties voor uw AEM plaatsen worden verkregen.

## Voorbeelden

Laten we verschillende voorbeelden van verkeersfilterregels onderzoeken, waaronder WAF-regels. Zorg ervoor dat u het vereiste installatieproces hebt voltooid zoals in het vorige [instellen](./how-to-setup.md) en u hebt het [AEM WKND-siteproject](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### Registratieverzoeken

Beginnen met **het registreren verzoeken van login WKND en logout wegen** op de service AEM publiceren.

- Voeg de volgende regel aan WKND project toe `/config/cdn.yaml` bestand.

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

- Leg de wijzigingen vast en duw deze naar de gegevensopslagruimte van Cloud Manager.

- De wijzigingen in AEM ontwikkelomgeving implementeren met Cloud Manager `Dev-Config` configuratiepijplijn [eerder gemaakt](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Cloud Manager Config Pipeline](./assets/cloud-manager-config-pipeline.png)

- Test de regel door u aan te melden en af te melden bij de WKND-site van uw programma op de service Publiceren (bijvoorbeeld `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). U kunt `asmith/asmith` als de gebruikersnaam en het wachtwoord.

  ![WKND-aanmelding](./assets/wknd-login.png)

#### Analyseren{#analyzing}

Analyseer de resultaten van de `publish-auth-requests` regel door de AEMCS CDN-logbestanden te downloaden van Cloud Manager en de [dashboardgereedschap](how-to-setup.md#analyze-results-using-elk-dashboard-tool), die u hebt ingesteld in het vorige hoofdstuk.

- Van [Cloud Manager](https://my.cloudmanager.adobe.com/)s **Omgevingen** kaart, download de AEMCS **Publiceren** CDN-logbestanden van de service.

  ![CDN-logbestanden voor cloudbeheer](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    Het kan tot 5 minuten duren voor de nieuwe verzoeken om in de CDN- logboeken te verschijnen.

- Kopieer het gedownloade logbestand (bijvoorbeeld `publish_cdn_2023-10-24.log` in de onderstaande schermafbeelding) in de `logs/dev` map van het project Elastic dashboard.

  ![Logboekmap ELK-gereedschap](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Vernieuw de gereedschapspagina Elastic dashboard.
   - Bovenaan **Globaal, filter** sectie, bewerken `aem_env_name.keyword` filter en selecteer de `dev` omgevingswaarde.

     ![Globaal filter ELK](./assets/elk-tool-global-filter.png)

   - Als u het tijdsinterval wilt wijzigen, klikt u op het kalenderpictogram in de rechterbovenhoek en selecteert u het gewenste tijdinterval.

     ![Tijdinterval ELK](./assets/elk-tool-time-interval.png)

- Controleer de bijgewerkte dashboardbestanden  **Geanalyseerde verzoeken**, **Gemarkeerde aanvragen**, en **Gegevens gemarkeerde verzoeken** deelvensters. Voor passende CDN logboekingangen, zou het de waarden van de cliëntIP van elke ingang (cli_ip), gastheer, url, actie (waf_action), en regel-naam (waf_match) moeten tonen.

  ![ELK-gereedschapdashboard](./assets/elk-tool-dashboard.png)


### Blokkeringsaanvragen

In dit voorbeeld, laten wij een pagina in toevoegen _internal_ map op `/content/wknd/internal` weg in het opgestelde WKND project. Dan verklaar een regel van de verkeersfilter die **blokkeert verkeer** aan subpagina&#39;s van overal buiten een gespecificeerd IP adres dat uw organisatie (bijvoorbeeld, een collectief VPN) aanpast.

U kunt uw eigen interne pagina maken (bijvoorbeeld `demo-page.html`) of de [bijgevoegd pakket](./assets/demo-internal-pages-package.zip)

- Voeg de volgende regel in het WKND-project toe `/config/cdn.yaml` bestand.

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

- Leg de wijzigingen vast en duw deze naar de gegevensopslagruimte van Cloud Manager.

- Implementeer de wijzigingen in de AEM Dev-omgeving met de [eerder gemaakt](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` configuratiepijplijn in Cloud Manager.

- Test de regel door de interne pagina van de WKND-site te openen, bijvoorbeeld `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` of met de onderstaande CURL-opdracht:

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Herhaal de bovenstaande stap van zowel het IP-adres dat u in de regel hebt gebruikt als een ander IP-adres (bijvoorbeeld met uw mobiele telefoon).

#### Analyseren

De resultaten van het `block-internal-paths` regel, dezelfde stappen volgen als in het dialoogvenster [vroeger voorbeeld](#analyzing).

Deze keer moet u echter de **Geblokkeerde aanvragen** en corresponderende waarden in client-IP (cli_ip), host, URL, action (waf_action) en regel-name (waf_match) kolommen.

![Verzoek om geblokkeerd ELK-dashboard](./assets/elk-tool-dashboard-blocked.png)


### DoS-aanvallen voorkomen

Laten we **DoS-aanvallen voorkomen** door verzoeken van een IP adres te blokkeren die 100 verzoeken per seconde maken, die het veroorzaken om 5 minuten worden geblokkeerd.

- Voeg het volgende toe [regel van de snelheidsbeperking van het verkeersfilter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) in het WKND-project `/config/cdn.yaml` bestand.

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
>Voor uw productiemilieu, werk met uw team van de Veiligheid van het Web samen om de aangewezen waarden voor te bepalen `rateLimit`,

- Breng, duw, en stel veranderingen zoals vermeld in [eerdere voorbeelden](#logging-requests).

- Om de aanval van Dos te simuleren, gebruik het volgende [Vegeta](https://github.com/tsenart/vegeta) gebruiken.

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  Dit bevel maakt 120 verzoeken om 5 seconden en output een rapport. Aangezien u kunt zien dat het succestarief 32.5% is en voor de rest een 406 de reactiecode van HTTP wordt ontvangen, die aantoont dat het verkeer werd geblokkeerd.

  ![Vegeta DoS-aanval](./assets/vegeta-dos-attack.png)

#### Analyseren

De resultaten van het `prevent-dos-attacks` regel, dezelfde stappen volgen als in het dialoogvenster [vroeger voorbeeld](#analyzing).

Deze keer ziet u er veel **Geblokkeerde aanvragen** en corresponderende waarden in client-IP (cli_ip), host, url, action (waf_action) en regel-name (waf_match) kolommen.

![Aanvraag voor Dashboard DoS van ELK-hulpprogramma](./assets/elk-tool-dashboard-dos.png)

Ook de **De hoogste 100 aanvallen door cliëntIP, land, en gebruiker-agent** bevat aanvullende details die kunnen worden gebruikt om de configuratie van de regels verder te optimaliseren.

![Dashboard DoS Top 100 van ELK-hulpprogramma](./assets/elk-tool-dashboard-dos-top-100.png)

### WAF-regels

De de regelvoorbeelden van de verkeersfilter tot nu toe kunnen door alle Sites en klanten van Forms worden gevormd.

Daarna, onderzoeken wij de ervaring voor een klant die een verbeterde vergunning van de Bescherming van de Veiligheid of WAF-DDoS heeft verworven. Dit machtigt u om geavanceerde regels te vormen om uw AEM tegen verfijndere aanvallen te beschermen.

Alvorens verder te gaan, laat de bescherming WAF-DDoS voor uw programma, zoals die in de documentatie van de regels van de verkeersfilter wordt beschreven toe [installatiestappen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup).

#### Zonder WAFFlags

Laten we eerst de ervaring zien, zelfs voordat WAF Rules wordt gedeclareerd. Wanneer WAF-DDoS op uw programma wordt toegelaten, registreert uw CDN door standaardlogboek om het even welke gelijken van kwaadwillig verkeer, zodat hebt u de juiste informatie om omhoog met aangewezen regels te komen.

Laten we beginnen door de WKND-site aan te vallen zonder de WAF-regel toe te voegen (of door de `wafFlags` eigenschap) en de resultaten analyseren.

- Om een aanval te simuleren, gebruik [Nikto](https://github.com/sullo/nikto) hieronder, dat het ongeveer 700 kwaadwillige verzoeken in 6 minuten verzendt.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Nikto Attack Simulation](./assets/nikto-attack.png)

  Om over aanvalsimulatie te leren, herzie [Nikto - Scannen](https://github.com/sullo/nikto/wiki/Scan-Tuning) documentatie, die u vertelt hoe te om het type van testaanvallen te specificeren om te omvatten of uit te sluiten.

##### Analyseren

Om de resultaten van aanvalsimulatie te analyseren, volg de zelfde stappen zoals die in [vroeger voorbeeld](#analyzing).

Deze keer moet u echter de **Gemarkeerde aanvragen** en corresponderende waarden in client IP (cli_ip), host, url, action (waf_action) en rule-name (waf_match) kolommen. Met deze informatie kunt u de resultaten analyseren en de regelconfiguratie optimaliseren.

![Aanvraag voor gemarkeerde GOF-markering van ELK-gereedschapdashboard](./assets/elk-tool-dashboard-waf-flagged.png)

Let op het volgende: **WAF-vlagverdeling en topaanvallen** de panelen tonen extra details, die kunnen worden gebruikt om de regelconfiguratie verder te optimaliseren.

![Aanvraag voor WAF-vlaggen van het ELK-hulpprogramma](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![Aanvraag voor bovenste WAF-aanvallen op het ELK-hulpprogramma](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Met WAFFlags

Nu een WAF-regel toevoegen die `wafFlags` eigenschap als onderdeel van het `action` eigendom en **blokkeren de gesimuleerde aanvalsverzoeken**.

Vanuit een syntaxisperspectief zijn de WAF-regels vergelijkbaar met de regels die eerder werden gezien, maar de `action` eigenschapverwijzingen een of meer `wafFlags` waarden. Meer informatie over de `wafFlags`, de [Lijst met WAF-markeringen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list) sectie.

- Voeg de volgende regel in het WKND-project toe `/config/cdn.yaml` bestand. Let op het volgende: `block-waf-flags` de regel omvat enkele wafFlags die in het dashboard tooling toen aangevallen met gesimuleerd kwaadwillig verkeer waren verschenen. Het is inderdaad een goede praktijk om logboeken te analyseren om te bepalen welke nieuwe regels moeten worden verklaard, aangezien het bedreigingslandschap evolueert.

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
            - SIGSCI-IP
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

- Breng, duw, en stel veranderingen zoals vermeld in [eerdere voorbeelden](#logging-requests).

- Om een aanval te simuleren, gebruik het zelfde [Nikto](https://github.com/sullo/nikto) gebruiken zoals voorheen.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Analyseren

Herhaal dezelfde stappen als in het dialoogvenster [vroeger voorbeeld](#analyzing).

Dit keer moet u de vermeldingen onder **Geblokkeerde aanvragen** en de overeenkomstige waarden in client-IP (cli_ip), host, url, action (waf_action) en regel-name (waf_match) kolommen.

![Aanvraag voor geblokkeerde WAF-bestanden van ELK-dashboard](./assets/elk-tool-dashboard-waf-blocked.png)

Ook de **WAF-vlagverdeling en topaanvallen** extra details worden weergegeven in de deelvensters.

![Aanvraag voor WAF-vlaggen van het ELK-hulpprogramma](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![Aanvraag voor bovenste WAF-aanvallen op het ELK-hulpprogramma](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Uitgebreide analyse

In het bovenstaande _analyse_ secties hebt u geleerd hoe u de resultaten van de **specifieke regel** met het dashboardgereedschap. U kunt de resultaten verder analyseren met andere dashboarddeelvensters, zoals:


- Geanalyseerde, gemarkeerde en geblokkeerde aanvragen
- WAF Vlaggen worden in de loop der tijd gedistribueerd
- Regels voor gestuurde verkeersfilters in de loop der tijd
- Bovenste aanvallen door WAF-vlaggen-id
- Bovenste getriggerde verkeersfilter
- De hoogste 100 aanvallers door cliëntIP, land, en gebruiker-agent

![Uitgebreide analyse van het Dashboard van het gereedschap ELK](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![Uitgebreide analyse van het Dashboard van het gereedschap ELK](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## Volgende stap

Ga vertrouwd met aanbevolen [best practices](./best-practices.md) het risico van inbreuken op de beveiliging te verminderen.

## Aanvullende bronnen

[Syntaxis verkeersfilterregels](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[CDN-logboekindeling](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)

