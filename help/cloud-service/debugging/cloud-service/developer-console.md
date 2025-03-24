---
title: Developer Console
description: AEM as a Cloud Service biedt een Developer Console voor elke omgeving die verschillende details van de actieve AEM-service beschikbaar maakt die nuttig zijn voor foutopsporing.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 295
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 0%

---

# Fouten in AEM as a Cloud Service opsporen met de Developer Console

AEM as a Cloud Service biedt een Developer Console voor elke omgeving die verschillende details van de actieve AEM-service beschikbaar maakt die nuttig zijn voor foutopsporing.

Elke AEM as a Cloud Service-omgeving heeft een eigen Developer Console.

## Navigeren naar Developer Console

Developer Console is per AEM as a Cloud Service-omgeving toegankelijk via Cloud Manager.

![ ga aan Developer Console ](./assets/developer-console/navigate.png)

1. Ga aan __[Cloud Manager ](https://my.cloudmanager.adobe.com/)__
2. Open het __Programma__ dat het milieu van AEM as a Cloud Service bevat om Developer Console te openen.
3. Bepaal de plaats van het __Milieu__, en selecteer `...`.
4. Selecteer __Developer Console__ van de dropdown lijst.


## Developer Console-toegang

Om tot Developer Console toegang te hebben en te gebruiken moeten de volgende toestemmingen aan Adobe ID van de ontwikkelaar via [ Adobe Admin Console ](https://adminconsole.adobe.com) worden gegeven.

1. Zorg ervoor dat de Adobe Org in de Adobe Org-switch staat en dat de Org gerelateerd is aan de omgevingen die u wilt inspecteren in de Developer Console.
1. Als u zich wilt aanmelden bij de Developer Console, moet de ontwikkelaar lid zijn van een van de volgende rollen:
   + __Ontwikkelaar 1} van het Product van 0} Cloud Manager - het Profiel van het Product van Cloud Service__ ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer): In dit geval, zal de ontwikkelaar de volledige lijst van milieu&#39;s zien beschikbaar onder geselecteerde Developer Console URL; als een milieu of RDE van de Ontwikkeling in Cloud Manager, andere milieu of RDEs in dat zelfde Programma zou kunnen worden geselecteerd.[
   + __Profiel van het Product van de Beheerders van AEM 1} op__ de Auteur van AEM __](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles): In dit geval, zal de lijst van milieu&#39;s die in het vorige opsommingsteken worden beschreven tot de verwante productprofielen worden beperkt waar deze rol wordt toegewezen.[__
1. De ontwikkelaar moet een lid van de [__Gebruikers van AEM__ zijn of __AEM Beheerders__ Profiel van het Product op de Auteur van AEM en/of publiceren ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + Als dit lidmaatschap niet bestaat, zullen de [ status ](#status) dumps onderbreking met een 401 Onbevoegde fout.

### Problemen met Developer Console-toegang oplossen

#### Als ik inlog, zie ik de omgeving waarnaar ik zoek niet in de lijst

Zorg voor het volgende:

+ U hebt de juiste Developer Console URL geselecteerd door op de drie stippen voor de geselecteerde omgeving via Cloud Manager te klikken en Developer Console te selecteren.
+ U of hebt __Ontwikkelaar 1} van het Product van 0} Cloud Manager - het Profiel van het Product van Cloud Service__ ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer) om de volledige lijst van milieu&#39;s te zien of u maakt deel uit van het [__Profiel van het Product van de Beheerders van AEM__ op __de Auteur van AEM__ ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles) voor het milieu u niet vindt.[

#### 401 Ongeoorloofde fout bij de status van dumping

![ Developer Console - 401 onbevoegd ](./assets/developer-console/troubleshooting__401-unauthorized.png)

Als er een status met 401 onbevoegde fout wordt genegeerd, betekent dit dat de gebruiker nog niet beschikt over de benodigde rechten in AEM as a Cloud Service of dat de gebruikte aanmeldingstokens ongeldig zijn of verlopen zijn.

Het 401-probleem met onbevoegden oplossen:

1. Zorg ervoor dat uw gebruiker lid is van het juiste Adobe IMS-productprofiel (AEM-beheerders of AEM-gebruikers) voor de aan Developer Console gekoppelde AEM as a Cloud Service-productinstantie.
   + Houd er rekening mee dat Developer Console toegang heeft tot 2 Adobe IMS-productinstanties; de AEM as a Cloud Service Author and Publish-productinstanties, zodat de juiste productprofielen worden gebruikt, afhankelijk van de servicelaag die toegang via Developer Console vereist.
1. Meld u aan bij de AEM as a Cloud Service (Auteur of Publiceren) en controleer of de gebruikers en groepen correct zijn gesynchroniseerd met AEM.
   + Developer Console vereist dat uw gebruikersrecord wordt gemaakt in de corresponderende AEM-servicelaag, zodat deze kan worden geverifieerd op die servicelaag.
