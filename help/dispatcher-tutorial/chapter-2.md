---
title: Hoofdstuk 2 - Dispatcher-infrastructuur
description: Begrijp publiceren en verzender topologie. Leer over de gemeenschappelijkste topologieën en de montages.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
doc-type: Tutorial
exl-id: a25b6f74-3686-40a9-a148-4dcafeda032f
duration: 403
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# Hoofdstuk 2 - Infrastructuur

## Een cacheinfrastructuur instellen

Wij introduceerden de basistopologie van een systeem van Publish en een verzender in Hoofdstuk 1 van deze reeks. Een reeks Publish- en Dispatcher-servers kan in veel variaties worden geconfigureerd - afhankelijk van de verwachte belasting, de topologie van uw datacenter(s) en de gewenste failover-eigenschappen.

We zullen de meest gangbare topologieën schetsen en de voordelen beschrijven en waar ze te kort komen. De lijst kan natuurlijk nooit volledig zijn. De enige limiet is je verbeelding.

### De instelling Verouderd

In de begindagen was het aantal potentiële bezoekers klein, was de hardware duur en werden de webservers niet zo kritisch gezien als vandaag de dag. Een algemene instelling zou zijn dat één Dispatcher als taakverdelingsmechanisme en cachegeheugen voor twee of meer Publish-systemen zou fungeren. De Apache-server in de kern van de Dispatcher was zeer stabiel en - in de meeste gevallen - in staat om een behoorlijke hoeveelheid verzoeken te doen.

