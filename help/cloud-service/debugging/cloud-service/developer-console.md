---
title: Developer Console
description: AEM als Cloud Service verstrekt een Console van de Ontwikkelaar voor elk milieu dat diverse details van de lopende AEM dienst blootstelt die in het zuiveren nuttig zijn.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
translation-type: tm+mt
source-git-commit: 1af3661e5c18206d58d339d51d5189834e843023
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---


# Fouten opsporen in AEM als Cloud Service met de ontwikkelaarsconsole

AEM als Cloud Service verstrekt een Console van de Ontwikkelaar voor elk milieu dat diverse details van de lopende AEM dienst blootstelt die in het zuiveren nuttig zijn.

Elke AEM als Cloud Service heeft zijn eigen Developer Console.

## Toegang tot ontwikkelaarsconsole

Om tot de Console van de Ontwikkelaar toegang te hebben en te gebruiken moeten de volgende toestemmingen aan de ontwikkelaar Adobe ID via [Adobe Admin Console](https://adminconsole.adobe.com)worden gegeven.

1. Zorg ervoor dat de Adobe Org die Cloud Manger en AEM als producten van de Cloud Service heeft aangetast, actief is in de Adobe Org-switch.
1. De ontwikkelaar moet lid zijn van het productprofiel voor __ontwikkelaars en Cloud Servicen__ van Cloud Manager.
   + Als dit lidmaatschap niet bestaat, kan de ontwikkelaar zich niet aanmelden bij de Developer Console.
1. De ontwikkelaar moet lid zijn van het productprofiel van AEM-auteur en -publicatieservice __AEM Beheerders__ .
   + Als dit lidmaatschap niet bestaat, zal de [status](#status) dumps onderbreking met een 401 Onbevoegde fout.

### Toegang tot ontwikkelaarsconsole voor probleemoplossing

#### 401 Ongeoorloofde fout bij de status van dumping

![Ontwikkelaarsconsole - 401 niet-geautoriseerd](./assets/developer-console/troubleshooting__401-unauthorized.png)

Als een status met 401 onbevoegde fout wordt verwijderd, betekent dit dat uw gebruiker nog niet beschikt over de benodigde machtigingen in AEM als Cloud Service of dat het gebruik van de aanmeldingstokens ongeldig is of is verlopen.

Het 401-probleem met onbevoegden oplossen:

1. Zorg ervoor dat uw gebruiker lid is van het juiste Adobe IMS-productprofiel (AEM beheerders of AEM gebruikers) voor de aan de Developer Console gekoppelde AEM als een Cloud Service Product-instantie.
   + Houd er rekening mee dat Developer Console toegang heeft tot 2 Adobe IMS-productinstanties; de AEM als auteur van de Cloud Service en publiceer productinstanties, zodat de correcte Profiles van het Product worden gebruikt afhankelijk van welke de dienstrij toegang via de Console van de Ontwikkelaar vereist.
1. Meld u aan bij de AEM als Cloud Service (Auteur of Publiceren) en controleer of de gebruiker en de groepen correct zijn gesynchroniseerd in AEM.
   + De Console van de ontwikkelaar vereist dat uw gebruikersverslag in de overeenkomstige AEM de dienstrij voor het voor authentiek verklaart aan die de dienstrij wordt gecreeerd.
1. Wis uw browservercookies en de status van de toepassing (lokale opslag) en meld u opnieuw aan in de Developer Console. Op deze manier zorgt u ervoor dat het toegangstoken dat de Developer Console gebruikt, correct en niet verlopen is.

## Pod

AEM als Auteur van de Cloud Service en de Publish diensten bestaan uit veelvoudige instanties respectievelijk om verkeersvariabiliteit en het rollen updates zonder onderbreking te behandelen. Deze gevallen worden pods genoemd. De selectie van de peul in de Console van de Ontwikkelaar bepaalt het werkingsgebied van gegevens die via de andere controles zullen blootstellen.

![Ontwerpconsole - Pod](./assets/developer-console/pod.png)

+ Een pod is een afzonderlijk exemplaar dat deel uitmaakt van een AEM (Auteur of Publiceren)
+ Pods zijn van voorbijgaande aard, wat betekent dat AEM als Cloud Service ze naar behoefte maakt en vernietigt
+ Alleen pods die deel uitmaken van de bijbehorende AEM als een Cloud Service-omgeving, worden weergegeven in de podswitch van de ontwikkelaarsconsole van die omgeving.
+ U kunt onder aan de podschakelaar onder aan de pod de opties voor het gebruiksgemak kiezen om pods te selecteren op basis van het servicetype:
   + Alle auteurs
   + Alle uitgevers
   + Alle instanties

## Status

Status biedt opties voor het uitvoeren van specifieke AEM runtime status in tekst of JSON-uitvoer. De console van de Ontwikkelaar verstrekt gelijkaardige informatie zoals de lokale console van het Web van OSGi van SDK van de AEM lokale QuickStart, met het duidelijke verschil dat de Console van de Ontwikkelaar read-only is.

![Developer Console - Status](./assets/developer-console/status.png)

### Bundels

Bij bundels worden alle OSGi-bundels in AEM weergegeven. Deze functionaliteit is vergelijkbaar met de lokale OSGi-bundels [van](http://localhost:4502/system/console/bundles) AEM SDK bij `/system/console/bundles`.

Bundels helpen bij het opsporen van fouten door:

+ Een lijst maken van alle OSGi-bundels die worden geïmplementeerd als een service AEM
+ vermelding van de staat van elke OSGi-bundel; inclusief of ze al dan niet actief zijn
+ Het verstrekken van details in onopgeloste gebiedsdelen die OSGi- bundels veroorzaken om actief te worden

### Onderdelen

De componenten maken een lijst van alle componenten OSGi in AEM. Deze functionaliteit is vergelijkbaar met de lokale OSGi Components [van](http://localhost:4502/system/console/components) AEM SDK bij `/system/console/components`.

Componenten helpen bij het opsporen van fouten door:

+ Een lijst maken van alle componenten OSGi die als Cloud Service worden opgesteld aan AEM
+ Verstrekken van de staat van elke component OSGi; inclusief of ze actief of ontevreden zijn
+ Het verstrekken van details in ontevreden de dienstverwijzingen kan componenten OSGi veroorzaken om actief te worden
+ OSGi-eigenschappen weergeven en hun waarden koppelen aan de OSGi-component

### Configuraties

De configuraties maken een lijst van alle configuraties van de component OSGi (eigenschappen OSGi en waarden). Deze functionaliteit is gelijkaardig aan de lokale Manager [van de Configuratie van](http://localhost:4502/system/console/configMgr) AEM SDK van de QuickStart OSGi bij `/system/console/configMgr`.

Configuraties helpen bij het opsporen van fouten door:

+ OSGi-eigenschappen en hun waarden per OSGi-component vermelden
+ Onjuist geconfigureerde eigenschappen zoeken en identificeren

### eiken indexen

De indexen van de eiken verstrekken een stortplaats van de hieronder bepaalde knopen `/oak:index`. Onthoud dat hier geen samengevoegde indexen worden weergegeven, die optreden wanneer een AEM index wordt gewijzigd.

De Indexen van het eiken helpen in het zuiveren door:

+ Alle definities in de Oak Index die inzicht verschaffen in de manier waarop zoekopdrachten in AEM worden uitgevoerd. Houd in mening, dat gewijzigd aan AEM indexen niet hier wordt weerspiegeld. Deze weergave is alleen handig voor indexen die alleen worden geleverd door AEM of alleen worden geleverd door de aangepaste code.

### OSGi Services

De componenten maken een lijst van alle diensten OSGi. Deze functionaliteit is gelijkaardig aan [AEM lokale Quickstart van de Diensten](http://localhost:4502/system/console/services) OSGi van SDK bij `/system/console/services`.

De hulp van OSGi Services in het zuiveren door:

+ Een lijst maken van alle OSGi diensten in AEM, samen met zijn leverende bundel OSGi, en alle bundels OSGi die het verbruiken

### Verkooptaken

De het verkopen Banen maakt een lijst van alle het Verschuiven Banen rijen. Deze functionaliteit is vergelijkbaar met de lokale QuickStart-taken [van](http://localhost:4502/system/console/slingevent) AEM SDK op `/system/console/slingevent`.

Het verkopen van Banen helpt in het zuiveren door:

+ Lijst met taakwachtrijen voor verkopen en hun configuraties
+ Het verstrekken van inzichten in het aantal actieve, een rij gevormde en verwerkte banen van het Verkopen, die voor het zuiveren van kwesties met Werkschema, Voorbijgaande Werkschema en ander werk nuttig is dat door het Verdelen van Banen in AEM wordt uitgevoerd.

## Java-pakketten

Met Java Packages kan worden gecontroleerd of een Java-pakket en -versie beschikbaar zijn voor gebruik in AEM als Cloud Service. Deze functionaliteit is hetzelfde als de lokale Quickstart-zoeker [van](http://localhost:4502/system/console/depfinder) AEM SDK bij `/system/console/depfinder`.

![Developer Console - Java Packages](./assets/developer-console/java-packages.png)

Java Packages wordt gebruikt om problemen op te lossen met bundels die niet worden gestart vanwege onopgeloste import of onopgeloste klassen in scripts (HTML, JSP, enz.). Als Java Packages rapporteert dat er geen bundels zijn, wordt een Java-pakket geëxporteerd (of de versie komt niet overeen met de versie die is geïmporteerd door een OSGi-bundel):

+ Zorg ervoor dat de AEM API van uw project in overeenstemming is met de versie van de AEM van de omgeving (en werk indien mogelijk alles bij naar de meest recente versie).
+ Als de extra Geweven gebiedsdelen in het Maven project worden gebruikt
   + Bepaal of in plaats daarvan een alternatieve API kan worden gebruikt die wordt geleverd door de AEM-API-afhankelijkheid.
   + Als de extra afhankelijkheid wordt vereist, zorg ervoor het als bundel OSGi (eerder dan a gewone Jar) verstrekt en het in het codepakket van uw project, (`ui.apps`), gelijkend op hoe de kernOSGi- Bundel in het `ui.apps` pakket wordt ingebed.

## Servlets

De servers worden gebruikt om inzicht te verstrekken over hoe AEM een URL aan een servlet of manuscript van Java (HTML, JSP) oplost die uiteindelijk het verzoek behandelt. Deze functionaliteit is hetzelfde als de lokale QuickStart-oplosser [van](http://localhost:4502/system/console/servletresolver) SDK voor Sling Servlet bij `/system/console/servletresolver`.

![Ontwerpconsole - Servlets](./assets/developer-console/servlets.png)

Servlets helpt bij het bepalen van foutopsporing:

+ Hoe een URL in zijn adresseerbare delen (middel, selecteur, uitbreiding) wordt gedecomposeerd.
+ Welk servlet of script een URL oplost, helpt bij het identificeren van onjuist gevormde URL&#39;s of onjuist geregistreerde servlets/scripts.

## Zoekopdrachten

Zoekopdrachten helpen u inzicht te verschaffen in wat en hoe zoekopdrachten worden uitgevoerd op AEM. Deze functionaliteit is hetzelfde als de lokale QuickStart-tools > Query Performance- [console van ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) AEM SDK.

Zoekopdrachten werken alleen wanneer een specifieke pod is geselecteerd, omdat deze de webconsole van Query Performance van die pod opent, waarbij de ontwikkelaar toegang moet hebben om zich aan te melden bij de AEM.

![Ontwikkelaarsconsole - query&#39;s - Query uitvoeren](./assets/developer-console/queries__explain-query.png)

Zoekopdrachten helpen bij het opsporen van fouten door:

+ Verklarend hoe de vragen door Oak worden geïnterpreteerd, geanalyseerd en uitgevoerd. Dit is zeer belangrijk wanneer het volgen waarom een vraag langzaam is, en het begrijpen hoe het kan worden versneld.
+ De populairste query&#39;s die in AEM worden uitgevoerd, weergeven met de mogelijkheid ze te verklaren.
+ Het opzoeken van de langzaamste vragen die in AEM lopen, met de capaciteit om hen te verklaren.
