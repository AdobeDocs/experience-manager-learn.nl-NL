---
title: Site-hiërarchie, Taxonomy en Tagging Guide
description: Een volledig overzicht van AEM Sites-metagegevens, codering, taxonomie en hiërarchie. Gebruik deze handleiding om ervoor te zorgen dat uw inhoudsstrategie consistent is en de beste praktijken volgt
topic: Content Management
feature: Learn From Your Peers
role: Admin, User
jira: KT-14254
level: Beginner, Intermediate
doc-type: Article
exl-id: c88c3ec7-9060-43e2-a6a2-d47bba6f7cf3
duration: 437
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2035'
ht-degree: 0%

---

# Aanbevolen werkwijzen voor tags, taxonomie en metagegevens: overzicht op hoog niveau

Metagegevens en codes zijn van wezenlijk belang voor de efficiëntie van AEM. Gebruikers, leiders en managers realiseren zich de noodzaak van een holistische strategie, maar ze vinden het moeilijk om vooruitgang te boeken. Vaak wordt kennis verdeeld onder gebruikers, die holistische strategie moeilijk-maken en aanpassingen nog problematischer maken.

Wat is het verschil tussen metagegevens en tags? Wat zijn de bedrijfsaspecten om in overweging te nemen wanneer het drijven van uw strategie?

## Wat is het doel van metagegevens?

Metagegevens voegen structuur toe aan minder gestructureerde inhoud.
Voorbeeld: een basisafbeelding heeft pixels. We kunnen die &#39;kerngegevens&#39; noemen. Het zijn de metagegevens die het formaat, de categorie, licentiegegevens, enz. beschrijven.
Metagegevens worden het vaakst gebruikt voor elementen. Maar er is een groot aantal gevallen waarin metagegevens in inhoudspagina&#39;s worden gebruikt of waarin fragmenten worden aangetroffen.

## Bronnen van metagegevens

Hieronder volgt een overzicht van de categorieën waarvoor metagegevens kunnen worden gegenereerd:

* Geëxtraheerde metagegevens - De informatie is al beschikbaar in het document, bijvoorbeeld in natuurlijke taal.
* Afgeleide metagegevens - De informatie is niet beschikbaar in de oorspronkelijke gegevens, maar kan worden afgeleid door naar elkaar te verwijzen eerdere kennis.
* Handmatig toegevoegde metagegevens - Dit zijn metagegevens die niet in een van de eerste categorieën vallen en die handmatig door een mens moeten worden toegevoegd.

## Typen metagegevens

Binnen de bovenstaande categorieën zijn er vier hoofdtypen:

* Technische en beschrijvende metagegevens: informatie over technische details van inhoud (bijvoorbeeld titel, taal, enz.)
* Operationele metagegevens: documenteert de levenscyclus van een middel (d.w.z. goedgekeurd, in creatieve zin, campagne)
* Administratieve metagegevens: de status of status van een actief binnen een organisatie (d.w.z. licentiegegevens, eigendom)
* Structurele metagegevens: helpt elementen of pagina&#39;s te categoriseren voor een vloeiend bedrijfsproces (van toepassing op de meeste tags en taxonomieën)

## Map- en bestandsnamen

Mappen zijn een natuurlijke manier om door de inhoud in AEM te bladeren. Hoe gaan uw belanghebbenden met AEM communiceren? Hiermee bepaalt u de structuur van uw mappen. Normaal gesproken is de mapstructuur met een (of twee) van de volgende inzichten gemaakt:

* Navigatie
* Bladeren
* Indeling
* Toegangsbeheer

Voor AEM Sites is navigatie sleutel. Mappen worden gebruikt om de toegang tot de elementen en pagina&#39;s te beheren.

Welke rijen auteurs hebben toegang tot homepages nodig? Hoe zit het met productpagina&#39;s? Of campagne? Gebruik machtigingen en mapstructuur om de juiste governance in te stellen.

## Metagegevens opslaan

Er zijn drie manieren om metagegevens op te slaan:

* Binair: binaire indeling die gerelateerd is aan de aard van het element (Photoshop, InDesign, PNG, JPG).
* Middelenknooppunt: dit zijn metagegevens over het element zelf, ongeacht het systeem of proces dat wordt gebruikt.
* Externe locatie: metagegevens die zich niet rechtstreeks op het element bevinden, maar die kunnen worden gebruikt als een beschrijving van de status van een element (bijvoorbeeld: een workflow die invloed kan hebben op een element, maar die niet rechtstreeks op het element is toegepast)

## Metagegevensmodel