![&#x200B; &quot;Verouderd&quot;Dispatcher Opstelling - niet zeer algemeen door de normen van vandaag &#x200B;](assets/chapter-2/legacy-dispatcher-setup.png)

*&quot;Verouderd&quot;Dispatcher Opstelling - niet zeer algemeen door de normen van vandaag*

<br> 

Dit is waar de verzender zijn naam van ontving: Het was eigenlijk verzendend verzoeken. Deze configuratie komt niet meer vaak voor, omdat deze niet kan voldoen aan de hogere eisen op het gebied van prestaties en stabiliteit die vandaag de dag worden gesteld.

### Multi-Legged Setup

Vandaag is een lichtjes verschillende topologie gemeenschappelijker. Een topologie met meerdere poten zou één Dispatcher per Publish-server hebben. Een speciaal taakverdelingsmechanisme (hardware) bevindt zich vóór de AEM-infrastructuur en verzendt de aanvragen naar deze twee (of meer) onderdelen:

![&#x200B; Moderne &quot;Standaard&quot;Opstelling van Dispatcher - Gemakkelijk te behandelen en te handhaven &#x200B;](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Moderne &quot;Standaard&quot;Opstelling van Dispatcher - Gemakkelijk te behandelen en te handhaven*

<br> 

Hier zijn de redenen voor dit soort opstelling,

1. Websites bedienen gemiddeld veel meer verkeer dan in het verleden. De &quot;Apache-infrastructuur&quot; moet dus worden uitgebreid.

2. De instelling &#39;Legacy&#39; biedt geen redundantie op Dispatcher-niveau. Als de Apache-server naar beneden ging, was de hele website onbereikbaar.

3. Apache-servers zijn goedkoop. Zij zijn gebaseerd op open bron en, aangezien u een virtueel datacenter hebt, kunnen zij zeer snel worden voorzien.

4. Deze opstelling verstrekt een gemakkelijke manier voor een &quot;het rollen&quot;of &quot;gestaffelde&quot;updatescenario. U sluit gewoon Dispatcher 1 af terwijl u een nieuw softwarepakket installeert op Publish 1. Wanneer de installatie klaar is en u voldoende met rook geteste Publish 1 van het interne netwerk hebt, maakt u de cache op Dispatcher 1 schoon en start u deze opnieuw terwijl u Dispatcher 2 inneemt voor onderhoud van Publish 2.

5. De ongeldigverklaring van het geheime voorgeheugen wordt zeer gemakkelijk en deterministisch in deze opstelling. Aangezien slechts één Publish-systeem op één Dispatcher is aangesloten, is er slechts één Dispatcher die ongeldig kan worden gemaakt. De volgorde en het tijdstip van de ongeldigverklaring zijn onbeduidend.

### De instelling Uitschalen

Apache-servers zijn goedkoop en eenvoudig in te richten. Duw daarom niet dat niveau iets verder uit te breiden. Waarom hebben twee of meer Dispatchers niet vóór elke Publish-server?

![&#x200B; &quot;Schaal uit&quot;Opstelling - heeft sommige toepassingsgebieden maar ook beperkingen en beveats &#x200B;](assets/chapter-2/scale-out-setup.png)

*&quot;Schaal uit&quot;Opstelling - heeft sommige toepassingsgebieden maar ook beperkingen en beveats*

<br> 

Dat kunt u absoluut doen! En er zijn veel geldige toepassingsscenario&#39;s voor die opstelling. Maar er zijn ook enkele beperkingen en complexiteiten die u in overweging moet nemen.

#### Ongeldigmaking

Elk Publish-systeem is verbonden met een groot aantal Dispatchers. Elk systeem moet ongeldig worden gemaakt wanneer de inhoud is gewijzigd.

#### Onderhoud

Het spreekt vanzelf dat de eerste configuratie van de Dispatcher- en Publish-systemen wat complexer is. Maar houd ook in mening dat de inspanning van een &quot;het rollen&quot;versie ook een beetje hoger is. AEM systemen kunnen en moeten worden bijgewerkt tijdens de uitvoering. Maar het is verstandig om dat niet te doen terwijl ze actief verzoeken indienen. Meestal wilt u slechts een deel van de Publish-systemen bijwerken - terwijl de andere nog steeds actief het verkeer bedienen en vervolgens - na het testen - overschakelen naar het andere deel. Als u geluk hebt en u tot lading-stabilisator in uw plaatsingsproces kunt toegang hebben, kunt u het verpletteren aan de servers onbruikbaar maken onder onderhoud hier. Als u zich op een gedeeld taakverdelingsmechanisme zonder directe toegang bevindt, sluit u liever de verzenders van de Publish die u wilt bijwerken. Hoe meer er is, hoe meer je moet sluiten. Als er een groot aantal is en u regelmatig updates plant, wordt een zekere automatisering geadviseerd. Als u geen automatiseringsgereedschappen hebt, is schalen toch een slecht idee.

In een vorig project hebben we een andere truc gebruikt om een Publish-systeem te verwijderen uit taakverdeling zonder directe toegang tot de taakverdelingsfunctie zelf.

Het taakverdelingsmechanisme pingelt gewoonlijk, een bepaalde pagina om te zien of is de server in gebruik. Een triviale keuze is meestal het pingelen van de homepage. Maar als u wilt gebruiken pingelen om lading-verdelingsmechanisme te signaleren niet om verkeer in evenwicht te brengen zou u iets anders kiezen. U maakt een toegewezen sjabloon of servlet die kan worden geconfigureerd om te reageren met `"up"` of `"down"` (in de hoofdtekst of als http-antwoordcode). De reactie van die pagina mag natuurlijk niet in de verzender worden opgeslagen - en wordt dus altijd vers opgehaald van het Publish-systeem. Als u nu het taakverdelingsmechanisme configureert om deze sjabloon of servlet te controleren, kunt u de Publish gemakkelijk laten &quot;doen alsof&quot; het is uitgevallen. Het maakt geen deel uit van de taakverdeling en kan worden bijgewerkt.

#### Wereldwijd distribueren

&quot;Wereldwijde distributie&quot; is een &quot;Scale out&quot;-instelling waarbij u meerdere Dispatchers voor elk Publish-systeem hebt geplaatst. Deze worden nu over de hele wereld gedistribueerd om dichter bij de klant te komen en betere prestaties te bieden. Natuurlijk, in dat scenario hebt u geen centraal ladingsverdelingsmechanisme maar een DNS en geo-IP gebaseerd ladingsverdelingsregeling.

>[!NOTE]
>
>Eigenlijk, bouwt u een soort netwerk van de Distributie van de Inhoud (CDN) met die benadering - zodat zou u moeten overwegen het kopen van een kant-en-klare oplossing CDN in plaats van zelf het bouwen. Het bouwen en onderhouden van een aangepaste CDN is geen eenvoudige taak.

#### Horizontale schaal

Zelfs in een lokaal datacenter heeft een &quot;schaal uit&quot;topologie die veelvoudige Dispatchers voor elk systeem van Publish heeft sommige voordelen. Als u prestatiesknelpunten ziet op de servers Apache toe te schrijven aan hoog verkeer (en een goede cache hit-rate) en u kunt de hardware niet meer schalen (door CPU&#39;s, RAM en snellere schijven toe te voegen), kunt u de prestaties verhogen door Dispatcher toe te voegen. Dit wordt &#39;horizontale schaling&#39; genoemd. Nochtans, heeft dit grenzen - vooral wanneer u vaak verkeer ongeldig maakt. In de volgende sectie wordt het effect beschreven.

#### Beperkingen van de Topologie van de Schaal uit

Het toevoegen van proxyservers zou normaal de prestaties moeten verhogen. Er zijn echter scenario&#39;s waarin het toevoegen van servers de prestaties kan verminderen. Hoe? Neem bijvoorbeeld een nieuwsportal, waar u elke minuut nieuwe artikelen en pagina&#39;s introduceert. Een Dispatcher maakt de validatie ongedaan door &quot;automatisch annuleren&quot;: wanneer een pagina wordt gepubliceerd, worden alle pagina&#39;s in de cache op dezelfde site ongeldig gemaakt. Dit is een nuttige eigenschap - wij behandelden dit in [&#x200B; Hoofdstuk 1 &#x200B;](chapter-1.md) van deze reeks - maar het betekent ook, dat wanneer u frequente veranderingen op uw website hebt u vrij vaak het geheime voorgeheugen ongeldig maakt. Als u slechts één Dispatcher per Publish-exemplaar hebt, activeert de eerste bezoeker die een pagina aanvraagt, een nieuwe caching van die pagina. De tweede bezoeker krijgt reeds de caching versie.

Als u twee Dispatcher hebt, heeft de tweede bezoeker een kans van 50% dat de pagina niet in het voorgeheugen ondergebracht is, en dan zou hij een grotere latentie ervaren wanneer die pagina opnieuw wordt teruggegeven. Door nog meer verzendingen per Publish wordt het nog erger. De Publish-server ontvangt meer belasting omdat de pagina voor elke Dispatcher afzonderlijk opnieuw moet worden weergegeven.

![&#x200B; Verminderde prestaties in een schaal-uit scenario met frequente geheim voorgeheugenflushes.](assets/chapter-2/decreased-performance.png)

*Verminderde prestaties in een schaal-uit scenario met frequente geheim voorgeheugenflushes.*

<br> 

#### Overschaalde problemen verhelpen

U kunt overwegen een centrale gedeelde opslag te gebruiken voor alle Dispatchers of de bestandssystemen van de Apache-servers te synchroniseren om de problemen te verhelpen. We kunnen slechts beperkte ervaring uit de eerste hand opdoen, maar we moeten ons ervan bewust zijn dat dit de complexiteit van het systeem vergroot en een hele nieuwe categorie fouten kan introduceren.

We hebben enkele experimenten met NFS uitgevoerd - maar NFS introduceert enorme prestatieproblemen door het vergrendelen van inhoud. Dit heeft de algehele prestaties verminderd.

**Conclusie** - het delen van een gemeenschappelijk dossiersysteem onder verscheidene verzenders is GEEN geadviseerde benadering.

Als u prestatieproblemen ondervindt, moet u Publish en Dispatcher even groot maken om piekbelasting op de instanties van Publisher te voorkomen. Er is geen gouden regel voor de verhouding Publish/Dispatcher - het hangt sterk af van de verspreiding van de verzoeken en de frequentie van publicaties en invalidaties in cache.

Als u ook over de latentie een bezoekerservaring bezorgd bent, denk na gebruikend een netwerk van de inhoudslevering, geheime voorgeheugen het terugwinnen, preventieve geheim voorgeheugenopwarming, plaatsend een aflossingstijd zoals die in [&#x200B; wordt beschreven Hoofdstuk 1 &#x200B;](chapter-1.md) van deze reeks of verwijs naar sommige geavanceerde ideeën van [&#x200B; Deel 3 &#x200B;](chapter-3.md).

### De instelling &quot;Cross Connected&quot;

Een andere instelling die we nu zien, is de &quot;cross-connected&quot; configuratie: de Publish-instanties hebben geen toegewijde Dispatchers, maar alle Dispatchers zijn verbonden met alle Publish-systemen.

![&#x200B; dwars-verbonden topologie: Verhoogde overtolligheid en meer ingewikkeldheid &#x200B;](assets/chapter-2/cross-connected-setup.png)

<br> 

*dwars-verbonden topologie: Verhoogde overtolligheid en meer ingewikkeldheid.*

Op het eerste gezicht biedt dit meer redundantie voor een relatief klein budget. Wanneer een van de Apache-servers uitvalt, kunt u nog steeds twee Publish-systemen gebruiken om de rendering uit te voeren. Als een van de Publish-systemen vastloopt, hebt u nog steeds twee Dispatchers die de cachebelasting bedienen.

Maar dat heeft wel een prijs.

In de eerste plaats is het erg omslachtig om één been voor onderhoud uit te nemen. Dit is waar dit systeem voor ontworpen is; om veerkrachtiger te zijn en op alle mogelijke manieren operationeel te blijven. We hebben gecompliceerde onderhoudsplannen gezien over de manier waarop hiermee omgegaan kan worden. U moet eerst de Dispatcher 2 opnieuw configureren, waarbij de kruisverbinding wordt verwijderd. Dispatcher 2 wordt opnieuw gestart. Sluit Dispatcher 1 af, werk Publish 1 bij, ... enzovoort. U dient zorgvuldig te overwegen of dat tot meer dan twee benen kan worden geschaald. U zult tot de conclusie komen dat het de complexiteit en de kosten verhoogt en een enorme bron van menselijke fouten is. Het zou beter zijn om dit te automatiseren. Dus beter controleren of je daadwerkelijk over de personele middelen beschikt om deze automatiseringstaak op te nemen in je projectplanning. Terwijl u sommige hardwarekosten met dit zou kunnen besparen, zou u aan het personeel van IT dubbel kunnen uitgeven.

Ten tweede kan het zijn dat er een gebruikerstoepassing op de AEM wordt uitgevoerd waarvoor een aanmelding is vereist. U gebruikt kleverige zittingen, om ervoor te zorgen dat één gebruiker altijd van de zelfde AEM instantie wordt gediend zodat u zittingsstaat op die instantie kon handhaven. Als u deze verbinding hebt, moet u ervoor zorgen dat de plaksessies goed werken op het taakverdelingsmechanisme en op de Dispatcher. Niet onmogelijk - maar u moet zich daarvan bewust zijn en extra configuratie en testuren toevoegen, die - opnieuw - de besparingen zouden kunnen verhogen u door hardware te besparen had gepland.

### Conclusie

Wij adviseren niet dat u dit dwars-verbind regeling als standaardoptie gebruikt. Nochtans, als u besluit om het te gebruiken, zult u risico&#39;s en verborgen kosten zorgvuldig willen beoordelen en van plan zijn om configuratieautomatisering als deel van uw project te omvatten.

## Volgende stap

* [3 - Geavanceerde onderwerpen in cache](chapter-3.md)
