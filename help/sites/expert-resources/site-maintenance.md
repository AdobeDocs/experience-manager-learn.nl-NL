---
title: Handleiding voor routineonderhoud
description: Ongeacht of u een beheerder, auteur of ontwikkelaar bent, siteonderhoud raakt elk aspect van uw AEM Sites-instantie. Gebruik deze handleiding om ervoor te zorgen dat uw strategie is ingesteld voor succes.
role: Admin
level: Beginner, Intermediate
topic: Administration
feature: Learn From Your Peers
jira: KT-14255
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
duration: 209
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 0%

---

# Tips en trucs voor het onderhoud van de site

Er zijn drie opties voor het installeren en onderhouden van een AEM

* AEMaaCS (cloudservice) - het systeem is altijd actief, up-to-date en wordt dynamisch geschaald naar wens
* Adobe Managed Services waar Adobe Customer Service Engineers alle dagelijkse/wekelijkse/maandelijkse onderhoudswerkzaamheden uitvoeren en ervoor zorgen dat alle servicepakketten worden geïnstalleerd en het systeem altijd veilig en probleemloos werkt
* het uitvoeren van het op-gebouw, waar u het volledige systeem, met inbegrip van steunen, verbeteringen, en veiligheid moet behandelen.

Als u ervoor kiest om uw eigen systeem op-gebouw uit te voeren, zijn er een paar dingen in mening om u te verzekeren hebt een veilig, prestatiessysteem. Naast de &quot;zorg en voeding&quot;-artikelen zal in dit artikel ook worden gewezen op verschillende punten AEM Ontwikkelaars zouden in gedachten moeten houden om het systeem goed te laten functioneren.

## Admins

Back-ups - zorg ervoor dat u regelmatig volledige en/of gedeeltelijke back-ups hebt:

* dagelijks
* wekelijks
* maandelijks

Veel klanten maken back-ups van momentopnamen, die slechts enkele minuten duren, ervan uitgaande dat het onderliggende besturingssysteem dergelijke back-ups ondersteunt. Zorg ervoor dat deze back-ups op de juiste wijze zijn opgeslagen (van het AEM systeem). Zorg ervoor dat de back-ups functioneel zijn en kunnen worden gebruikt om een werkend systeem regelmatig opnieuw te maken. Er is niets erger dan het laten vastlopen van het systeem en uw back-ups zijn om de een of andere reden beschadigd!

Er zijn verscheidene punten u moet controleren om probleem-vrije verrichting te verzekeren:

### Routine Maintenance

#### [ indexonderhoud ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=en)

Met indexen kunnen query&#39;s zo snel mogelijk worden uitgevoerd, waardoor bronnen voor andere bewerkingen vrijkomen. Zorg ervoor dat de indexen de top-top vorm hebben! AEM annuleert vragen die in plaats van het gebruiken van een index reizen om één slechte vraag te houden van het beïnvloeden van algemene AEM prestaties.

#### [ Opschonen van de Compactie/van de Revisie van de Tar ](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Bij elke update van de opslagplaats wordt een nieuwe inhoudsrevisie gemaakt. Als gevolg hiervan neemt de grootte van de gegevensopslagruimte bij elke update toe. Om ongecontroleerde groei van opslagplaatsen te voorkomen, moeten oude revisies worden opgeschoond tot vrije schijfmiddelen.

#### [ Lucene binaries schoonmaakbeurt ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

Wis lucene binaire binaire getallen en verlaag de vereiste opslaggrootte voor de actieve gegevens.

#### [ de Afval van de Opslag van Gegevens ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

Wanneer een middel in AEM wordt geschrapt, kan de verwijzing naar het onderliggende verslag van de gegevensopslag uit de knoophiërarchie worden verwijderd, maar het verslag van de gegevensopslag zelf blijft. Deze gegevensopslagrecord zonder referenties wordt &#39;garbage&#39; die niet hoeft te worden bewaard. In gevallen waarin een aantal middelen zonder referenties bestaat, is het nuttig deze te verwijderen, ruimte te behouden, back-up en onderhoudsprestaties van het bestandssysteem te optimaliseren.

#### [ Wissen van het Werkschema ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html)

Door het minimaliseren van het aantal workflowexemplaren worden de prestaties van de workflow-engine verbeterd, zodat u regelmatig voltooide of actieve workflowexemplaren uit de repository kunt verwijderen.