De structuur van hoe metagegevens worden vastgelegd en opgemaakt, wordt het metagegevensmodel of het metagegevensschema genoemd. Hierover moet overeenstemming worden bereikt voordat elementen of pagina&#39;s in het systeem worden opgenomen.

Een metagegevensmodel is gewoonlijk ontworpen om aan de volgende gebruiksgevallen te voldoen:

* Zoeken en ophalen: hiermee kunt u belangrijke aspecten van inhoud opslaan, zodat bedrijven deze eenvoudig kunnen ophalen.
* Hergebruik: helpt oude middelen te gebruiken voor hergebruik (tijd en geld besparen)
* Licentiebeheer: houdt het eigendom van bedrijfsmiddelen (vaak om juridische redenen) bij
* Distribueren: inhoud beschikbaar stellen aan consumenten of elementen synchroniseren aan partners.
* Archief: Metagegevens die een element aanduiden, zijn verouderd (altijd de beste manier om een &quot;gearchiveerde&quot; markering op het element te plaatsen zodat essentiële informatie niet verloren gaat)/
* Kruisverwijzing: associatieve metagegevens die de relatie van twee of meer elementen met elkaar vastleggen (de synthese van metagegevens maakt kruisverwijzingen en coherente groepsorganisatie mogelijk)
* Navigeren: de mapstructuur waarin elementen zijn opgeslagen (gebruikt om informatie op te halen door te bladeren)

*de meta-gegevens van de Auteur steunen hoofdzakelijk operationele processen. Publish steunt herwinning en distributie gebruiksgevallen.*

## Labels als vooraf gedefinieerde termen gebruiken

Een tag is een trefwoord of term die is toegewezen aan een informatie.vIn plaats van &quot;auto&quot;, &quot;voertuig&quot;, &quot;auto&quot; in te voeren, kan een labelsysteem bijvoorbeeld maar één waarde kiezen, waardoor het zoeken voorspelbaarder wordt.  Met labels wordt de categorisering van elementen genormaliseerd en vereenvoudigd.

*Nota: Hoewel AEM ad-hoc het etiketteren toestaat is het beste praktijken om van dit te blijven aangezien het tot onbepaalde en onhandelbare taxonomie zou kunnen leiden.*

Veelvoorkomende toepassingen van tags:

* Trefwoordzoeker: een tag kan beschrijven dat een resource tot een bepaalde groep entiteiten behoort. Met bijvoorbeeld een tag &quot;image/subject/car&quot; wordt aangegeven dat de bron behoort tot de reeks afbeeldingen die een auto tonen.
* Relaties tussen drivers: alle bronnen met dezelfde tag kunnen worden beschouwd als verbonden. Tags toevoegen in plaats van rechtstreeks koppelen is vooral handig op websites met veel dynamische en verbonden inhoud.
* Schijfnavigatie: tags die in hiërarchische taxonomie zijn geordend, kunnen navigatie, een koppeling of koppeling naar vergelijkbare documenten maken.
Tags moeten ook worden beschouwd als informatie die verschillende soorten gegevens met elkaar verbindt, op basis van zakelijke voorwaarden en niet op basis van technische eigenschappen.

## Algemene toepassingen van tags

Als tags worden gebruikt in AEM, kan dit een veel kortere implementatie van de complexe functie tot gevolg hebben, zoals:

* Gefactureerde zoekopdracht
* Persoonlijke navigatie
* Gerelateerde inhoud
* Content References
* Optimalisatie zoekmachine
* Belangrijke concepten markeren

## Taxonomieën

Een taxonomie is een systeem om markeringen te organiseren die op gedeelde kenmerken worden gebaseerd, die gewoonlijk hiërarchisch gestructureerd per organisatorische behoefte zijn. De structuur kan u helpen om sneller een tag te vinden of een generalisatie op te leggen.
Voorbeeld: Het is nodig om de beeldvorming van auto&#39;s te subcategoriseren.  De taxonomie kan er als volgt uitzien:

/subject/car/
/subject/car/sportscar
/subject/car/sportscar/porsche
/subject/car/sportscar/ferrari
...
/subject/car/minibus
/subject/car/minivan/mercedes
/subject/car/minivan/volkswagen
...
/onderwerp/auto/limousine
...

Nu kan een gebruiker kiezen of hij beelden van sportscars in het algemeen of een &quot;Porsche&quot; in het bijzonder wil opzoeken. Het zijn immers allebei sportliters.
Beste praktijken: vermijd vlakke taxonomieën. Platte taxonomieën hebben niet de hierboven beschreven voordelen en vereisen constant onderhoud

