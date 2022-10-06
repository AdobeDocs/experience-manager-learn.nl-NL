---
title: Werken met multimedia en gelijktijdige ontwikkeling
description: Leer over de voordelen, de uitdagingen, en de technieken om een multi-huurdersimplementatie met de Middelen van Adobe Experience Manager te beheren.
feature: Connected Assets
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: c9ee29d4-a8a5-4e61-bc99-498674887da5
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2017'
ht-degree: 0%

---

# Werken met multimedia en gelijktijdige ontwikkeling {#understanding-multitenancy-and-concurrent-development}

## Inleiding {#introduction}

Wanneer de veelvoudige teams hun code aan de zelfde AEM milieu&#39;s opstellen, zijn er praktijken zij zouden moeten volgen om ervoor te zorgen dat de teams zo onafhankelijk mogelijk kunnen werken, zonder op de tenen van andere teams te stappen. Hoewel zij nooit volledig kunnen worden geëlimineerd, zullen deze technieken dwars-teamgebiedsdelen minimaliseren. Een gelijktijdig ontwikkelingsmodel kan alleen succesvol zijn als de ontwikkelingsteams goed met elkaar communiceren.

Bovendien, wanneer de veelvoudige ontwikkelingsteams aan het zelfde AEM milieu werken, is er waarschijnlijk één of andere graad van multi-tenancy bij spel. Er is veel geschreven over de praktische overwegingen om te proberen meerdere huurders in een AEM omgeving te ondersteunen, met name wat betreft de uitdagingen die zich voordoen bij het beheer van bestuur, activiteiten en ontwikkeling. In dit document worden enkele technische uitdagingen besproken rond het implementeren van AEM in een omgeving met meerdere huurders, maar veel van deze aanbevelingen zijn van toepassing op elke organisatie met meerdere ontwikkelingsteams.

Het is belangrijk om op voorhand op te merken dat hoewel AEM veelvoudige plaatsen en zelfs veelvoudige merken kan steunen die op één enkele milieu worden opgesteld, het geen ware multi-huur aanbiedt. Sommige omgevingsconfiguraties en systeembronnen worden altijd gedeeld op alle sites die in een omgeving worden geïmplementeerd. In dit document worden richtsnoeren gegeven om de effecten van deze gedeelde bronnen tot een minimum te beperken en worden suggesties gedaan om de communicatie en de samenwerking op deze gebieden te stroomlijnen.

## Voordelen en uitdagingen {#benefits-and-challenges}

Er zijn vele uitdagingen aan implementatie van een multi-huurdermilieu.

Deze omvatten:

* Aanvullende technische complexiteit
* Hogere ontwikkeloverhead
* Interorganisatorische afhankelijkheden van gedeelde bronnen
* Meer operationele complexiteit

Ondanks de uitdagingen waarmee u wordt geconfronteerd, heeft het uitvoeren van een toepassing met meerdere huurders wel voordelen, zoals:

* Lagere hardwarekosten
* Minder tijd op de markt voor toekomstige sites
* Lagere implementatiekosten voor toekomstige huurders
* Standaardarchitectuur en ontwikkelingspraktijken in het hele bedrijf
* Een algemene codebase

Als de zaken ware multi-tenancy, met nul kennis van andere huurders en geen gedeelde code, inhoud, of gemeenschappelijke auteurs vereisen, dan zijn de afzonderlijke auteursinstanties de enige levensvatbare optie. De totale toename van de ontwikkelingsinspanningen moet worden vergeleken met de besparingen op infrastructuur- en licentiekosten om te bepalen of deze aanpak het best geschikt is.

## Ontwikkelingstechnieken {#development-techniques}

### Afhankelijkheden beheren {#managing-dependencies}

Wanneer het leiden Geweven projectgebiedsdelen, is het belangrijk dat alle teams de zelfde versie van een bepaalde bundel OSGi op de server gebruiken. Om te illustreren wat er mis kan gaan wanneer Maven-projecten verkeerd worden beheerd, geven we een voorbeeld:

Project A is afhankelijk van versie 1.0 van de bibliotheek, foo; foo versie 1.0 is ingesloten in hun implementatie op de server. Project B is afhankelijk van versie 1.1 van de bibliotheek, foo; foo versie 1.1 is ingebed in hun plaatsing.

