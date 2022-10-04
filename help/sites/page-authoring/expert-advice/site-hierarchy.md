---
title: Site-hiërarchie, Taxonomy en Tagging Guide
seo-title: AEM Sites Site Hierarchy, Taxonomy, and Tagging Guide
description: Een volledig overzicht van AEM Sites-metagegevens, codering, taxonomie en hiërarchie. Gebruik deze handleiding om ervoor te zorgen dat uw inhoudsstrategie consistent is en de beste praktijken volgt
seo-description: A full overview of AEM Sites metadata, tagging, taxonomy, and hierarchy. Use this guide to ensure your content strategy is consistent and following best practices
audience: author, marketer
source-git-commit: d545e7bb5e937959e2ede2b3c1ecfc312df5a044
workflow-type: tm+mt
source-wordcount: '2034'
ht-degree: 0%

---


# Aanbevolen werkwijzen voor tags, taxonomie en metagegevens: Samenvatting op hoog niveau

Metagegevens en codes zijn van wezenlijk belang voor de efficiëntie van AEM. Gebruikers, leiders en managers realiseren zich de noodzaak van een holistische strategie, maar ze vinden het moeilijk om vooruitgang te boeken. Vaak wordt kennis verdeeld onder gebruikers, die holistische strategie moeilijk-maken en aanpassingen nog problematischer maken.

Wat is het verschil tussen metagegevens en tags? Wat zijn de bedrijfsaspecten om in overweging te nemen wanneer het drijven van uw strategie?

## Wat is het doel van metagegevens?

Metagegevens voegen structuur toe aan minder gestructureerde inhoud.
Voorbeeld: Een basisafbeelding heeft pixels. We kunnen die &#39;kerngegevens&#39; noemen. Het zijn de metagegevens die het formaat, de categorie, licentiegegevens, enz. beschrijven.
Metagegevens worden het meest gebruikt voor elementen. Maar er is een groot aantal gevallen waarin metagegevens in inhoudspagina&#39;s worden gebruikt of waarin fragmenten worden aangetroffen.

## Bronnen van metagegevens

Hieronder volgt een overzicht van de categorieën waarvoor metagegevens kunnen worden gegenereerd:

* Geëxtraheerde metagegevens - De informatie is al beschikbaar in het document, bijvoorbeeld in natuurlijke taal.
* Afgeleide metagegevens - De informatie is niet beschikbaar in de oorspronkelijke gegevens, maar kan worden afgeleid door naar elkaar te verwijzen eerdere kennis.
* Handmatig toegevoegde metagegevens - Dit zijn metagegevens die niet in een van de eerste categorieën vallen en die handmatig door een mens moeten worden toegevoegd.

## Typen metagegevens

Binnen de bovenstaande categorieën zijn er vier hoofdtypen:

* Technische en beschrijvende metagegevens: Verstrekt informatie over technische details van inhoud (d.w.z. titel, taal, enz.)
* Operationele metagegevens: Documenteert de levenscyclus van een middel (d.w.z. goedgekeurd, in creatief, campagne)
* Administratieve metagegevens: De status of status van een actief binnen een organisatie (d.w.z. licentiegegevens, eigendom)
* Structuurmetagegevens: Helpt elementen of pagina&#39;s te categoriseren voor een vloeiend bedrijfsproces (van toepassing op de meeste labels en taxonomieën)

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

* Binair: Binaire indeling heeft betrekking op de aard van het element (Photoshop, InDesign, PNG, JPG).
* Middelenknooppunt: Dit zijn metagegevens over het element zelf, ongeacht het systeem of proces dat wordt gebruikt.
* Externe locatie: Metagegevens die zich niet rechtstreeks op het element bevinden, maar kunnen worden gebruikt als een beschrijving van de &quot;status&quot; van een element (voorbeeld: een werkstroom die een element kan beïnvloeden maar niet direct op het wordt toegepast)

## Metagegevensmodel

De structuur van de manier waarop metagegevens worden vastgelegd en opgemaakt, wordt het metagegevensmodel of het metagegevensschema genoemd. Hierover moet overeenstemming worden bereikt voordat elementen of pagina&#39;s in het systeem worden opgenomen.

Een metagegevensmodel is gewoonlijk ontworpen om aan de volgende gebruiksgevallen te voldoen:

* Zoeken en ophalen: Helpt belangrijke aspecten van inhoud op te slaan die gemakkelijk terugwinning door zaken helpen.
* Opnieuw gebruiken: Helpt oude middelen te gebruiken voor hergebruik (tijd en geld besparen)
* Licentiebeheer: Houdbaarheid van activa door de organisatie volgen (vaak om juridische redenen)
* Distribueren: Inhoud ter beschikking stellen van consumenten of bedrijfsmiddelen synchroniseren.
* Archief: Metagegevens die een element aanduiden, zijn verouderd (altijd aanbevolen om een &quot;gearchiveerde&quot; markering op een element te plaatsen zodat essentiële informatie niet verloren gaat)/
* Kruisverwijzing: Associatieve metagegevens die de relatie van twee of meer elementen met elkaar vastleggen (de synthese van metagegevens maakt kruisverwijzingen en coherente groepsorganisatie mogelijk)
* Navigeren: De mapstructuur waarin elementen zijn opgeslagen (wordt gebruikt om informatie op te halen door te bladeren)

*Metagegevens van auteurs ondersteunen hoofdzakelijk operationele processen. Publiceren ondersteunt Gebruiksscenario&#39;s voor ophalen en distribueren.*

## Labels als vooraf gedefinieerde termen gebruiken

Een tag is een trefwoord of term die is toegewezen aan een informatie.vIn plaats van &quot;auto&quot;, &quot;voertuig&quot;, &quot;auto&quot; in te voeren, kan een labelsysteem bijvoorbeeld maar één waarde kiezen, waardoor het zoeken voorspelbaarder wordt.  Met labels wordt de categorisering van elementen genormaliseerd en vereenvoudigd.

*Opmerking: Hoewel AEM ad-hoclabeling mogelijk maakt, is het raadzaam om dit te handhaven, aangezien dit tot een ongedefinieerde en omslachtige taxonomie kan leiden.*

Veelvoorkomende toepassingen van tags:

* Trefwoord zoeken: Een tag kan beschrijven dat een resource tot een bepaalde groep entiteiten behoort. Met bijvoorbeeld een tag &quot;image/subject/car&quot; wordt aangegeven dat de bron behoort tot de reeks afbeeldingen die een auto tonen.
* Rijrelaties: Alle bronnen met dezelfde tag kunnen worden beschouwd als verbonden. Tags toevoegen in plaats van rechtstreeks koppelen is vooral handig op websites met veel dynamische en verbonden inhoud.
* Schijfnavigatie: Met labels die in hiërarchische taxonomie zijn geordend, kunt u navigatie, een koppeling of koppeling naar vergelijkbare documenten maken.
Tags moeten ook worden beschouwd als informatie die verschillende soorten gegevens met elkaar verbindt, op basis van zakelijke voorwaarden en niet op basis van technische eigenschappen.

## Algemene toepassingen van tags

Als tags worden gebruikt in AEM, kan dit een veel kortere implementatie van de complexe functie tot gevolg hebben, zoals:

* Gefactureerd zoeken
* Persoonlijke navigatie
* Gerelateerde inhoud
* Content References
* Optimalisatie zoekmachine
* Belangrijke concepten markeren

## Taxonomieën

Een taxonomie is een systeem om markeringen te organiseren die op gedeelde kenmerken worden gebaseerd, die gewoonlijk hiërarchisch gestructureerd per organisatorische behoefte zijn. De structuur kan u helpen om sneller een tag te vinden of een generalisatie op te leggen.
Voorbeeld: Het is noodzakelijk de beeldvorming van auto&#39;s in categorieën te onderverdelen.  De taxonomie kan er als volgt uitzien:

/subject/car//subject/car/sportscar /subject/car/sportscar/porsche /subject/car/sportscar/ferrari ... /subject/car/minivan /subject/car/minivan/mercedes /subject/car/minivan/volkswagen .../subject/car/limousine ...

Nu kan een gebruiker kiezen of hij beelden van sportscars in het algemeen of een &quot;Porsche&quot; in het bijzonder wil opzoeken. Want beide zijn sportliters.
Beste praktijken: Vermijd platte taxonomieën. Platte taxonomieën hebben niet de hierboven beschreven voordelen en vereisen constant onderhoud

**Een taxonomie gebruiken als Thesaurus.**  Wanneer een gebruiker naar een sleutelwoord zoekt, leidt het systeem tot een tweede onderzoek naar alle synoniemen die daar worden gevonden.
Bovendien kan het systeem in plaats van &quot;auto&quot; handmatig te typen een lijst met trefwoorden leveren om de consistentie te verbeteren.

