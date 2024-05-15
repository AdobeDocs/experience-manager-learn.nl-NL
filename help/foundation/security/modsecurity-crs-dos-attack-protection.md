---
title: Gebruik ModSecurity om uw AEM te beschermen tegen DoS-aanval
description: Leer hoe te om ModSecurity toe te laten om uw plaats tegen Ontkenning van de aanval van de Dienst (Dos) te beschermen gebruikend de Reeks van de Regel van de Kern van de WASPIS ModSecurity (CRS).
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
jira: KT-10385
thumbnail: KT-10385.png
doc-type: Article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
duration: 783
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# Gebruik ModSecurity om uw AEM site te beschermen tegen DoS-aanvallen

Leer hoe te om ModSecurity toe te laten om uw plaats tegen Ontkenning van de aanvallen van de Dienst (Dos) te beschermen gebruikend **OWASP ModSecurity Core Rule Set (CRS)** op de Adobe Experience Manager (AEM) Publish Dispatcher.


>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## Overzicht

De [Open Web Application Security Project® (OWASP)](https://owasp.org/) stichting biedt [**OWASP Top 10**](https://owasp.org/www-project-top-ten/) de tien meest kritieke beveiligingsproblemen voor webtoepassingen worden beschreven.

ModSecurity is een open-source oplossing voor meerdere platforms die bescherming biedt tegen een reeks aanvallen op webtoepassingen. Het staat ook voor het verkeer van HTTP controle, registreren, en analyse in real time toe.

OWSAP® biedt ook [OWASP® ModSecurity Core Rule Set (CRS)](https://github.com/coreruleset/coreruleset). CRS is een reeks generische **aanvalsdetectie** regels voor gebruik met ModSecurity. CRS is er dus op gericht webtoepassingen te beschermen tegen een breed scala aan aanvallen, waaronder de OWASP Top Tien, met een minimum aan valse waarschuwingen.

Deze zelfstudie laat zien hoe u de **DOS-BESCHERMING** CRS-regel om uw site te beschermen tegen een mogelijke DoS-aanval.

>[!TIP]
>
>Het is belangrijk om op te merken, de AEM as a Cloud Service [beheerde CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) voldoet aan de meeste prestatie- en beveiligingsvereisten van de klant. Nochtans, verstrekt ModSecurity een extra laag van veiligheid en staat klant-specifieke regels, en configuraties toe.

## CRS toevoegen aan de projectmodule van de Verzender

1. Download en extraheer de [nieuwste OWASP ModSecurity Core-regelset](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. Maak de `modsec/crs` mappen binnen `dispatcher/src/conf.d/` in de code van uw AEM project. Bijvoorbeeld in de lokale kopie van het dialoogvenster [AEM WKND-siteproject](https://github.com/adobe/aem-guides-wknd).

   ![CRS omslag binnen AEM projectcode - ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. De `coreruleset-X.Y.Z/rules` map van het gedownloade CRS-releasepakket naar het `dispatcher/src/conf.d/modsec/crs` map.
1. De `coreruleset-X.Y.Z/crs-setup.conf.example` bestand van het gedownloade CRS-releasepakket naar het `dispatcher/src/conf.d/modsec/crs` map en hernoemen `crs-setup.conf`.
1. Maak alle gekopieerde regels van CRS van onbruikbaar `dispatcher/src/conf.d/modsec/crs/rules` door ze een andere naam te geven als `XXXX-XXX-XXX.conf.disabled`. U kunt de onderstaande opdrachten gebruiken om de naam van alle bestanden tegelijk te wijzigen.

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   Zie anders genoemd regels CRS en configuratiedossier in de WKND projectcode.

   ![Gehandicapte regels CRS binnen AEM projectcode - ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## De de beschermingsregel van de Ontkenning van de Dienst (Dos) toelaten en vormen

Om de de beschermingsregel van de Ontkenning van de Dienst (Dos) toe te laten en te vormen, volg de volgende stappen:

1. Laat de de beschermingsregel van Dos door anders te noemen toe `REQUEST-912-DOS-PROTECTION.conf.disabled` tot `REQUEST-912-DOS-PROTECTION.conf` (of verwijder de `.disabled` van liniaalextensie) binnen de `dispatcher/src/conf.d/modsec/crs/rules` map.
1. Vorm de regel door te bepalen  **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT** variabelen.
   1. Een `crs-setup.custom.conf` bestand in het `dispatcher/src/conf.d/modsec/crs` map.
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

In deze configuratie van de voorbeeldregel, **DOS_COUNTER_THRESHOLD** is 25, **DOS_BURST_TIME_SLICE** is 60 seconden, en **DOS_BLOCK_TIMEOUT** time-out is 600 seconden. Deze configuratie identificeert meer dan twee voorkomen van 25 verzoeken, exclusief statische dossiers, binnen 60 seconden kwalificeert als aanval van Dos, resulterend in de het verzoeken cliënt om voor 600 seconden (of 10 minuten) te worden geblokkeerd.

>[!WARNING]
>
>Om de aangewezen waarden voor uw behoeften te bepalen, werk met uw team van de Veiligheid van het Web samen.

## CRS initialiseren

Om CRS te initialiseren, verwijder gemeenschappelijke valse positieven, en voeg lokale uitzonderingen voor uw plaats toe volg de onderstaande stappen:

1. Als u de CRS wilt initialiseren, verwijdert u `.disabled` van de **VERZOEK-901-INITIALISATIE** bestand. Met andere woorden, hernoem de `REQUEST-901-INITIALIZATION.conf.disabled` bestand naar `REQUEST-901-INITIALIZATION.conf`.
1. Om gemeenschappelijke valse positieven zoals lokale IP (127.0.0.1) te verwijderen pingelt, verwijdert `.disabled` van de **VERZOEK-905-GEMEENSCHAPPELIJKE UITZONDERINGEN** bestand.
1. Als u lokale uitzonderingen wilt toevoegen, zoals het AEM platform of uw sitespecifieke paden, wijzigt u de naam van de `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` tot `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. Voeg AEM platformspecifieke weguitzonderingen aan het onlangs anders genoemde dossier toe.

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

1. Verwijder ook de `.disabled` van **REQUEST-910-IP-REPUTATION.conf.disabled** voor IP de controle van het reputatieblok en `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` voor een afwijkende score.

>[!TIP]
>
>Wanneer het vormen op AEM 6.5 zorg ervoor om de bovengenoemde wegen met respectieve AMS of op-prem wegen te vervangen die de gezondheid van de AEM (alias hartslagwegen) verifiëren.

## ModSecurity Apache-configuratie toevoegen

ModSecurity inschakelen (ook bekend als `mod_security` Apache module), voert u de volgende stappen uit:

1. Maken `modsecurity.conf` om `dispatcher/src/conf.d/modsec/modsecurity.conf` met de onderstaande sleutelconfiguraties.

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

1. Selecteer het gewenste `.vhost` van de Dispatcher-module van uw AEM project `dispatcher/src/conf.d/available_vhosts`, bijvoorbeeld `wknd.vhost`, voeg de onderstaande vermelding toe buiten de `<VirtualHost>` blokkeren.

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

Alle bovenstaande _ModSecurity CRS_ en _DOS-BESCHERMING_ configuraties zijn beschikbaar in de AEM WKND-siteprojecten [zelfstudie/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) vertakking voor de revisie.

### Configuratie van Dispatcher valideren

Wanneer het werken met AEM as a Cloud Service, alvorens uw _Dispatcher-configuratie_ wijzigingen aanbrengt, wordt aangeraden deze lokaal te valideren met `validate` script van het [AEM de Dispatcher Tools van SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## Implementeren

De lokaal gevalideerde Dispatcher-configuraties implementeren met Cloud Manager [Webniveau](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#web-tier-config) of [Volledige stapel](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#full-stack-code) pijpleiding. U kunt ook de opdracht [Snelle ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) voor een snellere doorlooptijd.

## Verifiëren

Om de bescherming van Dos, in dit voorbeeld te verifiëren, verzenden meer dan 50 verzoeken (25 verzoekdrempel keer twee voorkomen) binnen een spanwijdte van 60 seconden. Deze verzoeken moeten echter door de AEM as a Cloud Service [ingebouwd](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html) of [andere CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?#point-to-point-CDN) fronting van uw website.

Één techniek om CDN over te gaan is een vraagparameter met toe te voegen **nieuwe willekeurige waarde op elke aanvraag voor een sitepagina**.

Om een groter aantal verzoeken (50 of meer) binnen een korte periode (zoals 60 seconden) teweeg te brengen, Apache [JMeter](https://jmeter.apache.org/) of [Gereedschap Benchmark of Ab](https://httpd.apache.org/docs/2.4/programs/ab.html) kan worden gebruikt.

### DoS-aanval simuleren met JMeter-script

Volg de onderstaande stappen om een DoS-aanval met JMeter te simuleren:

1. [Apache JMeter downloaden](https://jmeter.apache.org/download_jmeter.cgi) en [installeren](https://jmeter.apache.org/usermanual/get-started.html#install) lokaal
1. [Uitvoeren](https://jmeter.apache.org/usermanual/get-started.html#running) het lokaal gebruiken van `jmeter` script van het `<JMETER-INSTALL-DIR>/bin` directory.
1. Het voorbeeld openen [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) JMX-script naar JMeter met het **Openen** menu.

   ![Voorbeeld van JMX Test Script koppelen WKND DoS - ModSecurity openen](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. Werk de **Servernaam of IP** veldwaarde in _Startpagina_ en _Adventure-pagina_ HTTP-aanvraagsampler die overeenkomt met de URL van de AEM. Bekijk andere details van het JMeter-voorbeeldscript.

   ![HTTP-aanvraag JMetere - ModSecurity AEM](assets/modsecurity-crs/aem-server-name-http-request.png)

1. Voer het script uit door op de knop **Start** in het menu Gereedschap. Het manuscript verzendt 50 HTTP- verzoeken (5 gebruikers en 10 lijnaantallen) tegen de plaats WKND _Startpagina_ en _Adventure-pagina_. Aldus een totaal van 100 verzoeken aan niet-statische dossiers, kwalificeert het de aanval van Dos per **DOS-BESCHERMING** Aangepaste configuratie van de CRS-regel.

   ![JavaScript uitvoeren - ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. De **Resultaten in tabel weergeven** JMeter-listeners **Mislukt** antwoordstatus voor aanvraagnummer ~ 53 en daarna.

   ![Respons mislukt in weergaveresultaten in Tabel JMeter - ModSecurity](assets/modsecurity-crs/failed-response-jmeter.png)

1. De **503 HTTP-antwoordcode** wordt geretourneerd voor de mislukte aanvragen, kunt u de details weergeven met de **De resultatenstructuur weergeven** JMeter-listener.

   ![503 Response JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### Revisielogboeken

De modSecurity logger configuratie registreert de details van het de aanvalsincident van Dos. Voer de volgende stappen uit om de details weer te geven:

1. Download en open de `httpderror` logbestand van de **Dispatcher publiceren**.
1. Woord zoeken `burst` in het logbestand om de **fout** lijnen

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. Bekijk de details zoals _IP-adres client_, actie, foutbericht en aanvraagdetails.

## Prestatieeffect van ModSecurity

Het toelaten van ModSecurity en de bijbehorende regels heeft sommige prestatiesimplicaties, zodat ben bewust van welke regels worden vereist, overtollig, en overgeslagen. De partner met uw deskundigen van de Veiligheid van het Web om, de regels toe te laten en aan te passen CRS.

### Aanvullende regels

Deze zelfstudie schakelt het **DOS-BESCHERMING** CRS-regel voor demonstratie. Het wordt geadviseerd om met de deskundigen van de Veiligheid van het Web samen te werken om, aangewezen regels te begrijpen te herzien en te vormen.