Bovendien, veronderstellen wij dat API in deze bibliotheek tussen versies 1.0 en 1.1 is veranderd. Op dit moment zal een van deze twee projecten niet meer naar behoren werken.

Om dit probleem aan te pakken, raden we aan alle Maven-projecten kinderen te maken van één ouder reactorproject. Dit reactorproject heeft twee doelen: het staat voor de bouw en de plaatsing van alle projecten samen toe indien zo gewenst, en het bevat de gebiedsdeelverklaringen voor alle kindprojecten. Het ouderproject bepaalt de gebiedsdelen en hun versies terwijl de kindprojecten slechts de gebiedsdelen verklaren die zij vereisen, die de versie van het ouderproject erven.

In dit scenario, als het team dat aan Project B werkt functionaliteit in versie 1.1 van foo vereist, zal het snel duidelijk in de ontwikkelomgeving worden dat deze verandering Project A zal breken. Op dit punt kunnen de teams deze verandering bespreken en of Project A compatibel maken met de nieuwe versie of zoeken naar een alternatieve oplossing voor Project B.

Merk op dat dit niet de behoefte voor deze teams elimineert om dit gebiedsdeel te delen-het benadrukt snel en vroeg enkel kwesties zodat de teams om het even welke risico&#39;s kunnen bespreken en over een oplossing kunnen stemmen.

### Codeduplicatie voorkomen {#preventing-code-duplication-nbsp-br}

Wanneer het werken aan veelvoudige projecten, is het belangrijk om ervoor te zorgen dat de code niet wordt gedupliceerd. Codeduplicatie verhoogt de kans op fouten, de kosten van wijzigingen in het systeem en de algemene rigiditeit in de codebasis. Om dubbel werk te voorkomen, refactor gemeenschappelijke logica in herbruikbare bibliotheken die over veelvoudige projecten kunnen worden gebruikt.

Om deze behoefte te steunen, adviseren wij de ontwikkeling en het onderhoud van een kernproject dat alle teams van kunnen afhangen en tot bijdragen. Daarbij is het van belang ervoor te zorgen dat dit kernproject op zijn beurt niet afhankelijk is van de projecten van de afzonderlijke teams. dit maakt een onafhankelijke inzetbaarheid mogelijk terwijl het hergebruik van code nog steeds wordt bevorderd .

Enkele voorbeelden van code die algemeen in een kernmodule zijn:

* Systeembrede configuraties, zoals:
   * OSGi-configuraties
   * Servlet-filters
   * ResourceResolver-toewijzingen
   * Sling Transformer-pijpleidingen
   * Fouthandlers (of gebruik de fouthandler 1 van de ACS-AEM Commons-pagina)
   * Machtigingsservices voor het in cache plaatsen van gegevens die gevoelig zijn voor machtigingen
* Hulpprogrammaklassen
* Core Business Logica
* Integratielogica van derden
* Authoring UI-overlays
* Andere aanpassingen die vereist zijn voor ontwerpen, zoals aangepaste widgets
* Workflowstartprogramma&#39;s
* Algemene ontwerpelementen die op verschillende sites worden gebruikt

*Modulaire projectarchitectuur*

Dit elimineert niet de behoefte voor veelvoudige teams om van en potentieel de zelfde reeks code af te hangen bij te werken. Door een kernproject te creëren, hebben wij de grootte van codebase verminderd die tussen teams-dalend maar niet de behoefte aan gedeelde middelen wordt gedeeld.

Om ervoor te zorgen dat de veranderingen die aan dit kernpakket worden aangebracht de functionaliteit van het systeem niet verstoren, adviseren wij dat een senior ontwikkelaar of een team van ontwikkelaars toezicht handhaven. Eén mogelijkheid is één team te hebben dat alle wijzigingen in dit pakket beheert. een andere is dat teams pull-verzoeken indienen die door deze middelen worden herzien en samengevoegd. Het is belangrijk dat de teams een bestuursmodel opstellen en ermee instemmen en dat ontwikkelaars dit model volgen.

## Implementatiebereik beheren  {#managing-deployment-scope}

