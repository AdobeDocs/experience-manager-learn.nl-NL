---
title: Logboeken
description: De logboeken handelen als frontline voor het zuiveren AEM toepassingen in AEM als Cloud Service, maar zijn afhankelijk van het adequate registreren in de opgestelde AEM toepassing.
feature: Gereedschappen voor ontwikkelaars
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Ontwikkeling
role: Developer
level: Beginner
source-git-commit: e2473a1584ccf315fffe5b93cb6afaed506fdbce
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Fouten opsporen in AEM als Cloud Service met behulp van logbestanden

De logboeken handelen als frontline voor het zuiveren AEM toepassingen in AEM als Cloud Service, maar zijn afhankelijk van het adequate registreren in de opgestelde AEM toepassing.

Alle logboekactiviteit voor de AEM dienst van een bepaalde milieu (de Verzender van de Auteur, van de Publicatie/van de Publicatie) wordt geconsolideerd in één enkel logboekdossier, zelfs als de verschillende pods binnen die dienst de logboekverklaringen produceren.

Pod Ids wordt verstrekt in elke logboekverklaring, en het toestaan van het filtreren of het sorteren van logboekverklaringen. Pod-id&#39;s hebben de volgende indeling:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Voorbeeld: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Aangepaste logbestanden

AEM als Cloud Services steunt geen dossiers van het douanelogboek, nochtans steunt het douane registreren.