**Gebruikend een Taxonomie als Synoniemenlijst.** Wanneer een gebruiker naar een trefwoord zoekt, maakt het systeem een tweede zoekopdracht naar alle synoniemen die daar worden gevonden.
Bovendien kan het systeem in plaats van &quot;auto&quot; handmatig te typen een lijst met trefwoorden leveren om de consistentie te verbeteren.

**Gebruikend een Taxonomie als Woordenboek.** In plaats van alleen maar &quot;car&quot; af te drukken, kunt u de enkele tag uitvouwen en alle synoniemen van de tag gebruiken.

**veelvoudige Categorieën.** In tegenstelling tot een mappenhiërarchie kunnen tags worden gebruikt om meerdere categorieën tegelijk uit te drukken. Een element dat is gelabeld met:

/subject/car/minivan/mercedes
/subject/people/family
/color/red

## Metagegevens vs. tag

Niet alle metagegevens moeten worden beschouwd als een kandidaat voor het coderingssysteem. Technische metagegevens kunnen de informatie onnodig dupliceren. De beste keuze voor tags is zakelijke metagegevens. .Tags zijn een goede keuze voor het afdwingen van consistente woordenlijsten, gezichtszoekopdrachten en navigatie.

## Tag Management

Tagbeheer is een speciaal kernteam. Nieuwe leden moeten eerst het doel en de functie van de taxonomie leren voordat ze nieuwe tags toevoegen.  Deskundigen met een zaadje, die als gatekeer naar nieuwe tags gaan, zullen de inconsistentie op lange termijn verminderen.

## Tags maken

Taxonomieën moeten door inhoudauteurs worden gebruikt en door eindgebruikers worden begrepen. Deze moeten worden gemaakt voordat de inhoud wordt gemaakt. Eventuele sneltoetsen zullen leiden tot extra inspanningen voor beheer en onderhoud.

## Doorlopend onderhoud

De zaken veranderen en ook de behoeften van de taglijst zullen veranderen. Ontdek het onderhoudsproces dat dubbel werk vermindert.

Zorg ervoor dat gebruikers die inhoud leveren weten hoe ze wijzigingen kunnen voorstellen en dat editors of inhoudsbeheerders de voorwaarden regelmatig evalueren.

## Beste praktijken met Markeringen en Taxonomieën

**normaliseer Markeringen.** Maak een verklarende woordenlijst die een gezaghebbende woordenlijst biedt. Als er geen normen worden vastgesteld, zal er sprake zijn van dubbel werk. Daarnaast wordt aanbevolen niet alleen de taxonomie maar ook het gebruik van de labels te controleren.

**niet over-markering.** Tags kunnen hun betekenis verliezen als ze te vaak worden gedistribueerd.Verwijder vreemde tags voor optimale efficiëntie.

**herevalueer markeringen in tijd.** Houd er rekening mee dat de bedrijfsterminologie en de zakelijke context zelden statisch blijven. Mogelijk is het nodig om tags opnieuw te standaardiseren en opnieuw toe te passen.

**Gebruikend AI-aangedreven Slimme Tags.** het Slimme etiketteren [ zie verbinding ] is een vermogen AI in AEM om de inspanning te verminderen om activa manueel te etiketteren. Slimme tags toepassen gebruikt een AI om informatie over het onderwerp van een afbeelding af te leiden. Er worden beschrijvende codes gemaakt die de inhoud van een afbeelding beschrijven.

## Metagegevenskwaliteit en -onderhoud

Het begrip van bedrijfsvereisten is een belangrijke stap in het uitvoeren van een model van het meta-gegevensbeheer. Zonder definitie kan de informatie niet worden opgeslagen. Het model moet periodiek worden herzien. Dit is een essentiële kwaliteitscontrole.

Bovendien moeten metagegevens zo vroeg mogelijk worden vastgelegd tijdens het maken van de inhoud. Als metagegevens niet op het juiste moment worden toegepast, is er weinig kans om deze retroactief toe te passen.

**gebruik meta-gegevens** om samenwerking te verbeteren: Gebruik de Verbinding van Activa van de Adobe, Adobe Bridge en de Desktop van de AEM om creatief proces samen te binden en meta-gegevens te gebruiken om creatieve werkschema&#39;s te stroomlijnen. Met deze gereedschappen verrijkt u metagegevens en de gebruikerservaring in uw creatieve proces.

## Aanbevolen procedures voor metagegevensbeheer