**Een Taxonomy als woordenboek gebruiken.** In plaats van alleen maar &quot;car&quot; af te drukken, kunt u de enkele tag uitbreiden en alle synoniemen van de tag gebruiken.

**Meerdere categorieën.** In tegenstelling tot een mappenhiërarchie kunnen tags worden gebruikt om meerdere categorieën tegelijk uit te drukken. Een middel gelabeld met:

/subject/car/minivan/mercedes/subject/people/family/color/red

## Metagegevens vs. tag

Niet alle metagegevens moeten worden beschouwd als een kandidaat voor het coderingssysteem. Technische metagegevens kunnen de informatie onnodig dupliceren. De beste keuze voor tags is zakelijke metagegevens. .Tags zijn een goede keuze voor het afdwingen van consistente woordenlijsten, beperkte zoekopdrachten en navigatie.

## Tag Management

Tagbeheer is een speciaal kernteam. Nieuwe leden moeten eerst het doel en de functie van de taxonomie leren voordat ze nieuwe tags toevoegen.  Deskundigen met een zaadje, die als gatekeer naar nieuwe tags gaan, zullen de inconsistentie op lange termijn verminderen.

## Tags maken

Taxonomieën moeten door inhoudauteurs worden gebruikt en door eindgebruikers worden begrepen. Deze moeten worden gemaakt voordat de inhoud wordt gemaakt. Eventuele sneltoetsen zullen leiden tot extra inspanningen voor beheer en onderhoud.

## Doorlopend onderhoud

De zaken veranderen en ook de behoeften van de taglijst zullen veranderen. Ontdek het onderhoudsproces dat dubbel werk vermindert.

Zorg ervoor dat gebruikers die inhoud leveren weten hoe ze wijzigingen kunnen voorstellen en dat editors of inhoudsbeheerders de voorwaarden regelmatig evalueren.

## Beste praktijken met Markeringen en Taxonomieën

**Tags standaardiseren.** Creeer verklarende woordenlijst die een gebiedende woordenlijst verstrekt. Als er geen normen worden vastgesteld, zal er sprake zijn van dubbel werk. Daarnaast wordt aanbevolen niet alleen de taxonomie maar ook het gebruik van de labels te controleren.

**Niet te veel tags toepassen.** Tags kunnen hun betekenis verliezen als ze te vaak worden gedistribueerd.Verwijder vreemde tags voor optimale efficiëntie.

**Tags na verloop van tijd opnieuw evalueren.** Herinner dat bedrijfsterminologie en bedrijfscontext zelden statisch blijven. Mogelijk is het nodig om tags opnieuw te standaardiseren en opnieuw toe te passen.

**Slimme tags toepassen met behulp van AI** Slimme tags toepassen [zie koppeling] is een AI-mogelijkheid in AEM om het handmatig labelen van activa te beperken. Slimme tags toepassen gebruikt een AI om informatie over het onderwerp van een afbeelding af te leiden. Er worden beschrijvende codes gemaakt die de inhoud van een afbeelding beschrijven.

## Metagegevenskwaliteit en -onderhoud

Het begrip van bedrijfsvereisten is een belangrijke stap in het uitvoeren van een model van het meta-gegevensbeheer. Zonder definitie kan de informatie niet worden opgeslagen. Het model moet periodiek worden herzien. Dit is een essentiële kwaliteitscontrole.

Bovendien moeten metagegevens zo vroeg mogelijk worden vastgelegd tijdens het maken van de inhoud. Als metagegevens niet op het juiste moment worden toegepast, is er weinig kans om deze retroactief toe te passen.

**Metagegevens gebruiken** de samenwerking verbeteren : Gebruik Adobe Asset Link, Adobe Bridge en het AEM Desktop om creatief proces te koppelen en metagegevens te gebruiken om creatieve workflows te stroomlijnen. Met deze gereedschappen verrijkt u metagegevens en de gebruikerservaring in uw creatieve proces.

## Aanbevolen procedures voor metagegevensbeheer

