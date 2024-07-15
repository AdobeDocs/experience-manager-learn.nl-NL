---
title: Samenstellen en implementeren
description: Adobe Cloud Manager vereenvoudigt het samenstellen van code en implementaties aan AEM as a Cloud Service. De mislukkingen kunnen tijdens stappen in het bouwstijlproces voorkomen, die actie vereisen om hen op te lossen. Deze gids loopt door het begrip gemeenschappelijke mislukkingen in de plaatsing, en hoe te om hen het best te benaderen.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
duration: 534
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2476'
ht-degree: 0%

---

# Fouten opsporen in AEM as a Cloud Service-build en -implementaties

Adobe Cloud Manager vereenvoudigt het samenstellen van code en implementaties aan AEM as a Cloud Service. De mislukkingen kunnen tijdens stappen in het bouwstijlproces voorkomen, die actie vereisen om hen op te lossen. Deze gids loopt door het begrip gemeenschappelijke mislukkingen in de plaatsing, en hoe te om hen het best te benaderen.

![ de Wolk beheert bouwstijlpijpleiding ](./assets/build-and-deployment/build-pipeline.png)

## Validatie

De validatiestap zorgt er eenvoudig voor dat Cloud Manager-basisconfiguraties geldig zijn. Veelvoorkomende validatiefouten zijn:

### De omgeving heeft een ongeldige status

+ __het bericht van de Fout:__ het milieu is in een ongeldige staat.
  ![ het milieu is in een ongeldige staat ](./assets/build-and-deployment/validation__invalid-state.png)
+ __Oorzaak:__ het doelmilieu van de pijpleiding is in een overgangsstaat waarbij het geen nieuwe bouwt kan goedkeuren.
+ __Resolutie:__ wacht op de staat om aan een lopende (of update beschikbare) staat op te lossen. Als de omgeving wordt verwijderd, maakt u de omgeving opnieuw of kiest u een andere omgeving om aan te maken.

### Het milieu verbonden aan de pijpleiding kan niet worden gevonden