Aangezien de verschillende teams hun code aan de zelfde bewaarplaats opstellen, is het belangrijk dat zij elkaars veranderingen niet beschrijven. AEM heeft een mechanisme om dit te controleren wanneer het opstellen van inhoudspakketten, de filter. xml-bestand. Het is belangrijk dat er geen overlapping is tussen filters.  xml- dossiers, anders kon de plaatsing van één team potentieel de vorige plaatsing van een ander team wissen. Ter illustratie hiervan raadpleegt u de volgende voorbeelden van goed gemaakte versus problematische filterbestanden:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs. /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Als elk team expliciet hun filterbestand configureert tot de site(s) waaraan ze werken, kan elk team hun componenten, clientbibliotheken en siteontwerpen onafhankelijk implementeren zonder elkaars wijzigingen te wissen.

Omdat het een globaal systeemweg en niet specifiek voor één plaats is, zou de volgende servlet in het kernproject moeten worden omvat, aangezien de hier aangebrachte veranderingen potentieel om het even welk team konden beïnvloeden:

/apps/sling/servlet/errorhandler

### Bedekkingen {#overlays}

Bedekkingen worden vaak gebruikt om de AEM van het vak uit te breiden of te vervangen, maar het gebruik van een bedekking beïnvloedt de gehele AEM toepassing (dat wil zeggen dat eventuele overlappende wijzigingen in de functionaliteit beschikbaar worden gemaakt voor alle huurders). Dit zou nog ingewikkelder zijn als huurders andere eisen voor de overlay zouden stellen. In het ideale geval zouden bedrijfsgroepen moeten samenwerken om overeenstemming te bereiken over de functionaliteit en de vormgeving van AEM administratieve consoles.

Als er geen consensus kan worden bereikt tussen de verschillende bedrijfseenheden, zou een mogelijke oplossing zijn om overlays eenvoudigweg niet te gebruiken. In plaats daarvan, creeer een douaneexemplaar van de functionaliteit en stel het via een verschillende weg voor elke huurder bloot. Dit staat elke huurder toe om een volledig verschillende gebruikerservaring te hebben, maar deze benadering verhoogt de kosten van implementatie en verdere verbeteringsinspanningen eveneens.

### Workflowstartprogramma&#39;s {#workflow-launchers}

AEM gebruikt werkstroomdraagraketten om werkstroomuitvoering automatisch te starten wanneer opgegeven wijzigingen in de opslagplaats worden aangebracht. AEM biedt verschillende draagraketten uit het vak, bijvoorbeeld om processen voor het genereren van vertoningen en het ophalen van metagegevens uit te voeren voor nieuwe en bijgewerkte elementen. Terwijl het mogelijk is om deze draagraketten zoals-is, in een multi-huurdermilieu te verlaten, als de huurders verschillende lancerings en/of werkschemamodel vereisten hebben, dan is het waarschijnlijk dat individuele draagraketten voor elke huurder zullen moeten worden gecreeerd en worden gehandhaafd. Deze draagraketten zullen moeten worden gevormd om op de updates van hun huurder uit te voeren terwijl het verlaten van inhoud van andere huurders onaangeroerd. Dit kunt u eenvoudig bereiken door draagraketten toe te passen op opgegeven opslagpaden die huurspecifiek zijn.

### Vanity-URL&#39;s {#vanity-urls}

AEM biedt vanity URL-functionaliteit die per pagina kan worden ingesteld. De zorg met deze benadering in een multi-huurdersscenario is dat AEM geen uniciteit tussen de ijdelheid URLs verzekert die op deze manier wordt gevormd. Als twee verschillende gebruikers hetzelfde ijdelingspad voor verschillende pagina&#39;s configureren, kan onverwacht gedrag optreden. Om deze reden, adviseren wij het gebruiken van mod_rewrite regels in de Apache berichtcher instanties, die voor een centraal punt van configuratie in overleg met de uitgaande-enige regels van de Resolver van het Middel toestaan.

### Componentgroepen {#component-groups}

Wanneer het ontwikkelen van componenten en malplaatjes voor veelvoudige auteursgroepen, is het belangrijk om effectief gebruik van componentGroup en allowedPaths eigenschappen te maken. Door deze effectief te gebruiken met plaatsontwerpen, kunnen wij ervoor zorgen dat de auteurs van merk A slechts componenten en malplaatjes zien die voor hun plaats zijn gecreeerd terwijl de auteurs van Brand B slechts hun hun zien.

### Testen {#testing}