1. Wis uw browsers cookies en de status van de toepassing (lokale opslag) en meld u opnieuw aan bij Developer Console, zodat het toegangstoken dat Developer Console gebruikt correct en niet verlopen is.

## Pod

De auteur en de Publish diensten van AEM as a Cloud Service bestaan uit veelvoudige instanties respectievelijk om verkeersvariabiliteit en het rollen updates zonder onderbreking te behandelen. Deze gevallen worden pods genoemd. De selectie van de peul in Developer Console bepaalt het werkingsgebied van gegevens die via de andere controles zullen blootstellen.

![ Developer Console - Peul ](./assets/developer-console/pod.png)

+ Een pod is een afzonderlijk exemplaar dat deel uitmaakt van een AEM-service (Auteur of Publiceren)
+ Pods zijn van voorbijgaande aard, wat betekent dat AEM as a Cloud Service ze naar behoefte maakt en vernietigt
+ Alleen pods die deel uitmaken van de bijbehorende AEM as a Cloud Service-omgeving, worden weergegeven in de Developer Console Pod-switch van die omgeving.
+ U kunt onder aan de pod-schakeloptie de optie Pods selecteren op basis van het servicetype:
   + Alle auteurs
   + Alle uitgevers
   + Alle instanties

## Status

Status biedt opties voor het uitvoeren van specifieke AEM-runtimestatus in tekst of JSON-uitvoer. De Developer Console verstrekt gelijkaardige informatie zoals de lokale QuickStart-console van OSGi Web van AEM SDK, met het duidelijke verschil dat Developer Console read-only is.

![ Developer Console - Status ](./assets/developer-console/status.png)

### Bundels

