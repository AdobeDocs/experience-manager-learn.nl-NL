---
title: Logboeken
description: De logboeken handelen als frontline voor het zuiveren AEM toepassingen in AEM as a Cloud Service, maar zijn afhankelijk van het adequate registreren in de opgestelde AEM toepassing.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 1%

---

# Foutopsporing AEM as a Cloud Service met logbestanden

De logboeken handelen als frontline voor het zuiveren AEM toepassingen in AEM as a Cloud Service, maar zijn afhankelijk van het adequate registreren in de opgestelde AEM toepassing.

Alle logboekactiviteit voor de AEM dienst van een bepaalde milieu (de Verzender van de Auteur, van de Publicatie/van de Publicatie) wordt geconsolideerd in één enkel logboekdossier, zelfs als de verschillende pods binnen die dienst de logboekverklaringen produceren.

Pod Ids wordt verstrekt in elke logboekverklaring, en het toestaan van het filtreren of het sorteren van logboekverklaringen. Pod-id&#39;s hebben de volgende indeling:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Voorbeeld: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Aangepaste logbestanden

AEM als Cloud Servicen steunt geen dossiers van het douanelogboek, nochtans steunt het douane registreren.

Java-logboeken zijn beschikbaar in AEM as a Cloud Service (via [Cloud Manager](#cloud-manager) of [ADOBE I/O CLI](#aio)), moeten aangepaste loginstructies worden geschreven in de `error.log`. Logboeken die naar aangepaste benoemde logboeken worden geschreven, zoals `example.log`, is niet toegankelijk vanuit AEM as a Cloud Service.

U kunt logbestanden schrijven naar de `error.log` het gebruiken van een Sling LogManager OSGi configuratiebezit in de toepassing `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` bestanden.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM Auteur- en publicatieservice

Zowel AEM auteur- als publicatieservices bieden AEM serverlogboeken bij uitvoering:

+ `aemerror` is het Java-foutenlogboek (te vinden op `/crx-quickstart/logs/error.log` op de AEM SDK (lokale QuickStart). Het volgende is [aanbevolen logniveaus](#log-levels) voor aangepaste loggers per omgevingstype:
   + Ontwikkeling: `DEBUG`
   + Werkgebied: `WARN`
   + Productie: `ERROR`
+ `aemaccess` maakt een lijst van HTTP- verzoeken aan de AEM dienst met details
+ `aemrequest` maakt een lijst van HTTP- verzoeken aan AEM dienst en hun overeenkomstige reactie van HTTP

## Logboeken AEM van Dispatcher publiceren

Alleen AEM Publish Dispatcher biedt logboeken van Apache-webservers en Dispatcher, aangezien deze aspecten alleen bestaan in de AEM Publish-laag en niet in de AEM Auteur-laag.

+ `httpdaccess` bevat een lijst met HTTP-aanvragen die zijn ingediend bij de Apache-webserver/Dispatcher van de AEM service.
+ `httperror`  bevat een lijst met logberichten van de Apache-webserver en Help bij foutopsporing in ondersteunde Apache-modules, zoals `mod_rewrite`.
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

Adobe Cloud Manager ondersteunt toegang tot AEM as a Cloud Service logbestanden via [ADOBE I/O CLI](https://github.com/adobe/aio-cli) met de [Cloud Manager-insteekmodule voor de Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Eerste, [De Adobe I/O instellen met de plug-in Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Ervoor zorgen dat de relevante programma-id en milieu-id zijn geïdentificeerd en worden gebruikt [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) om een lijst te maken van de logboekopties die worden gebruikt om [staart](#aio-cli-tail-logs) of [downloaden](#aio-cli-download-logs) logboeken.

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

Adobe I/O CLI verstrekt de capaciteit om logboeken in real time van AEM as a Cloud Service te stallen gebruikend het [staart-logboeken](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) gebruiken. Tailing is handig voor het bekijken van realtime logactiviteit terwijl acties worden uitgevoerd op de AEM as a Cloud Service omgeving.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Andere opdrachtregelprogramma&#39;s, zoals `grep` kan worden gebruikt in overleg met `tail-logs` om logverklaringen van belang te helpen isoleren, bijvoorbeeld:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... geeft alleen logboekinstructies weer die zijn gegenereerd van `com.example.MySlingModel` of bevat die tekenreeks erin.

### Logbestanden downloaden{#aio-cli-download-logs}

Adobe I/O CLI verstrekt de capaciteit logboeken van AEM as a Cloud Service downloaden gebruikend [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Dit levert hetzelfde eindresultaat op als het downloaden van de logbestanden via de webinterface van Cloud Manager, waarbij het verschil is: `download-logs` de opdracht consolideert logbestanden over dagen op basis van het aantal dagen dat logbestanden worden aangevraagd.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Logboeken begrijpen

Logboeken in AEM as a Cloud Service hebben meerdere pods die loginstructies in deze pods schrijven. Omdat de veelvoudige AEM instanties aan het zelfde logboekdossier schrijven, is het belangrijk om te begrijpen hoe te analyseren, en lawaai te verminderen terwijl het zuiveren. Om uit te leggen, het volgende `aemerror` Logboekfragment wordt gebruikt:

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

de algemene richtsnoeren van de Adobe voor het logniveau per AEM as a Cloud Service omgeving zijn :

+ Lokale ontwikkeling (AEM SDK): `DEBUG`
+ Ontwikkeling: `DEBUG`
+ Werkgebied: `WARN`
+ Productie: `ERROR`

Het plaatsen van het meest aangewezen logboekniveau voor elk milieutype is as a Cloud Service AEM, de logboekniveaus worden gehandhaafd in code

+ Java-logconfiguraties worden onderhouden in OSGi-configuraties
+ Logniveaus van Apache-webserver en Dispatcher in het verzeneratieproject

...en dus een implementatie vereisen om te wijzigen.

### Omgevingsspecifieke variabelen om Java-logniveaus in te stellen

Een alternatief voor het instellen van statische bekende Java-logniveaus voor elke omgeving is AEM gebruiken als Cloud Service [omgevingsspecifieke variabelen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) om logboekniveaus te bepalen, toestaand de waarden dynamisch via [Adobe I/O CLI met plug-in Cloud Manager](#aio-cli).

Dit vereist het bijwerken van de registrerenconfiguraties OSGi om de milieu specifieke veranderlijke placeholders te gebruiken. [Standaardwaarden](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) voor logbestandniveaus moeten worden ingesteld als [Aanbevelingen van de Adobe](#log-levels). Bijvoorbeeld:

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

+ [Een beperkt aantal omgevingsvariabelen is toegestaan](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)en het maken van een variabele voor het beheren van het logniveau wordt gebruikt.
+ Omgevingsvariabelen kunnen via programmacode worden beheerd [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), [ADOBE I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid), en [HTTP-API&#39;s voor Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ Wijzigingen in omgevingsvariabelen moeten handmatig worden hersteld met een ondersteund gereedschap. Het vergeten om een hoog verkeersmilieu, zoals Productie, aan een minder uitgebreid logboekniveau terug te stellen kan de logboeken overstromen en AEM prestaties beïnvloeden.

_Omgevingsspecifieke variabelen werken niet voor Apache-webserver- of Dispatcher-logconfiguraties, omdat deze niet via de OSGi-configuratie worden geconfigureerd._