Hoewel een goede architectuur en open communicatiekanalen de introductie van defecten in onverwachte gebieden van de site kunnen helpen voorkomen, zijn deze benaderingen niet onbestendig. Daarom is het belangrijk om volledig te testen wat er op het platform wordt ingezet voordat er iets in productie wordt genomen. Dit vereist coördinatie tussen teams tijdens hun releasecycli en versterkt de behoefte aan een reeks geautomatiseerde tests die zoveel mogelijk functionaliteit bestrijken. Bovendien, omdat één systeem door veelvoudige teams wordt gedeeld, worden de prestaties, de veiligheid, en de ladingstests belangrijker dan ooit.

## Operationele overwegingen {#operational-considerations}

### Gedeelde bronnen {#shared-resources}

AEM binnen één JVM wordt uitgevoerd; om het even welke opgestelde AEM toepassingen delen inherent middelen met elkaar, bovenop middelen die reeds in het normale lopen van AEM worden verbruikt. Binnen de ruimte JVM zelf, is er geen logische scheiding van draden, en de eindige middelen beschikbaar aan AEM, zoals geheugen, cpu, en schijf I/O wordt ook gedeeld. Om het even welke huurder die middelen verbruikt zal onvermijdelijk andere systeemhuurders beïnvloeden.

### Prestaties {#performance}

Als AEM beste praktijken niet volgen, is het mogelijk om toepassingen te ontwikkelen die middelen voorbij gebruiken wat als normaal wordt beschouwd. De voorbeelden van dit leiden tot vele zware werkschemaverrichtingen (zoals DAM Update Asset), het gebruiken van druk-op-wijzigen verrichtingen MSM over vele knopen, of het gebruiken van dure vragen JCR om inhoud in real time terug te geven. Dit zal onvermijdelijk een effect op de prestaties van andere huurderstoepassingen hebben.

### Logboekregistratie {#logging}

AEM verstrekt uit de doosinterfaces voor robuuste logboekconfiguratie die aan ons voordeel in gedeelde ontwikkelingsscenario&#39;s kan worden gebruikt. Door afzonderlijke loggers op te geven voor elk merk, per pakketnaam, kunnen we een bepaalde mate van logscheiding bereiken. Terwijl de verrichtingen voor het hele systeem zoals replicatie en authentificatie nog aan een centrale plaats zullen worden geregistreerd, kan de niet-gedeelde douanecode afzonderlijk worden geregistreerd, verlichtend controle en het zuiveren inspanningen voor het technische team van elk merk.

### Back-up en herstel {#backup-and-restore}

Vanwege de aard van de JCR-opslagplaats werken traditionele back-ups in de gehele opslagplaats in plaats van op een afzonderlijk inhoudspad. Het is daarom niet mogelijk om steunen op een per-huurdersbasis gemakkelijk te scheiden. Omgekeerd worden bij het herstellen van een back-up inhoud en opslagknooppunten teruggedraaid voor alle gebruikers van het systeem. Terwijl het mogelijk is om gerichte inhoudsteunen uit te voeren, gebruikend hulpmiddelen zoals VLT, of om inhoud te koesteren om te herstellen door een pakket in een afzonderlijke milieu te bouwen, deze\
de benaderingen omvatten niet gemakkelijk configuratiemontages of toepassingslogica en kunnen omslachtig zijn te beheren.

## Beveiligingsoverwegingen {#security-considerations}

### ACLs {#acls}

Het is natuurlijk mogelijk om de Lijsten van het Toegangsbeheer (ACLs) te gebruiken om te controleren wie toegang tot mening heeft, creeert, en schrapt inhoud die op inhoudspaden wordt gebaseerd, die de verwezenlijking en het beheer van gebruikersgroepen vereist. De moeilijkheid om ACLs en de groepen te handhaven hangt van de nadruk af bij het verzekeren van elke huurder nul kennis van anderen heeft en of de opgestelde toepassingen op gedeelde middelen vertrouwen. Om efficiënte ACL, gebruiker, en groepsbeleid te verzekeren, adviseren wij het hebben van een gecentraliseerde groep met het noodzakelijke toezicht om ervoor te zorgen dat deze toegangscontroles en de hoofden overlappen (of niet overlappen) op een manier die efficiency en veiligheid bevordert.