Java-logboeken zijn alleen beschikbaar in AEM als Cloud Service (via [Cloud Manager](#cloud-manager) of [Adobe I/O CLI](#aio)) als aangepaste loginstructies zijn geschreven in `error.log`. Logboeken die naar aangepaste benoemde logboeken worden geschreven, zoals `example.log`, zijn niet toegankelijk vanaf AEM als Cloud Service.

Logs kan aan `error.log` worden geschreven gebruikend een Sling LogManager OSGi configuratiebezit in de `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` dossiers van de toepassing.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Logboeken van AEM-auteur- en publicatieservice

Zowel AEM-auteur- als -publicatieservices bieden AEM serverlogboeken bij uitvoering:

+ `aemerror` Dit is het Java-foutenlogboek (wordt gevonden  `/crx-quickstart/logs/error.log` op de lokale QuickStart van de AEM SDK). Hieronder vindt u de [aanbevolen logniveaus](#log-levels) voor aangepaste loggers per omgevingstype:
   + Ontwikkeling: `DEBUG`
   + Werkgebied: `WARN`
   + Productie: `ERROR`
+ `aemaccess` maakt een lijst van HTTP- verzoeken aan de AEM dienst met details
+ `aemrequest` maakt een lijst van HTTP- verzoeken aan AEM dienst en hun overeenkomstige reactie van HTTP

## Logboeken van AEM-publicatieserver

Alleen AEM Publish Dispatcher biedt logboeken van Apache-webservers en Dispatcher, aangezien deze aspecten alleen in de publicatielijst voor AEM staan en niet in de lijst AEM-auteur.

+ `httpdaccess` Hier worden HTTP-aanvragen weergegeven die zijn ingediend bij de Apache-webserver/Dispatcher van de AEM-service.
+ `httperror`  bevat een lijst met logberichten van de Apache-webserver en hulp bij het opsporen van ondersteunde Apache-modules, zoals  `mod_rewrite`.
   + Ontwikkeling: `DEBUG`
   + Werkgebied: `WARN`
   + Productie: `ERROR`
+ `aemdispatcher` maakt een lijst van logboekberichten van de modules van de Verzender, met inbegrip van het filtreren en het dienen van geheim voorgeheugenberichten.
   + Ontwikkeling: `DEBUG`
   + Werkgebied: `WARN`
   + Productie: `ERROR`

## Cloud Manager{#cloud-manager}

Met Adobe Cloud Manager kunt u logbestanden overdag downloaden via de handeling Logbestanden downloaden van een omgeving.

![Cloud Manager - Logbestanden downloaden](./assets/logs/download-logs.png)

Deze logbestanden kunnen worden gedownload en geïnspecteerd met behulp van alle programma&#39;s voor loganalyse.

## Adobe I/O CLI met plug-in Cloud Manager{#aio}

Adobe Cloud Manager ondersteunt toegang tot AEM als een Cloud Service logt via de [Adobe I/O CLI](https://github.com/adobe/aio-cli) met de [Cloud Manager-plug-in voor de Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Stel eerst [de Adobe I/O in met de plug-in van Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Zorg ervoor dat de relevante programma-id en milieu-id zijn geïdentificeerd en gebruik [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) om de logopties weer te geven die worden gebruikt voor [tail](#aio-cli-tail-logs) of [download](#aio-cli-download-logs) logbestanden.

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

Adobe I/O CLI verstrekt de capaciteit om logboeken in real time van AEM als Cloud Service te stallen gebruikend [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) bevel. Tailing is handig voor het bekijken van realtime logactiviteit terwijl acties op de AEM als een Cloud Service-omgeving worden uitgevoerd.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Andere opdrachtregelprogramma&#39;s, zoals `grep`, kunnen in overleg met `tail-logs` worden gebruikt om loginstructies van belang te isoleren, bijvoorbeeld:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... geeft alleen logboekinstructies weer die zijn gegenereerd op basis van `com.example.MySlingModel` of bevat die tekenreeks in die instructies.

### Logbestanden downloaden{#aio-cli-download-logs}

Adobe I/O CLI verstrekt de capaciteit om logboeken van AEM als Cloud Service te downloaden gebruikend [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)) bevel. Dit levert hetzelfde eindresultaat op als het downloaden van de logbestanden vanuit de webinterface van Cloud Manager, waarbij het verschil is dat de opdracht `download-logs` de logbestanden over dagen consolideert op basis van het aantal dagen dat logbestanden worden aangevraagd.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Logboeken begrijpen

Aanmeldt AEM als Cloud Service meerdere pods die logboekinstructies naar deze pods schrijven. Omdat de veelvoudige AEM instanties aan het zelfde logboekdossier schrijven, is het belangrijk om te begrijpen hoe te analyseren, en lawaai te verminderen terwijl het zuiveren. Om uit te leggen, wordt het volgende `aemerror` logboekfragment gebruikt:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Met behulp van de pod-id, het gegevenspunt na de datum en tijd, kunnen de logboeken worden gesorteerd door de pod of AEM instantie binnen de service, waardoor het eenvoudiger wordt om code-uitvoering te traceren en te begrijpen.

__Pod cm-p12345-e56789-aem-auteur-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-auteur-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Aanbevolen logniveaus{#log-levels}

Adobe als Cloud Service

+ Lokale ontwikkeling (AEM SDK): `DEBUG`
+ Ontwikkeling: `DEBUG`
+ Werkgebied: `WARN`
+ Productie: `ERROR`

Het plaatsen van het meest aangewezen logboekniveau voor elk milieutype is met AEM als Cloud Service, de logboekniveaus worden gehandhaafd in code

+ Java-logconfiguraties worden onderhouden in OSGi-configuraties
+ Logniveaus van Apache-webserver en Dispatcher in het verzeneratieproject

...en dus een implementatie vereisen om te wijzigen.

### Omgevingsspecifieke variabelen om Java-logniveaus in te stellen

Als alternatief voor het instellen van statische bekende Java-logniveaus voor elke omgeving kunt u AEM gebruiken als omgevingsspecifieke variabelen [a1/> van de Cloud Service om logniveaus te bepalen, zodat de waarden dynamisch kunnen worden gewijzigd via de [Adobe I/O CLI met de insteekmodule van Cloud Manager](#aio-cli).](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values)

Dit vereist het bijwerken van de registrerenconfiguraties OSGi om de milieu specifieke veranderlijke placeholders te gebruiken. [De standaardwaarden ](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) voor logniveaus moeten worden ingesteld volgens de aanbevelingen [ van ](#log-levels)Adobe. Bijvoorbeeld:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

Deze aanpak heeft nadelen waarmee rekening moet worden gehouden:

+ [Er is een beperkt aantal omgevingsvariabelen toegestaan](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables) en het maken van een variabele voor het beheren van het logniveau maakt gebruik van deze variabelen.
+ Omgevingsvariabelen kunnen alleen via [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) of [Cloud Manager HTTP API&#39;s](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties) via programmacode worden beheerd.
+ Wijzigingen in omgevingsvariabelen moeten handmatig worden hersteld met een ondersteund gereedschap. Het vergeten om een hoog verkeersmilieu, zoals Productie, aan een minder uitgebreid logboekniveau terug te stellen kan de logboeken overstromen en AEM prestaties beïnvloeden.

_Omgevingsspecifieke variabelen werken niet voor Apache-webserver- of Dispatcher-logconfiguraties, omdat deze niet via de OSGi-configuratie worden geconfigureerd._