* Core Team toewijzen met sterk uitvoerend mandaat: Vorm een meta-gegevens kernteam dat volledig inzicht in het bedrijfseconomische systeem en een sterk mandaat door het beheer van de organisatie heeft.
* Metagegevensstrategie en -beheer definiëren: Een goede metagegevensstrategie kan de organisaties helpen om de behoefte aan en de voordelen van de metagegevens uit te leggen.  Een strategie bestaat uit: metagegevensschema(&#39;s), taxonomie, bedrijfsprocessen (voor gegevenskwaliteit en -vastlegging), rollen en verantwoordelijkheden, en governanceprocessen. *
* Consistente metagegevensmodellen definiëren en communiceren: Gedefinieerde strategie en redenering moeten goed gedocumenteerd zijn en binnen de organisaties worden meegedeeld.
* Standaardnaamgevingsconventie: Maak een consistente en beschrijvende naamgevingsconventie voor bestanden voor verbeterde branding, informatiebeheer en bruikbaarheid.
* Veilige tekens in bestandsnamen: Bestandsnaam moet kunnen worden geïnterpreteerd door alle gangbare besturingssystemen. U kunt veilig tekens, getallen, umlauts, spaties en onderstreping gebruiken. Het minteken is ook veilig, maar als u knipt en plakt, ziet het eruit als een streepje.
* Naamgevingsconventie versie: AEM biedt bepaalde mogelijkheden om eerdere versies van middelen te behouden. In sommige gevallen wilt u mogelijk meerdere versies behouden. Nochtans, zou u ervoor moeten zorgen dat het versioning schema verenigbaar is.

## Organisatorische versus beschrijvende metagegevens

Een paar richtlijnen kunnen u helpen om te beslissen hoe te om meta-gegevens te categoriseren:

**Beschrijving** - Als de gegevens het element of de inhoud beschrijven, moet het onderdeel zijn van de bijgevoegde metagegevens.

**Zoeken** - Indien de metagegevens bij de zoekactie worden gebruikt, moeten zij als bijlage worden gevoegd.

**Belichting** - Als u de metagegevens op een distributieplatform toegankelijk maakt voor derden, moet u ervoor zorgen dat u de &#39;interne&#39; metagegevens niet ook toegankelijk maakt.

**Duur** - Hoe langer de metagegevens moeten leven, des te groter de kans dat ze geschikt zijn voor de metagegevens in de bijlage.

**Verwante bedrijfsprocessen** - Het is zeker handig om een permanente product-id te hebben als onderdeel van de metagegevens. Maar de categorie van een item in relatie tot de productcatalogus is een twijfelachtige metagegevens voor het element.

**Organisatie en verwerking** - Als de aard van de metagegevens van organisatorische aard is, zoals de status in een goedkeuringswerkstroom of de eigendom van een bepaalde afdeling, moeten externe metagegevens worden overwogen over het koppelen van de metagegevens aan het element.

*Stel de volgende vragen om de strategie te maken:*

* Welk type van inhoud en &quot;extra informatie&quot; (= meta-gegevens) is nodig om bedrijfsproblemen/bedrijfsvragen/bedrijfskwesties op te lossen?
* Wat zijn de variabelen, de &quot;gebieden&quot;in het schema, en wat zijn mogelijke waarden? Welke variabelen een vrije-tekstinput nodig hebben, die degenen door type (aantal, datum, boolean, ...), een reeks vaste waarden (b.v. landen) of markeringen van een bepaalde taxonomie kunnen worden versmald. Hoeveel tags zijn vereist, toegestaan?
* Welke technische problemen/problemen/vragen kunnen door meta-gegevens worden opgelost?
* Hoe kunt u die inhoud/metagegevens verkrijgen/maken? Hoeveel kost het om die metagegevens te verkrijgen/maken?
* Welke soorten meta-gegevens worden vereist voor een bepaalde gebruikersgroep?
* Hoe worden metagegevens onderhouden en bijgewerkt?
* Wie is verantwoordelijk voor welk deel van het proces?
* Hoe kunt u verzekeren dat de overeengekomen bedrijfsprocessen worden gevolgd?
* Welke normen moet u volgen? Moet u een industriestandaard aannemen en wijzigen (Dublin Core, ISO 19115, PRISM, enz.) of moet de organisatie haar eigen normen opstellen ?
* Waar wordt de strategie gedocumenteerd? Hoe kunt u ervoor zorgen dat alle belanghebbenden toegang hebben? Hoe kunt u ervoor zorgen dat nieuw aangeworven personeel zich aan de overeengekomen norm houdt (bijvoorbeeld een bezoek aan een training voordat u toegang krijgt?)