Bij bundels worden alle OSGi-bundels in AEM weergegeven. Deze functionaliteit is gelijkaardig aan [ AEM SDK die lokale Quickstart Bundles OSGi ](http://localhost:4502/system/console/bundles) bij `/system/console/bundles` .

Bundels helpen bij het opsporen van fouten door:

+ Alle OSGi-bundels die als service aan AEM worden geïmplementeerd, weergeven
+ Een overzicht geven van de status van elke OSGi-bundel, ook als deze actief is of niet.
+ Het verstrekken van details in onopgeloste gebiedsdelen die OSGi- bundels veroorzaken om actief te worden

### Onderdelen

De componenten maken een lijst van alle componenten OSGi in AEM. Deze functionaliteit is gelijkaardig aan [ AEM SDK die lokale QuickStart componenten OSGi ](http://localhost:4502/system/console/components) bij `/system/console/components` .

Componenten helpen bij het opsporen van fouten door:

+ Alle OSGi-componenten vermeld die naar AEM as a Cloud Service zijn geïmplementeerd
+ Het verstrekken van de staat van elke component OSGi; met inbegrip van als zij actief of ontevreden zijn
+ Het verstrekken van details in ontevreden de dienstverwijzingen kan componenten OSGi veroorzaken om actief te worden
+ OSGi-eigenschappen vermelden en hun waarden koppelen aan de OSGi-component.
   + Dit zal daadwerkelijke die waarden tonen via [ worden ingespoten OSGi de variabelen van de milieuconfiguratie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### Configuraties

De configuraties maken een lijst van alle configuraties van de component OSGi (eigenschappen OSGi en waarden). Deze functionaliteit is gelijkaardig aan [ AEM SDK die lokale Quickstart Manager van de Configuratie OSGi ](http://localhost:4502/system/console/configMgr) bij `/system/console/configMgr` .

Configuraties helpen bij het opsporen van fouten door:

+ OSGi-eigenschappen en hun waarden per OSGi-component vermelden
   + Dit zal geen daadwerkelijke die waarden tonen via [ worden ingespoten OSGi de variabelen van de milieuconfiguratie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). Zie [ Componenten ](#components) hierboven, voor de geïnjecteerde waarden.
+ Onjuist geconfigureerde eigenschappen zoeken en identificeren

### Oak-indexen

Oak Indexes biedt een stortplaats van de hieronder gedefinieerde knooppunten `/oak:index` . Houd er rekening mee dat hier geen samengevoegde indexen worden weergegeven, die optreden wanneer een AEM-index wordt gewijzigd.

Oak Indexes helpt bij het opsporen van fouten door:

+ Alle Oak Index-definities weergeven die inzicht bieden in de manier waarop zoekopdrachten in AEM worden uitgevoerd. Houd er rekening mee dat de indexen die zijn gewijzigd in AEM, hier niet worden weergegeven. Deze weergave is alleen handig voor indexen die alleen door AEM worden geleverd of die alleen door de aangepaste code worden geleverd.

### OSGi Services

De componenten maken een lijst van alle diensten OSGi. Deze functionaliteit is gelijkaardig aan [ AEM SDK die lokale QuickStart de Diensten OSGi ](http://localhost:4502/system/console/services) bij `/system/console/services` .

De hulp van OSGi Services in het zuiveren door:

+ Alle OSGi-services in AEM vermelden, samen met de OSGi-bundel die deze levert, en alle OSGi-bundels die deze gebruiken

### Verkooptaken

De het verkopen Banen maakt een lijst van alle het Verschuiven Banen rijen. Deze functionaliteit is gelijkaardig aan [ AEM SDK die lokale taken van QuickStart ](http://localhost:4502/system/console/slingevent) bij `/system/console/slingevent` .

Het verkopen van Banen helpt in het zuiveren door:

+ Lijst met taakwachtrijen voor verkopen en hun configuraties
+ Het verstrekken van inzichten in het aantal actieve, een rij gevormde en verwerkte het schrapen banen, die voor het zuiveren van kwesties met Werkschema, Voorbijgaande Werkschema en ander werk nuttig is door het Verdelen van Banen in AEM wordt uitgevoerd.

## Java-pakketten

Met Java Packages kan worden gecontroleerd of een Java-pakket en -versie beschikbaar zijn voor gebruik in AEM as a Cloud Service. Deze functionaliteit is het zelfde als [ AEM SDK die lokale quickstart de Vinder van de Afhankelijkheid van het Begin ](http://localhost:4502/system/console/depfinder) bij `/system/console/depfinder` .

![ Developer Console - de Pakketten van Java ](./assets/developer-console/java-packages.png)

Java Packages wordt gebruikt om problemen op te lossen met bundels die niet worden gestart vanwege onopgeloste import of onopgeloste klassen in scripts (HTML, JSP, enz.). Als Java Packages rapporteert dat er geen bundels zijn, wordt een Java-pakket geëxporteerd (of de versie komt niet overeen met de versie die is geïmporteerd door een OSGi-bundel):

+ Zorg ervoor dat de versie van de AEM API-afhankelijke versie van uw project overeenkomt met de AEM Release-versie van de omgeving (en werk indien mogelijk alles bij naar de laatste versie).
+ Als de extra Geweven gebiedsdelen in het Maven project worden gebruikt
   + Bepaal of in plaats daarvan een alternatieve API kan worden gebruikt die door de AEM SDK API-afhankelijkheid wordt geleverd.
   + Als de extra afhankelijkheid wordt vereist, zorg ervoor het als bundel OSGi (eerder dan gewone Jar) verstrekt en het in het codepakket van uw project, (`ui.apps`), gelijkend op hoe de kernOSGi- Bundel in het `ui.apps` pakket wordt ingebed.

## Servlets

De servers worden gebruikt om inzicht te verstrekken over hoe AEM een URL aan een servlet of manuscript van Java (HTML, JSP) oplost die uiteindelijk het verzoek behandelt. Deze functionaliteit is het zelfde als [ AEM SDK lokale quickstart Sling Servlet Resolver ](http://localhost:4502/system/console/servletresolver) bij `/system/console/servletresolver`.

![ Developer Console - Servlets ](./assets/developer-console/servlets.png)

Servlets helpt bij het bepalen van foutopsporing:

+ Hoe een URL in zijn adresseerbare delen (middel, selecteur, uitbreiding) wordt gedecomposeerd.
+ Welk servlet of script een URL oplost, helpt bij het identificeren van onjuist gevormde URL&#39;s of onjuist geregistreerde servlets/scripts.

## Zoekopdrachten

Zoekopdrachten helpen u inzicht te verschaffen in wat en hoe zoekopdrachten worden uitgevoerd op AEM. Deze functionaliteit is het zelfde als [ AEM SDK die lokale quickstart Hulpmiddelen > de console van de Prestaties van de Vraag ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) gebruikt.

Zoekopdrachten werken alleen wanneer een specifieke pod is geselecteerd, omdat deze de webconsole van Query Performance van die pod opent, waarbij de ontwikkelaar toegang moet hebben om zich aan te melden bij de AEM-service.

![ Developer Console - Vragen - verklaar Vraag ](./assets/developer-console/queries__explain-query.png)

Zoekopdrachten helpen bij het opsporen van fouten door:

+ Uitleggen hoe query&#39;s worden geïnterpreteerd, geanalyseerd en uitgevoerd door Oak. Dit is zeer belangrijk wanneer het volgen waarom een vraag langzaam is, en het begrijpen hoe het kan worden versneld.
+ De populairste query&#39;s die in AEM worden uitgevoerd, weergeven met de mogelijkheid ze te verklaren.
+ De langzaamste query&#39;s die in AEM worden uitgevoerd, weergeven met de mogelijkheid om ze te verklaren.
