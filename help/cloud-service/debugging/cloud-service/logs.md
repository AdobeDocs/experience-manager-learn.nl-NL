---
title: Logboeken
description: Logs fungeert als frontline voor foutopsporing in AEM-toepassingen in AEM as a Cloud Service, maar is afhankelijk van een adequate aanmelding in de geïmplementeerde AEM-toepassing.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 229
source-git-commit: 0363505b426d6e4733c57409e17e9d69f7a567c7
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 0%

---

# Foutopsporing in AEM as a Cloud Service met behulp van logbestanden

Logs fungeert als frontline voor foutopsporing in AEM-toepassingen in AEM as a Cloud Service, maar is afhankelijk van een adequate aanmelding in de geïmplementeerde AEM-toepassing.

Alle logboekactiviteit voor de dienst van AEM van een bepaalde milieu (Auteur, Publish/Publish Dispatcher) wordt geconsolideerd in één enkel logboekdossier, zelfs als de verschillende pods binnen die dienst de logboekverklaringen produceren.

Pod Ids wordt verstrekt in elke logboekverklaring, en het toestaan van het filtreren of het sorteren van logboekverklaringen. Pod-id&#39;s hebben de volgende indeling:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Voorbeeld: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Aangepaste logbestanden

AEM als Cloud Services biedt geen ondersteuning voor aangepaste logbestanden, maar biedt wel ondersteuning voor aangepaste logboekregistratie.