* Wijs het Team van de Kern met Sterk Uitvoerend Mandaat toe: Vorm een team van de meta-gegevensKern dat volledig inzicht in het bedrijfsecosysteem en een sterk mandaat door het beheer van de organisatie heeft.
* Metagegevensstrategie en -beheer definiëren: een goede metagegevensstrategie kan de organisaties helpen om de behoefte aan en de voordelen van de metagegevens uit te leggen.  Een strategie bestaat uit: metagegevensschema(&#39;s), taxonomie, bedrijfsprocessen (voor gegevenskwaliteit en -vastlegging), rollen en verantwoordelijkheden, en governanceprocessen. *
* Definieer en communiceer het Consistente Metagegevensmodel: een gedefinieerde strategie en redenering moeten goed worden gedocumenteerd en moeten worden gecommuniceerd binnen de organisaties.
* Standaard naamgevingsconventie: consistente en beschrijvende naamgevingsconventie voor bestanden maken voor verbeterde branding, informatiebeheer en bruikbaarheid.
* Veilige tekens in bestandsnamen: de bestandsnaam moet door alle besturingssystemen kunnen worden geïnterpreteerd. U kunt veilig tekens, getallen, umlauts, spaties en onderstreping gebruiken. Het minteken is ook veilig, maar als u knipt en plakt, ziet het eruit als een streepje.
* Conventie over naamgeving van versies: AEM biedt bepaalde mogelijkheden om eerdere versies van middelen te behouden. In sommige gevallen wilt u wellicht verschillende versies behouden. Nochtans, zou u ervoor moeten zorgen dat het versioning schema verenigbaar is.

## Organisatorische versus beschrijvende metagegevens

Een paar richtlijnen kunnen u helpen om te beslissen hoe te om meta-gegevens te categoriseren:

**Beschrijving** - als het gegeven het element of het stuk van inhoud beschrijft zou het deel van de meta-gegevens in bijlage moeten zijn.

**Onderzoek** - als de meta-gegevens in onderzoek zullen worden gebruikt moet het in bijlage zijn.

**Blootstelling** - als u de meta-gegevens op een distributieplatform aan een derde blootstelt, ben zorgvuldig om &quot;interne&quot;meta-gegevens niet ook bloot te stellen.

**Duur** - langer wordt de meta-gegevens verondersteld om te leven, waarschijnlijker is het een goede kandidaat voor meta-gegevens in bijlage te zijn.

**Verwante BedrijfsProcessen** - het is beslist nuttig om een permanente productidentiteitskaart als deel van de meta-gegevens te hebben. Maar de categorie van een item in relatie tot de productcatalogus is een twijfelachtige metagegevens voor het element.

**Organisatie en Verwerking** - als de aard van de meta-gegevens van organisatorische aard, zoals staat in een goedkeuringswerkschema of eigendom van een bepaalde afdeling is, zouden de externe meta-gegevens over het vastmaken van de meta-gegevens aan het middel moeten worden overwogen.

*om de strategie tot stand te brengen, stel de volgende vragen:*

* Welk type van inhoud en &quot;extra informatie&quot; (= meta-gegevens) is nodig om bedrijfsproblemen/bedrijfsvragen/bedrijfskwesties op te lossen?
* Wat zijn de variabelen, de &quot;gebieden&quot;in het schema, en wat zijn mogelijke waarden? Welke variabelen een vrije-tekstinput nodig hebben, die op type (aantal, datum, boolean, ...), een reeks vaste waarden (bijvoorbeeld landen) of markeringen van een bepaalde taxonomie kan worden versmald. Hoeveel tags zijn vereist, toegestaan?
* Welke technische problemen/problemen/vragen kunnen door meta-gegevens worden opgelost?
* Hoe kunt u die inhoud/metagegevens verkrijgen/maken? Hoeveel kost het om die metagegevens te verkrijgen/maken?
* Welke soorten meta-gegevens worden vereist voor een bepaalde gebruikersgroep?
* Hoe worden metagegevens onderhouden en bijgewerkt?
* Wie is verantwoordelijk voor welk deel van het proces?
* Hoe kunt u verzekeren dat de overeengekomen bedrijfsprocessen worden gevolgd?
* Welke normen moet u volgen? Moet u een industriestandaard aannemen en wijzigen (Dublin Core, ISO 19115, PRISM, enz.) of moet de organisatie haar eigen normen opstellen ?
* Waar wordt de strategie gedocumenteerd? Hoe kunt u ervoor zorgen dat alle belanghebbenden toegang hebben? Hoe kunt u ervoor zorgen dat nieuw aangeworven personeel zich aan de overeengekomen norm houdt (bijvoorbeeld een bezoek aan een training voordat u toegang krijgt?)