+ __het bericht van de Fout:__ het milieu wordt duidelijk zoals geschrapt.
  ![ het milieu wordt duidelijk zoals geschrapt ](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Oorzaak:__ het milieu de pijpleiding wordt gevormd om te gebruiken is geschrapt.
Zelfs als een nieuw milieu van de zelfde naam opnieuw wordt gecreeerd, zal Cloud Manager niet automatisch de pijpleiding aan dat zelfde-genoemde milieu opnieuw associëren.
+ __Resolutie:__ geef de pijpleidingsconfiguratie uit, en herselecteer het milieu om op te stellen.

### De Git-vertakking die aan de pijplijn is gekoppeld, is niet gevonden

+ __het bericht van de Fout:__ Ongeldige pijpleiding: XXXXXX. Reason=Branch=xxxx niet gevonden in bewaarplaats.
  ![ Ongeldige pijpleiding: XXXXXX. Reason=Branch=xxxx niet gevonden in bewaarplaats ](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Oorzaak:__ de tak van het Git de pijpleiding wordt gevormd om te gebruiken is geschrapt.
+ __Resolutie:__ creeer de ontbrekende tak van het Git gebruikend de nauwkeurige zelfde naam, of vorm de pijpleiding opnieuw om van een verschillende, bestaande tak te bouwen.

## Testen van build en eenheid

![ Bouwstijl en het Testen van de Eenheid ](./assets/build-and-deployment/build-and-unit-testing.png)

De bouw en de Testende fase van de Eenheid voert een Gemaakt bouwstijl (`mvn clean package`) van het project uit die uit de gevormde tak van het Git van de pijpleiding wordt gecontroleerd.

De fouten die in deze fase zijn vastgesteld, moeten de lokale opbouw van het project kunnen heroveren, met de volgende uitzonderingen:

+ Een beproefde gebiedsdeel niet beschikbaar op [ Gemaakt Centraal ](https://search.maven.org/) wordt gebruikt, en de Geweven bewaarplaats die het gebiedsdeel bevat is of:
   + Niet bereikbaar vanuit Cloud Manager, zoals een interne privéopslagplaats in Maven, of de Maven-opslagplaats vereist verificatie en de onjuiste referenties zijn opgegeven.
   + Niet expliciet geregistreerd in de map `pom.xml` van het project. Merk op dat het opnemen van Maven repositories wordt ontmoedigd omdat het de bouwtijden verhoogt.
+ Eenheidstests mislukken vanwege tijdproblemen. Dit kan zich voordoen wanneer eenheidstests timinggevoelig zijn. Een sterke indicator vertrouwt op `.sleep(..)` in de testcode.
+ Het gebruik van niet-ondersteunde plug-ins.

## Codescannen

![ Scannen van de Code ](./assets/build-and-deployment/code-scanning.png)

Met codescannen wordt een statische codeanalyse uitgevoerd met behulp van een combinatie van Java en AEM-specifieke aanbevolen procedures.

Het aftasten van de code resulteert in een bouwstijlmislukking als de Kritieke kwetsbaarheid van de Veiligheid in de code bestaat. Minder overtredingen kunnen worden overschreven, maar het wordt aanbevolen dat deze worden gecorrigeerd. Merk op dat het codescannen onvolledig is en in [ valse positieven ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html#dealing-with-false-positives) kan resulteren.

Om code die kwesties aftasten op te lossen, download het CSV-Geformatteerde rapport door Cloud Manager via de **knoop van de Details van de Download** wordt verstrekt en herzie om het even welke ingangen die.

Voor meer details zie AEM specifieke regels, zie de documentaties van Cloud Manager [ douane AEM-specifieke de scanregels van de code ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html).

## Afbeeldingen samenstellen

![ bouwt Beelden ](./assets/build-and-deployment/build-images.png)

Build image is verantwoordelijk voor het combineren van de ingebouwde code-artefacten die in de stap Build &amp; Unit Testing zijn gemaakt met de AEM Release, om één implementeerbaar artefact te vormen.

Terwijl om het even welke code bouwt en compilatiekwesties tijdens het Testen van de Bouwstijl &amp; van de Eenheid worden gevonden, kunnen er configuratie of structurele kwesties worden geïdentificeerd wanneer het proberen om het douanebouwartefact met de AEM versie te combineren.

### OSGi-configuraties dupliceren

Wanneer de veelvoudige configuraties OSGi via runmode voor het doel AEM milieu oplossen, ontbreekt de stap van het Beeld van de Bouwstijl met de fout:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### Oorzaak 1

+ __Oorzaak:__ het alle pakket van het AEM project, bevat veelvoudige codepakketten, en de zelfde configuratie OSGi wordt verstrekt door meer dan één van de codepakketten, resulterend in een conflict, resulterend de stap van het Beeld van de Bouwstijl niet kon beslissen welke zou moeten worden gebruikt, waarbij de bouwstijl ontbreekt. Merk op dat dit niet op OSGi fabrieksconfiguraties van toepassing is, zolang zij unieke namen hebben.
+ __Resolutie:__ herzie alle codepakketten (met inbegrip van om het even welke inbegrepen pakketten van de derdencode) die als deel van de AEM toepassing worden opgesteld, zoekend dubbele configuraties OSGi die, via runmode, aan het doelmilieu oplossen. De instructies van het foutbericht om de markering mergeConfigurations in te stellen op true, zijn niet mogelijk in AEM als Cloud-service en moeten worden genegeerd.

#### Oorzaak 2

+ __Oorzaak:__ het AEM project omvat verkeerd het zelfde codepakket tweemaal, resulterend in de duplicatie van om het even welke configuratie OSGi in genoemd pakket.
+ __Resolutie:__ herzie alle pom.xml- pakketten ingebed in het al project, en zorg ervoor zij de `filevault-package-maven-plugin` [ configuratie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target) hebben die aan `<cloudManagerTarget>none</cloudManagerTarget>` wordt geplaatst.

### Onjuist geformuleerd script voor opnieuw aanwijzen

De opnieuw aanwijzen manuscripten bepalen basislijninhoud, gebruikers, ACLs, enz. In AEM as a Cloud Service worden opnieuw aanwijzen van scripts toegepast tijdens Build Image, maar op AEM lokale QuickStart van SDK worden deze toegepast wanneer de OSGi-fabrieksconfiguratie wordt geactiveerd. Hierdoor kunnen scripts voor opnieuw aanwijzen stilletjes mislukken (met registratie) bij AEM lokale QuickStart van SDK en de stap Build Image laten mislukken, waardoor de implementatie wordt gestopt.

+ __Oorzaak:__ A repoinit manuscript is misvormd. Dit kan ertoe leiden dat de repository onvolledig blijft omdat eventuele opnieuw geplaatste scripts na het falende script niet worden uitgevoerd tegen de repository.
+ __Resolutie:__ herzie lokale QuickStart van AEM SDK wanneer de opnieuw richt manuscriptOSGi configuratie wordt opgesteld om te bepalen als en wat de fouten zijn.

### Onbevredigd opnieuw richten inhoudsgebiedsdeel

De opnieuw aanwijzen manuscripten bepalen basislijninhoud, gebruikers, ACLs, enz. In AEM lokale QuickStart van SDK worden opnieuw aanwijsscripts toegepast wanneer de opnieuw aanwijzen van de OSGi-fabrieksconfiguratie wordt geactiveerd, met andere woorden, nadat de opslagplaats actief is en direct of via inhoudspakketten wijzigingen in de inhoud kan hebben ondergaan. In AEM as a Cloud Service worden tijdens Build Image opnieuw aanwijsscripts toegepast op een opslagplaats die geen inhoud bevat waarvan het script voor opnieuw aanwijzen afhankelijk is.

+ __Oorzaak:__ A repoinit manuscript hangt van inhoud af die niet bestaat.
+ __Resolutie:__ verzeker de inhoud het opnieuw richt manuscript van bestaat afhangt. Dit geeft vaak aan dat er onvoldoende gedefinieerde scripts zijn die verwijzen naar scripts die geen instructies bevatten die deze ontbrekende, maar vereiste inhoudsstructuren definiëren. Dit kan plaatselijk worden gereproduceerd door AEM te schrappen, Jar te decomprimeren en de opnieuw richt configuratie toe te voegen OSGi die het repoinit manuscript aan de installatiemap bevat, en AEM te beginnen. De fout wordt weergegeven in het bestand error.log van de lokale QuickStart-AEM SDK.


### De versie van de Componenten van de Kern van de toepassing is groter dan opgestelde versie

_Deze kwestie beïnvloedt slechts niet-productiemilieu&#39;s die niet auto-update aan de recentste AEM Versie._

AEM as a Cloud Service neemt automatisch de nieuwste versie van Core Components op in elke AEM Release. Dit betekent dat de nieuwste versie van Core Components in de AEM as a Cloud Service-omgeving automatisch of handmatig wordt bijgewerkt.

Is mogelijk voor de stap van het Beeld van de Bouwstijl zal ontbreken wanneer:

+ De implementerende toepassing werkt de versie van Core Components maven voor afhankelijkheden bij in het `core` (OSGi-bundel)-project
+ De implementerende toepassing wordt vervolgens geïmplementeerd in een AEM as a Cloud Service-omgeving met een sandbox (niet-productie) die niet is bijgewerkt voor het gebruik van een AEM die die nieuwe versie van Core Components bevat.

Om deze mislukking te verhinderen, wanneer een Update van het milieu van AEM as a Cloud Service beschikbaar is, omvat de update als deel van volgende bouwstijl/opstellen, en zorgt altijd ervoor dat de updates na het verhogen van de versie van de Componenten van de Kern in de basis van de toepassingscode inbegrepen zijn.

+ __Symptomen:__
De stap Build Image mislukt met een ERROR-melding dat `com.adobe.cq.wcm.core.components...` -pakketten met specifieke versiebereiken niet door het `core` -project kunnen worden geïmporteerd.

  ```
  [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
  [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD FAILURE
  [INFO] ------------------------------------------------------------------------
  ```

+ __Oorzaak:__ De bundel OSGi van de toepassing (die in het `core` project wordt bepaald) voert de klassen van Java van de kerngebiedsafhankelijkheid van Componenten van de Kern, op een verschillend versieniveau in dan is wat aan AEM as a Cloud Service wordt opgesteld.
+ __Resolutie:__
   + Gebruikend Git, keer terug aan het werk begaat die vóór de verhoging van de versie van de Component van de Kern bestaat. Druk op deze koppeling om een Cloud Manager Git-vertakking uit te voeren en voer een update van de omgeving uit vanuit deze vertakking. Hiermee wordt AEM as a Cloud Service geüpgraded naar de nieuwste versie AEM, die de latere versie van Core Components bevat. Als de AEM as a Cloud Service eenmaal is bijgewerkt naar de nieuwste versie AEM, die de nieuwste versie van Core Components bevat, kunt u de code die aanvankelijk niet werkte opnieuw implementeren.
   + Als u dit probleem lokaal wilt reproduceren, zorgt u ervoor dat de AEM SDK-versie dezelfde AEM releaseversie is als de AEM as a Cloud Service-omgeving gebruikt.


### Een Adobe-ondersteuningsgeval maken

Als de bovenstaande oplossingen het probleem niet oplossen, kunt u een Adobe Support-case maken via:

+ [ Adobe Admin Console ](https://adminconsole.adobe.com) > het Lusje van de Steun > leidt tot Geval

  _als u een lid van veelvoudige Adobe Orgs bent, zorg de Adobe Org die ontbrekende pijpleiding heeft geselecteerd in de Adobe Orgs schakelaar alvorens het geval tot stand te brengen._

## Distribueren naar

Deploy aan stap is verantwoordelijk voor het nemen van het codeartefact dat in het Beeld van de Bouwstijl wordt geproduceerd, begint nieuwe AEMAuteur en de diensten van Publish die het gebruiken, en op succes, verwijdert om het even welke oude AEMAuteur en de diensten van Publish. Ook in deze stap worden mobiele inhoudspakketten en indexen geïnstalleerd en bijgewerkt.

Vertrouwd me met [ de logboeken van AEM as a Cloud Service ](./logs.md) voorafgaand aan het zuiveren van opstellen aan stap. Het logbestand van `aemerror` bevat informatie over het opstarten en afsluiten van pods die relevant kan zijn voor het implementeren van problemen. Het logbestand dat beschikbaar is via de knop Logbestand downloaden in de Cloud Manager-toepassing voor implementeren om de stap uit te voeren, is niet het `aemerror` -logbestand en bevat geen gedetailleerde informatie over het opstarten van uw toepassingen.

![ opstellen aan ](./assets/build-and-deployment/deploy-to.png)

De drie primaire redenen waarom Deploy aan stap kan ontbreken:

### De Cloud Manager-pijplijn bevat een oude AEM.

+ __Oorzaak:__ Een pijpleiding van Cloud Manager houdt een oudere versie van AEM dan wat aan het doelmilieu wordt opgesteld. Dit kan gebeuren wanneer een pijpleiding wordt hergebruikt en op een nieuw milieu gericht dat een recentere versie van AEM in werking stelt. Dit kan door controle worden geïdentificeerd om te zien of is de AEM versie van het milieu groter dan AEM versie van de pijpleiding.
  ![ de pijpleiding van Cloud Manager houdt een oude AEM versie ](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Resolutie:__
   + Als het doelmilieu een Beschikbare Update heeft, uitgezochte Update van de acties van het milieu, en dan rerun de bouwstijl.
   + Als de doelomgeving geen Update Beschikbaar heeft, betekent dit dat de meest recente versie van AEM wordt uitgevoerd. Om dit op te lossen, schrap de pijpleiding en creeer het opnieuw.


### Cloud Manager keer uit

De code die tijdens de aanvang van de nieuw opgestelde AEM dienst loopt neemt zo lang dat de tijden van Cloud Manager uit alvorens de implementatie kan voltooien. In deze gevallen kan de implementatie uiteindelijk slagen, zelfs als de Cloud Manager-status als mislukt wordt gerapporteerd.

+ __Oorzaak:__ de code van de Douane kan verrichtingen, zoals grote vragen of inhoudstraversals uitvoeren, vroeg in bundel OSGi of de levenscycli van de Component worden teweeggebracht beduidend vertragend de opstartietijd van AEM.
+ __Resolutie:__ herzie de implementatie voor code die vroeg in de levenscyclus van de Bundel OSGi loopt, en herzie de `aemerror` logboeken voor de diensten van AEM Auteur en van Publish rond de tijd van de mislukking (logboektijd in GMT) zoals aangetoond door Cloud Manager, en zoek logboekberichten die op om het even welke processen wijzen van het douanelogboek in werking stellen.

### Niet-compatibele code of configuratie

De meeste code en configuratieschendingen worden gevangen in vroeger in de bouwstijl, nochtans is het mogelijk voor douanecode of configuratie om met AEM as a Cloud Service onverenigbaar te zijn en niet ontdekt te gaan tot het in de container uitvoert.

+ __Oorzaak:__ de code van de Douane kan lange verrichtingen, zoals grote vragen of inhoudstraversals aanhalen, vroeg op in OSGi bundel of de levenscycli van de Component worden teweeggebracht beduidend vertragend de opstartietijd van AEM.
+ __Resolutie:__ herzie de `aemerror` logboeken voor AEM de diensten van de Auteur en van Publish rond de tijd (logboektijd in GMT) van de mislukking zoals aangetoond door Cloud Manager.
   1. Bekijk de logboeken voor om het even welke FOUTEN die door de klassen van Java worden geworpen die door de douanetoepassing worden verstrekt. Als om het even welke kwesties worden gevonden, los op, duw de vaste code, en herbouw de pijpleiding.
   1. Bekijk de logboeken voor om het even welke FOUTEN die door aspecten van AEM worden gemeld dat u zich uitbreidt/met in uw douanetoepassing in wisselwerking staat, en onderzoek die; deze FOUTEN kunnen niet direct aan klassen van Java worden toegeschreven. Als om het even welke kwesties worden gevonden, los op, duw de vaste code, en herbouw de pijpleiding.

### /var opnemen in inhoudspakket

`/var` is mutueel en bevat diverse overgangsinhoud. `/var` opnemen in inhoudspakketten (bijvoorbeeld `ui.content`) geïmplementeerd via Cloud Manager kan ertoe leiden dat de stap Implementeren mislukt.

Dit probleem is moeilijk te identificeren omdat het niet resulteert in een mislukking op de aanvankelijke plaatsing, slechts op verdere plaatsingen. Tot de symptomen behoren:

+ De eerste implementatie slaagt, maar nieuwe of gewijzigde veranderbare inhoud, die onderdeel is van de implementatie, lijkt niet te bestaan op AEM Publish-service.
+ Activering/deactivering van inhoud in AEM auteur wordt geblokkeerd
+ De verdere plaatsingen ontbreken in Deploy aan stap, met Deploy om na ongeveer 60 minuten te ontbreken te stappen.

Om deze kwestie te bevestigen is de oorzaak van het falende gedrag:

1. Wanneer wordt bepaald dat ten minste één inhoudspakket dat deel uitmaakt van de implementatie, wordt geschreven naar `/var` .
1. Verifieer de primaire (bolde) distributierij wordt geblokkeerd bij:
   + AEM Auteur > Extra > Implementatie > Distributie
     ![ Geblokkeerde distributierij ](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. Als de volgende implementatie mislukt, downloadt u logboeken van Cloud Manager &quot;Deploto&quot; met de knop Logbestand downloaden:

   ![ opstellen van de Download aan logboeken ](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... en controleer of er ongeveer 60 minuten zijn tussen de loginstructies:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   en ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Merk op dat dit logboek deze indicatoren op de aanvankelijke plaatsingen niet zal bevatten die als succesvol, eerder slechts op verdere mislukkende plaatsingen meldt.

+ __Oorzaak:__ AEM de gebruiker van de replicatieservice die wordt gebruikt om inhoudspakketten aan de AEM dienst van Publish op te stellen kan niet aan `/var` op AEM Publish schrijven. Dit resulteert in de plaatsing van het inhoudspakket aan de dienst van AEM Publish om te ontbreken.
+ __Resolutie:__ De volgende manieren om deze kwesties op te lossen zijn vermeld in de orde van voorkeur:
   1. Als de `/var` -bronnen niet nodig zijn, verwijdert u onder `/var` bronnen uit inhoudspakketten die als onderdeel van uw toepassing worden geïmplementeerd.
   2. Als de `/var` middelen noodzakelijk zijn, bepaal de knoopstructuren gebruikend [ repoinit ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit). Scripts voor opnieuw aanwijzen kunnen worden gericht op AEM auteur, AEM Publish of beide, via OSGi-runmodes.
   3. Als de `/var` middelen slechts op AEM auteur worden vereist en redelijkerwijs niet kunnen worden gemodelleerd gebruikend [ repoinit ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit), hen naar een discrete inhoudspakket verplaatsen, dat slechts op AEM Auteur door [ inbeddend ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds) het in het `all` pakket in een AEM omslag van de Auteur (`<target>/apps/example-packages/content/install.author</target>`) geïnstalleerd is.
   4. Verstrek aangewezen ACLs aan de `sling-distribution-importer` dienstgebruiker zoals die in dit [ wordt beschreven Adobe KB ](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Een Adobe-ondersteuningsgeval maken

Als de bovenstaande oplossingen het probleem niet oplossen, kunt u een Adobe Support-case maken via:

+ [ Adobe Admin Console ](https://adminconsole.adobe.com) > het Lusje van de Steun > leidt tot Geval

  _als u een lid van veelvoudige Adobe Orgs bent, zorg de Adobe Org die ontbrekende pijpleiding heeft geselecteerd in de Adobe Orgs schakelaar alvorens het geval tot stand te brengen._