Voor de logboeken van Java om in AEM as a Cloud Service (via [&#x200B; Cloud Manager &#x200B;](#cloud-manager) of [&#x200B; Adobe I/O CLI &#x200B;](#aio)) beschikbaar te zijn, moeten de verklaringen van het douanelogboek worden geschreven `error.log`. Logbestanden die naar aangepaste benoemde logboeken worden geschreven, zoals `example.log` , zijn niet toegankelijk vanuit AEM as a Cloud Service.

Logbestanden kunnen naar de `error.log` worden geschreven met behulp van een Sling LogManager OSGi-configuratieeigenschap in de `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` -bestanden van de toepassing.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM Author and Publish service logs

Zowel AEM-auteur- als -publicatieservices bieden AEM-runtimeserverlogboeken:

+ `aemerror` is het Java-foutenlogboek (dit wordt gevonden op `/crx-quickstart/logs/error.log` in de lokale QuickStart van AEM SDK). Het volgende is de [&#x200B; geadviseerde logboekniveaus &#x200B;](#log-levels) voor douaneloggers per milieutype:
   + Ontwikkeling: `DEBUG`
   + Werkgebied: `WARN`
   + Productie: `ERROR`
+ `aemaccess` geeft een overzicht van HTTP-aanvragen bij de AEM-service met details
+ `aemrequest` geeft een lijst weer van HTTP-aanvragen die zijn ingediend bij de AEM-service en de corresponderende HTTP-respons

## AEM Publish Dispatcher logs

Alleen AEM Publish Dispatcher biedt Apache-webserver en Dispatcher-logbestanden, aangezien deze aspecten alleen bestaan in de AEM Publish-laag en niet in de AEM Author-laag.

+ `httpdaccess` geeft een lijst weer van HTTP-aanvragen die zijn ingediend bij de Apache-webserver/Dispatcher van de AEM-service.
+ `httperror` geeft een lijst weer van logberichten van de Apache-webserver en hulp bij het opsporen van ondersteunde Apache-modules, zoals `mod_rewrite` .
   + Ontwikkeling: `DEBUG`
   + Werkgebied: `WARN`
   + Productie: `ERROR`
+ `aemdispatcher` geeft een lijst weer van logboekberichten van de Dispatcher-modules, inclusief het filteren en verzenden van cacheberichten.
   + Ontwikkeling: `DEBUG`
   + Werkgebied: `WARN`
   + Productie: `ERROR`

## Cloud Manager{#cloud-manager}

Met Adobe Cloud Manager kunt u logbestanden overdag downloaden via de actie Download Logs van een omgeving.

![&#x200B; Cloud Manager - de Logboeken van de Download &#x200B;](./assets/logs/download-logs.png)

Deze logbestanden kunnen worden gedownload en geïnspecteerd met behulp van alle programma&#39;s voor loganalyse.

## Adobe I/O CLI met Cloud Manager-insteekmodule{#aio}

Adobe Cloud Manager steunt de toegang tot van AEM as a Cloud Service logboeken via [&#x200B; Adobe I/O CLI &#x200B;](https://github.com/adobe/aio-cli) met de [&#x200B; stop van Cloud Manager voor Adobe I/O CLI &#x200B;](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Eerst, [&#x200B; opstelling Adobe I/O met de stop van Cloud Manager &#x200B;](../../local-development-environment/development-tools.md#aio-cli).

Verzeker relevante identiteitskaart van het Programma en Milieu ID zijn geïdentificeerd, en gebruik [&#x200B; lijst-beschikbaar-logboek-opties &#x200B;](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) om van de logboekopties een lijst te maken die aan [&#x200B; staart &#x200B;](#aio-cli-tail-logs) of [&#x200B; download &#x200B;](#aio-cli-download-logs) logboeken worden gebruikt.

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### Logboeken voor trainingen{#aio-cli-tail-logs}

Adobe I/O CLI verstrekt de capaciteit om logboeken in real time van AEM as a Cloud Service te staart gebruikend het [&#x200B; staart-logboeken &#x200B;](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) bevel. Tailing is handig voor het bekijken van realtime logactiviteiten terwijl acties worden uitgevoerd in de AEM as a Cloud Service-omgeving.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Andere opdrachtregelprogramma&#39;s, zoals `grep` , kunnen samen met `tail-logs` worden gebruikt om logbestandinstructies te isoleren, bijvoorbeeld:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... geeft alleen loginstructies weer die zijn gegenereerd vanuit `com.example.MySlingModel` of bevatten die tekenreeks in die instructies.

### Logbestanden downloaden{#aio-cli-download-logs}

Adobe I/O CLI verstrekt de capaciteit om logboeken van AEM as a Cloud Service te downloaden gebruikend het [&#x200B; download-logboeken &#x200B;](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)) bevel. Dit levert hetzelfde eindresultaat op als het downloaden van de logbestanden vanuit de Cloud Manager-webinterface. Het verschil is dat met de opdracht `download-logs` de logbestanden over dagen worden geconsolideerd op basis van het aantal dagen dat logbestanden worden aangevraagd.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Logboeken begrijpen

Logboeken in AEM as a Cloud Service bevatten meerdere pods waarin loginstructies worden geschreven. Omdat meerdere AEM-instanties naar hetzelfde logbestand schrijven, is het belangrijk dat u begrijpt hoe u de ruis kunt analyseren en verminderen tijdens het opsporen van fouten. Voor de uitleg wordt het volgende `aemerror` -logfragment gebruikt:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Met behulp van de pod-id, het gegevenspunt na de datum en tijd, kunnen de logboeken worden gesorteerd door de pod of door een AEM-instantie binnen de service, waardoor het eenvoudiger wordt om code-uitvoering te traceren en te begrijpen.

__pod cm-p12345-e56789-aem-auteur-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-auteur-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Aanbevolen logniveaus{#log-levels}

De algemene richtlijnen van Adobe voor logniveaus per AEM as a Cloud Service-omgeving moeten worden nageleefd met de standaardloginstellingen van AEM (met het standaardlogniveau `INFO`). Adobe raadt aan dat ook de aangepaste code van instrumenten wordt voorzien met loginstructies, waardoor deze kan worden uitgevoerd met het logniveau van `INFO` . De logboekniveaus worden gehandhaafd in code

+ Java-logconfiguraties worden onderhouden in OSGi-configuraties
+ Apache-webserver en Dispatcher-logniveaus in het verzendingsproject

...en dus een implementatie vereisen om te wijzigen.

### Omgevingsspecifieke variabelen om Java-logniveaus in te stellen

Een alternatief aan het plaatsen van statische bekende het logboekniveaus van Java voor elk milieu moet AEM gebruiken als Cloud Service [&#x200B; milieu specifieke variabelen &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=nl-NL#environment-specific-configuration-values) om logboekniveaus van parameters te bepalen, toestaand de waarden om dynamisch via [&#x200B; Adobe I/O CLI met de stop van Cloud Manager &#x200B;](#aio-cli) worden veranderd.

Dit vereist het bijwerken van de registrerenconfiguraties OSGi om de milieu specifieke veranderlijke placeholders te gebruiken. [&#x200B; Standaardwaarden &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=nl-NL#default-values) voor logboekniveaus zouden moeten worden geplaatst zoals per [&#x200B; aanbevelingen van Adobe &#x200B;](#log-levels). Bijvoorbeeld:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Deze aanpak heeft nadelen waarmee rekening moet worden gehouden:

+ [&#x200B; Een beperkt aantal milieuvariabelen wordt toegestaan &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=nl-NL#number-of-variables), en het creëren van een variabele om het logboekniveau te beheren zal gebruiken.
+ De variabelen van het milieu kunnen programmatically via [&#x200B; Cloud Manager &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=nl-NL), [&#x200B; CLI van Adobe I/O &#x200B;](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid), en [&#x200B; HTTP APIs van Cloud Manager &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=nl-NL#cloud-manager-api-format-for-setting-properties) worden beheerd.
+ Wijzigingen in omgevingsvariabelen moeten handmatig worden hersteld met een ondersteund gereedschap. Het vergeten om een hoog verkeersmilieu, zoals Productie, aan een minder uitgebreid logboekniveau terug te stellen kan de logboeken overstromen en de prestaties van AEM beïnvloeden.

_Milieu specifieke variabelen werken niet voor Apache Webserver of het logboekconfiguraties van Dispatcher aangezien deze niet via configuratie OSGi worden gevormd._
