---
title: Gebruik ModSecurity om uw AEM-site te beschermen tegen DoS-aanval
description: Leer hoe te om ModSecurity toe te laten om uw plaats tegen Ontkenning van de aanval van de Dienst (Dos) te beschermen gebruikend de Reeks van de Regel van de Kern van OWASP ModSecurity (CRS).
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Developer
level: Experienced
jira: KT-10385
thumbnail: KT-10385.png
doc-type: Article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
duration: 783
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1171'
ht-degree: 0%

---

# Gebruik ModSecurity om uw AEM-site te beschermen tegen DoS-aanvallen

Leer hoe te om ModSecurity toe te laten om uw plaats tegen Ontkenning van de aanvallen van de Dienst (Dos) te beschermen gebruikend de **Reeks van de Regel van de Kern van OWASP ModSecurity (CRS)** op Adobe Experience Manager (AEM) publiceer Dispatcher.


>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## Overzicht

De [&#x200B; Open stichting van de Veiligheid Project® van de Toepassing van het Web (OWASP) &#x200B;](https://owasp.org/) verstrekt de [**Hoogste 10 van OWASP** &#x200B;](https://owasp.org/www-project-top-ten/) schetterend de tien meest kritieke veiligheidszorgen voor Webtoepassingen.

ModSecurity is een open-source oplossing voor meerdere platforms die bescherming biedt tegen een reeks aanvallen op webtoepassingen. Het staat ook voor het verkeer van HTTP controle, registreren, en analyse in real time toe.

OWSAP® verstrekt ook de [&#x200B; Reeks van de Regel van de Kern OWASP® ModSecurity (CRS) &#x200B;](https://github.com/coreruleset/coreruleset). CRS is een reeks generische **aanvalsopsporing** regels voor gebruik met ModSecurity. CRS is er dus op gericht webtoepassingen te beschermen tegen een breed scala van aanvallen, waaronder de Top Ten van OWASP, met een minimum aan foute waarschuwingen.

Dit leerprogramma toont aan hoe te om **DOS-BESCHERMING** regel van CRS toe te laten en te vormen om uw plaats tegen een potentiële aanval van Dos te beschermen.

>[!TIP]
>
>Het is belangrijk om nota te nemen, voldoet AEM as a Cloud Service [&#x200B; beheerde CDN &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=nl-NL) aan de prestaties en de veiligheidsvereisten van de meeste klant. Nochtans, verstrekt ModSecurity een extra laag van veiligheid en staat klant-specifieke regels, en configuraties toe.

## CRS toevoegen aan Dispatcher-projectmodule

1. De download en haalt [&#x200B; recentste Reeks van de Regel van de Kern van OWASP ModSecurity &#x200B;](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. Maak de `modsec/crs` mappen in `dispatcher/src/conf.d/` in de code van uw AEM-project. Bijvoorbeeld, in het lokale exemplaar van het [&#x200B; project van de Plaatsen van AEM WKND &#x200B;](https://github.com/adobe/aem-guides-wknd).

   ![&#x200B; omslag CRS binnen het projectcode van AEM - ModSecurity &#x200B;](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. Kopieer de map `coreruleset-X.Y.Z/rules` van het gedownloade CRS-releasepakket naar de map `dispatcher/src/conf.d/modsec/crs` .
1. Kopieer het `coreruleset-X.Y.Z/crs-setup.conf.example` -bestand van het gedownloade CRS-releasepakket naar de `dispatcher/src/conf.d/modsec/crs` -map en geef het bestand een andere naam in `crs-setup.conf` .
1. Schakel alle gekopieerde CRS-regels uit vanuit de `dispatcher/src/conf.d/modsec/crs/rules` door ze een andere naam te geven als `XXXX-XXX-XXX.conf.disabled` . U kunt de onderstaande opdrachten gebruiken om de naam van alle bestanden tegelijk te wijzigen.

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   Zie anders genoemd regels CRS en configuratiedossier in de WKND projectcode.

   ![&#x200B; Gehandicapte regels CRS binnen het projectcode van AEM - ModSecurity &#x200B;](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## De de beschermingsregel van de Ontkenning van de Dienst (Dos) toelaten en vormen

Om de de beschermingsregel van de Ontkenning van de Dienst (Dos) toe te laten en te vormen, volg de volgende stappen:

1. Schakel de beveiligingsregel van het besturingssysteem in door de naam van de `REQUEST-912-DOS-PROTECTION.conf.disabled` te wijzigen in `REQUEST-912-DOS-PROTECTION.conf` (of de `.disabled` uit de extensie van de liniaal te verwijderen) in de map `dispatcher/src/conf.d/modsec/crs/rules` .
1. Vorm de regel door **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT** variabelen te bepalen.
   1. Maak een `crs-setup.custom.conf` -bestand in de map `dispatcher/src/conf.d/modsec/crs` .
   1. Voeg het onderstaande regelfragment toe aan het nieuwe bestand.

   ```
   # The Denial of Service (DoS) protection against clients making requests too quickly.
   # When a client is making more than 25 requests (excluding static files) within
   # 60 seconds, this is considered a 'burst'. After two bursts, the client is
   # blocked for 600 seconds.
   SecAction \
       "id:900700,\
       phase:1,\
       nolog,\
       pass,\
       t:none,\
       setvar:'tx.dos_burst_time_slice=60',\
       setvar:'tx.dos_counter_threshold=25',\
       setvar:'tx.dos_block_timeout=600'"    
   ```

In deze configuratie van de voorbeeldregel, **DOS_COUNTER_THRESHOLD** is 25, **DOS_BURST_TIME_SLICE** is 60 seconden, en **DOS_BLOCK_TIMEOUT** onderbreking is 600 seconden. Deze configuratie identificeert meer dan twee voorkomen van 25 verzoeken, exclusief statische dossiers, binnen 60 seconden kwalificeert als aanval van Dos, resulterend in de het verzoeken cliënt om voor 600 seconden (of 10 minuten) te worden geblokkeerd.

>[!WARNING]
>
>Om de aangewezen waarden voor uw behoeften te bepalen, werk met uw team van de Veiligheid van het Web samen.

## CRS initialiseren

Om CRS te initialiseren, verwijder gemeenschappelijke valse positieven, en voeg lokale uitzonderingen voor uw plaats toe volg de onderstaande stappen:

1. Om CRS te initialiseren, verwijder `.disabled` uit het **VERZOEK-901-INITIALIZATION** dossier. Met andere woorden, wijzig de naam van het `REQUEST-901-INITIALIZATION.conf.disabled` -bestand in `REQUEST-901-INITIALIZATION.conf` .
1. Om gemeenschappelijke valse positieven zoals lokale IP (127.0.0.1) te verwijderen pingelt, verwijder `.disabled` uit het **VERZOEK-905-COMMON-UITZONDERINGEN** dossier.
1. Als u lokale uitzonderingen wilt toevoegen, zoals het AEM-platform of uw sitespecifieke paden, wijzigt u de naam `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` in `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. Voeg platformspecifieke weguitzonderingen van AEM aan het onlangs anders genoemde dossier toe.

   ```
   ########################################################
   # AEM as a Cloud Service exclusions                    #
   ########################################################
   # Ignoring AEM-CS Specific internal and reserved paths
   
   SecRule REQUEST_URI "@beginsWith /systemready" \
       "id:1010,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"    
   
   SecRule REQUEST_URI "@beginsWith /system/probes" \
       "id:1011,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   SecRule REQUEST_URI "@beginsWith /gitinit-status" \
       "id:1012,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   ########################################################
   # ADD YOUR SITE related exclusions                     #
   ########################################################
   ...
   ```

1. Ook, verwijder `.disabled` uit **VERZOEK-910-IP-REPUTATION.conf.disabled** voor IP de controle van het reputatieblok en `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` voor anomalie scorecontrole.

>[!TIP]
>
>Zorg er bij de configuratie op AEM 6.5 voor dat u de bovenstaande paden vervangt door de respectievelijke AMS- of on-prem-paden die de status van de AEM (ook wel bekend als hartslagpaden) controleren.

## ModSecurity Apache-configuratie toevoegen

Voer de volgende stappen uit om ModSecurity (ook wel `mod_security` Apache-module genoemd) in te schakelen:

1. Maak `modsecurity.conf` op `dispatcher/src/conf.d/modsec/modsecurity.conf` met de onderstaande sleutelconfiguraties.

   ```
   # Include the baseline crs setup
   Include conf.d/modsec/crs/crs-setup.conf
   
   # Include your customizations to crs setup if exist
   IncludeOptional conf.d/modsec/crs/crs-setup.custom.conf
   
   # Select all available CRS rules:
   #Include conf.d/modsec/crs/rules/*.conf
   
   # Or alternatively list only specific ones you want to enable e.g.
   Include conf.d/modsec/crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   Include conf.d/modsec/crs/rules/REQUEST-901-INITIALIZATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-905-COMMON-EXCEPTIONS.conf
   Include conf.d/modsec/crs/rules/REQUEST-910-IP-REPUTATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf
   Include conf.d/modsec/crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
   
   # Start initially with engine off, then switch to detection and observe, and when sure enable engine actions
   #SecRuleEngine Off
   #SecRuleEngine DetectionOnly
   SecRuleEngine On
   
   # Remember to use relative path for logs:
   SecDebugLog logs/httpd_mod_security_debug.log
   
   # Start with low debug level
   SecDebugLogLevel 0
   #SecDebugLogLevel 1
   
   # Start without auditing
   SecAuditEngine Off
   #SecAuditEngine RelevantOnly
   #SecAuditEngine On
   
   # Tune audit accordingly:
   SecAuditLogRelevantStatus "^(?:5|4(?!04))"
   SecAuditLogParts ABIJDEFHZ
   SecAuditLogType Serial
   
   # Remember to use relative path for logs:
   SecAuditLog logs/httpd_mod_security_audit.log
   
   # You might still use /tmp for temporary/work files:
   SecTmpDir /tmp
   SecDataDir /tmp
   ```

1. Selecteer de gewenste `.vhost` in de Dispatcher-module van uw AEM-project `dispatcher/src/conf.d/available_vhosts` , `wknd.vhost` bijvoorbeeld, en voeg de onderstaande vermelding toe buiten het `<VirtualHost>` -blok.

   ```
   # Enable the ModSecurity and OWASP CRS
   <IfModule mod_security2.c>
       Include conf.d/modsec/modsecurity.conf
   </IfModule>
   
   ...
   
   <VirtualHost *:80>
       ServerName    "publish"
       ...
   </VirtualHost>
   ```

Al bovengenoemde _CRS ModSecurity_ en _DOS-BESCHERMING_ configuraties zijn beschikbaar op de 4&rbrace; tutorial/enable-modsecurity-crs-bescherming van het Project van de Plaatsen van AEM WKND [&#x200B; tak voor uw overzicht.](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection)

### Dispatcher-configuratie valideren

Wanneer het werken met AEM as a Cloud Service, alvorens uw _configuratie van Dispatcher_ veranderingen op te stellen, wordt het geadviseerd om hen plaatselijk te bevestigen gebruikend `validate` manuscript van [&#x200B; AEM SDK Dispatcher Tools &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=nl-NL).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## Implementeren

Stel de plaatselijk bevestigde configuraties van Dispatcher op gebruikend de Cloud Manager [&#x200B; Rij van het Web &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=nl-NL&#web-tier-config) of [&#x200B; Volledige pijpleiding van de Stapel &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=nl-NL&#full-stack-code). U kunt het [&#x200B; Snelle Milieu van de Ontwikkeling &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=nl-NL) voor snellere het doorgeven tijd ook gebruiken.

## Verifiëren

Om de bescherming van Dos, in dit voorbeeld te verifiëren, verzenden meer dan 50 verzoeken (25 verzoekdrempel keer twee voorkomen) binnen een spanwijdte van 60 seconden. Nochtans, zouden deze verzoeken door AEM as a Cloud Service [&#x200B; moeten overgaan ingebouwd &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=nl-NL) of om het even welk [&#x200B; andere CDN &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=nl-NL&#point-to-point-CDN) die uw website vooraf gaat.

Één techniek om CDN over:geven-door te bereiken moet een vraagparameter met a **nieuwe willekeurige waarde op elk verzoek van de plaatspagina toevoegen**.

Om een groter aantal verzoeken (50 of meer) binnen een korte periode (als 60 seconden) teweeg te brengen, kan Apache [&#x200B; JMeter &#x200B;](https://jmeter.apache.org/) of [&#x200B; Benchmark of het hulpmiddel van Ab &#x200B;](https://httpd.apache.org/docs/2.4/programs/ab.html) worden gebruikt.

### DoS-aanval simuleren met JMeter-script

Volg de onderstaande stappen om een DoS-aanval met JMeter te simuleren:

1. [&#x200B; download Apache JMeter &#x200B;](https://jmeter.apache.org/download_jmeter.cgi) en [&#x200B; installeer &#x200B;](https://jmeter.apache.org/usermanual/get-started.html#install) het plaatselijk
1. [&#x200B; Looppas &#x200B;](https://jmeter.apache.org/usermanual/get-started.html#running) het plaatselijk gebruikend het `jmeter` manuscript van de `<JMETER-INSTALL-DIR>/bin` folder.
1. Open het steekproef [&#x200B; WKND-DoS-Attack-Simulation-Test &#x200B;](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) JMX manuscript in JMeter gebruikend het **Open** hulpmiddelmenu.

   ![&#x200B; open steekproefWKND JMX van de Test JMX- ModSecurity &#x200B;](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. Werk de **Naam van de Server of IP** gebiedswaarde in _de Pagina van het Huis_ en _de monsternemer van het Verzoek van de Pagina van het avontuur_ HTTP bij die uw milieu URL van de testAEM aanpassen. Bekijk andere details van het JMeter-voorbeeldscript.

   ![&#x200B; HTTP- Verzoek JMetere van de Naam van de Server van AEM - ModSecurity &#x200B;](assets/modsecurity-crs/aem-server-name-http-request.png)

1. Voer het manuscript uit door de **knoop van het Begin** van het hulpmiddelmenu te drukken. Het manuscript verzendt 50 HTTP- verzoeken (5 gebruikers en 10 lijnaantallen) tegen de WebND- plaats _Pagina van het Huis_ en _Pagina van het avontuur_. Aldus een totaal van 100 verzoeken aan niet-statische dossiers, kwalificeert het de aanval van Dos per **DOS-BESCHERMING** de regeldouaneconfiguratie van CRS.

   ![&#x200B; voert Manuscript JMeter uit - ModSecurity &#x200B;](assets/modsecurity-crs/execute-jmeter-script.png)

1. De **Resultaten van de Mening in Lijst** JMeter luisteraar toont **Ontbroken** reactiestatus voor verzoekaantal ~ 53 en verder.

   ![&#x200B; Ontbroken Reactie in Resultaten van de Mening in Lijst JMeter - ModSecurity &#x200B;](assets/modsecurity-crs/failed-response-jmeter.png)

1. De **503 code van de Reactie van HTTP** is teruggekeerd voor de ontbroken verzoeken, kunt u de details bekijken gebruikend de **boom van de Resultaten van de Mening** JMeter luisteraar.

   ![&#x200B; 503 Reactie JMeter - ModSecurity &#x200B;](assets/modsecurity-crs/503-response-jmeter.png)

### Revisielogboeken

De modSecurity logger configuratie registreert de details van het de aanvalsincident van Dos. Voer de volgende stappen uit om de details weer te geven:

1. Download en open het `httpderror` logboekdossier van **publiceer Dispatcher**.
1. Onderzoek naar woord `burst` in het logboekdossier, om de **fout** lijnen te zien

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. Herzie de details als _cliëntIP adres_, actie, foutenmelding, en verzoekdetails.

## Prestatieeffect van ModSecurity

Het toelaten van ModSecurity en de bijbehorende regels heeft sommige prestatiesimplicaties, zodat ben bewust van welke regels worden vereist, overtollig, en overgeslagen. De partner met uw deskundigen van de Veiligheid van het Web om, de regels toe te laten en aan te passen CRS.

### Aanvullende regels

Dit leerprogramma laat slechts toe en past de **DOS-BESCHERMING** regel van CRS voor demonstratiedoeleinden aan. Het wordt geadviseerd om met de deskundigen van de Veiligheid van het Web samen te werken om, aangewezen regels te begrijpen te herzien en te vormen.