#### [ het Onderhoud van het Logboek van de Controle ] (https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

AEM gebeurtenissen die in aanmerking komen voor accountlogboekregistratie, genereren veel gearchiveerde gegevens. Deze gegevens kunnen na verloop van tijd snel groeien als gevolg van replicaties, het uploaden van bedrijfsmiddelen en andere systeemactiviteiten.

#### [ Veiligheid ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en)

Zorg ervoor dat de best practices van de lijst met beveiligingscontroles nauwlettend worden gevolgd om ervoor te zorgen dat de AEM het veiligst is.

#### Diskspace

Monitor diskspace om er zeker van te zijn dat u voldoende hebt voor de JCR Repository, plus ongeveer de helft meer ruimte - de teercompactie gebruikt extra ruimte wanneer deze wordt uitgevoerd. Het wegvallen van de schijfruimte is de belangrijkste oorzaak van JCR-corruptie!

## Developer

Probeer om douanecomponenten niet te gebruiken - gebruik [ kerncomponenten ](https://www.aemcomponents.dev/). U moet kerncomponenten 80-90% van de tijd en aangepaste componenten slechts spaarzaam gebruiken. Dit vereist vaak een nieuwe manier om naar de componenten op een pagina te kijken - u moet zich realiseren dat de componenten gemakkelijk door een front-end ontwikkelaar kunnen worden hervormd gebruikend CSS. Houd er ook rekening mee dat deze kerncomponenten in elkaar kunnen worden ingesloten om vrij complexe resultaten te bereiken. Creatief!

### [ Systemen van de Stijl ](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

Met stijlsystemen kunnen de basiscomponenten, en zelfs aangepaste componenten, naar eigen goeddunken naar wens van de auteur hun uiterlijk veranderen om geheel nieuwe, kijkende componenten te maken. Deze stilistische veranderingen impliceren over het algemeen slechts een front-end ontwerper en een goed geïnformeerde auteur (die vaak naar een &quot;Super Auteur&quot;wordt verwezen

### [ Lanceringen ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

Met Starten kan het werk worden voltooid voor een nieuwe promotie-, verkoop- of websiteuitrol zonder dat dit gevolgen heeft voor de momenteel geïmplementeerde pagina&#39;s. Bovendien kunnen ze automatisch, zonder aanwezigheid of toezicht, live gaan, zodat auteurs nu hun werk van volgende week (of volgend kwartaal) kunnen doen en niet de dag voor het leven op de pagina komen - het is echt de gave van TIME!)

### [Contentfragmenten](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

Inhoudsfragmenten zijn aanpasbare &quot;brokken&quot; met informatie die gemakkelijk overal op de site opnieuw kunnen worden gebruikt. Als u een wijziging nodig hebt, wijzigt u gewoon het oorspronkelijke segment en de update wordt overal weergegeven waar deze wordt gebruikt - onmiddellijk!

### [Ervaringsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

Hoewel u bijna hetzelfde klinkt als Content Fragments, zijn Experience Fragments kleine, zichtbare stukken van een pagina. Deze kunnen ook op grote schaal op uw site worden hergebruikt en binnen AEM op een centrale locatie worden onderhouden om het maken van mogelijk algemene wijzigingen op uw site in seconden, niet dagen of weken, te vergemakkelijken.

Denk vooruit en bekijk wat opnieuw gebruikt kan worden. Voettekst? Een disclaimer? Een koptekst? Bepaalde soorten inhoud? Al deze bestanden kunnen op een hele site worden gedeeld, maar het onderhoud moet tot een minimum worden beperkt. Wilt u een datum in een disclaimer bijwerken, maar deze staat op 1000 pagina&#39;s op uw site? Het is 5 seconden als je een Experience Fragment gebruikt!

## Algemeen

Blijf op de hoogte van veranderingen AEM door voortdurend leren - blijf niet vastzitten in het verleden. Het Experience League van het gebruik ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) en [ Digitale het Leren van de Adobe de Diensten (ADLS) ](https://learning.adobe.com/) om uw vaardigheden te kweken.[

## Conclusie

AEM kan een groot systeem zijn, en het kost veel mensen om het &quot;zingen&quot; te maken. Van Admins aan Ontwikkelaars (zowel front-end als hardcore Java-ontwikkelaars) aan Auteurs - er is iets voor iedereen! En als je je niet voelt om het dagelijks bestuur te beheren, zijn er altijd AMS en AEM as a Cloud Service.
