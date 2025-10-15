---
title: Veelgestelde vragen over migratie van AEM as a Cloud Service-inhoud
description: Antwoorden op veelgestelde vragen over de migratie van inhoud naar AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
doc-type: article
topic: Migration
feature: Migration
role: Architect, Developer
level: Beginner
jira: KT-11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
duration: 399
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1884'
ht-degree: 0%

---

# Veelgestelde vragen over migratie van AEM as a Cloud Service-inhoud

Antwoorden op veelgestelde vragen over de migratie van inhoud naar AEM as a Cloud Service.

## Terminologie

+ **AEMaaCS**: [&#x200B; AEM as a Cloud Service &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html?lang=nl-NL)
+ **BPA**: [&#x200B; Analysator van Beste praktijken &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html?lang=nl-NL)
+ **CTT**: [&#x200B; Het Hulpmiddel van de Overdracht van de Inhoud &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=nl-NL)
+ **CAM**: [&#x200B; Cloud Acceleration Manager &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=nl-NL)
+ **IMS**: [&#x200B; Systeem van Identity Management &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html?lang=nl-NL)
+ **DM**: [&#x200B; Dynamische Media &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html?lang=nl-NL)

Gebruik de onderstaande sjabloon voor meer informatie wanneer u CTT-gerelateerde Adobe-ondersteuningstickets maakt.

![&#x200B; de Malplaatje van het Ticket van de Steun van Adobe van de Migratie van de Inhoud &#x200B;](../../assets/faq/adobe-support-ticket-template.png) { align= &quot;centrum&quot;}

## Algemene vragen over migratie van inhoud

### V: Wat zijn de verschillende methoden om inhoud als Cloud Services naar AEM te migreren?

Er zijn drie verschillende methoden beschikbaar

+ Het gereedschap Inhoud overbrengen gebruiken (AEM 6.3+ → AEMaaCS)
+ Via Package Manager (AEM → AEMaaCS)
+ Buiten de box Bulk Import Service voor Assets (S3/Azure → AEMaaCS)

### Q: Is er een grens op de hoeveelheid inhoud die kan worden overgebracht gebruikend CTT?

Nee. CTT als een gereedschap kan uit AEM-bron worden geëxtraheerd en in AEMaaCS worden opgenomen. Er zijn echter specifieke limieten voor het AEMaaCS-platform die vóór de migratie in overweging moeten worden genomen.

Voor meer info, verwijs naar [&#x200B; de eerste vereisten van de wolkenmigratie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html?lang=nl-NL).

### V: Ik heb het laatste BPA-rapport van mijn bronsysteem, wat moet ik ermee doen?

Exporteer het rapport als CSV en upload het vervolgens naar Cloud Acceleration Manager, [&#x200B; gekoppeld aan uw IMS-organisatie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=nl-NL) . Dan ga door het overzichtsproces zoals [&#x200B; die in de Fase van de Bereidheid &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/cam-readiness-phase.html?lang=nl-NL) wordt geschetst.

Controleer de evaluatie van de complexiteit van de code en inhoud die door het gereedschap wordt geleverd en noteer de bijbehorende actiepunten die leiden tot de evaluatie van de achtergrondcode voor het vernieuwen van code of de cloudmigratie.

### Q: Is het geadviseerd om op bronauteur te halen en in auteur AEMaaCS op te nemen en te publiceren?

Het wordt altijd aangeraden een-op-een-uitname uit te voeren tussen auteur- en publicatielagen. Desalniettemin is het acceptabel om de auteur van de bronproductie uit te pakken en die in Dev, Stage en Production CS in te voeren.

### Q: Is er een manier om de tijd te schatten, het neemt om de inhoud van bron AEM in AEMaaCS te migreren gebruikend CTT?

Aangezien het migratieproces afhankelijk is van internetbandbreedte, voor het CTT-proces toegewezen heap, beschikbaar vrij geheugen en schijf-IO die aan elk bronsysteem zijn onderworpen, wordt aanbevolen om het bewijs van migratie in een vroeg stadium uit te voeren en die gegevenspunten te extrapoleren om schattingen te maken.

### V: Hoe wordt mijn AEM-bronprestatie beïnvloed als ik het CTT-extractieproces start?

Het hulpmiddel CTT loopt in zijn eigen proces Java™ dat tot 4gb heap neemt, die door configuratie OSGi configureerbaar is. Dit getal kan veranderen, maar u kunt het Java™-proces opzoeken en daar achter komen.

Als AZCopy is geïnstalleerd en/of optie / validatiefunctie voor kopiëren is ingeschakeld, verbruikt AZCopy-proces CPU-cycli.

Naast jvm gebruikt het gereedschap ook schijf-IO om de gegevens op te slaan op een tijdelijke tijdelijke ruimte en die na de extractiecyclus worden opgeschoond. Naast RAM, CPU en Schijf IO, gebruikt het hulpmiddel CTT ook de netwerkbandbreedte van het bronsysteem om gegevens in Azure blob store te uploaden.

De hoeveelheid middelen die het CTT-extractieproces nodig heeft, is afhankelijk van het aantal knooppunten, het aantal lobs en de geaggregeerde grootte ervan. Het is moeilijk om een formule te verstrekken en daarom wordt het geadviseerd om een klein Bewijs van migratie uit te voeren om de vereisten van de bronserver te bepalen upsize.

Als kloonomgevingen worden gebruikt voor migratie, heeft dit geen invloed op het gebruik van bronnen voor live productieservers, maar heeft het zijn eigen nadelen voor het synchroniseren van inhoud tussen live productie en kloon

### Q: Wat betekenen de termen &quot;veeggen&quot; en &quot;overschrijven&quot; in de context van CTT?

In de context van [&#x200B; extractiefase &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=nl-NL#extraction-setup-phase), zijn de opties of om de gegevens in de het opvoeren container van vorige extractiecycli te beschrijven of het verschil (toegevoegd/bijgewerkt/geschrapt) in het toe te voegen. De container van het opvoeren is niets, maar de blob opslagcontainer verbonden aan migratiereeks. Elke migratieset krijgt een eigen staging container.

In de context van [&#x200B; innamefase &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/ingesting-content.html?lang=nl-NL), zijn de opties + om de volledige inhoudsbewaarplaats van AEMaaCS te vervangen of de differentiële (toegevoegde/bijgewerkte/geschrapte) inhoud van de opvoerende migratiecontainer te synchroniseren.

### Q: Er zijn veelvoudige websites, bijbehorende activa, gebruikers, groepen in het bronsysteem. Is het mogelijk om ze in fasen te migreren naar AEMaaCS?

Ja, dat is mogelijk, maar er is een zorgvuldige planning nodig met betrekking tot:

+ Het creëren van de migratiesets veronderstellend de plaatsen, zijn de activa in hun respectieve hiërarchieën
   + Verifieer of het aanvaardbaar is om alle activa als deel van één migratiereeks te migreren en dan plaatsen te brengen die hen in fasen gebruiken
+ In de huidige staat maakt het auteursinvoerproces de auteursinstantie voor inhoudcreatie onbeschikbaar alhoewel de publicatielaag nog de inhoud kan dienen
   + Dit betekent dat de activiteiten voor het schrijven van inhoud worden bevroren totdat de opname is voltooid
+ De gebruikers worden niet meer gemigreerd, hoewel de groepen.

Controleer het extractie- en innameproces voor de top-up zoals beschreven voordat u de migraties plant.

### V: Zijn mijn websites beschikbaar voor eindgebruikers, ook al gebeurt er inname in de auteur van AEMaaCS of in de publicatie?

Ja. Eindgebruikersverkeer wordt niet onderbroken door de activiteit van de inhoudsmigratie. De opname van de auteur bevriest de inhoud echter totdat deze is voltooid.

### Q: Het BPA rapport toont punten met betrekking tot ontbrekende originele vertoningen. Moet ik ze eerst opruimen aan de bron voordat ze worden geëxtraheerd?

Ja. De ontbrekende oorspronkelijke vertoning betekent dat het binaire element niet correct is geüpload. Beschouw het als slechte gegevens, bekijk, maak een reservekopie met gebruik van Package Manager (zoals vereist) en verwijder ze uit bron-AEM voordat u extractie uitvoert. De onjuiste gegevens hebben negatieve resultaten bij de verwerking van de elementen.

### Q: Het BPA-rapport bevat items die verwant zijn aan het ontbrekende `jcr:content` knooppunt voor mappen. Wat moet ik met hen doen?

Als `jcr:content` ontbreekt op mapniveau, moet u een actie uitvoeren om instellingen zoals verwerkingsprofielen, enz. door te geven. de ouders zullen op dit niveau breken . Controleer de reden waarom `jcr:content` ontbreekt. Hoewel deze mappen kunnen worden gemigreerd, wordt de gebruikerservaring door deze mappen afgebroken en worden later onnodige cycli voor probleemoplossing veroorzaakt.

### V: Ik heb een migratieset gemaakt. is het mogelijk de omvang ervan te controleren ?

Ja, is er de eigenschap van de Grootte van de Controle van de a [&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=nl-NL#migration-set-size) die deel van CTT uitmaakt.

### Q: Ik voer de migratie uit (extractie, inname). Kan worden gecontroleerd of al mijn geëxtraheerde inhoud in het doel is opgenomen?

Ja, is er a [&#x200B; bevestiging &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html?lang=nl-NL) eigenschap die deel van CTT uitmaakt.

### Q: Mijn klant heeft de vereiste om inhoud te verplaatsen tussen AEMaaCS-omgevingen, zoals van AEMaaCS Dev naar AEMaaCS Stage of naar AEMaaCS Prod. Kan ik het hulpmiddel van de inhoudoverdracht voor deze gebruiksgevallen gebruiken?

Helaas, nee. De gebruikscase van CTT is het migreren van inhoud van Op-gebouw/AMS-ontvangen bron van AEM 6.3+ aan AEMaaCS wolmilieu&#39;s. [&#x200B; gelieve te lezen CTT documentatie &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=nl-NL).

### V: Welke problemen worden verwacht tijdens de extractie?

De Fase van de extractie is een betrokken proces dat veelvoudige aspecten vereist om te werken zoals verwacht. Als u zich bewust bent van verschillende soorten problemen die zich kunnen voordoen en hoe u deze kunt beperken, wordt de migratie van inhoud over het algemeen succesvoller.

De openbare documentatie wordt voortdurend verbeterd op basis van de lessen, maar er zijn enkele probleemcategorieën op hoog niveau en mogelijke onderliggende redenen.

![&#128279;](../../assets/faq/extraction-issues.jpg) { align= &quot;center&quot;} de kwesties van de de inhoudsmigratie van AEM as a Cloud Service

### V: Welke problemen worden verwacht tijdens inname?

De inbraakfase komt volledig voor in het wolkenplatform en vereist hulp van de middelen die toegang tot infrastructuur AEMaaCS hebben. Maak een ondersteuningsticket voor meer hulp.

Hier zijn mogelijke categorieën van kwesties (gelieve te beschouwen dit niet als een exclusieve lijst)

![&#x200B; de kwesties van de de inhoudsmigratie van AEM as a Cloud Service &#x200B;](../../assets/faq/ingestion-issues.jpg) { align= &quot;centrum&quot;}



### V: Moet mijn bronserver een uitgaande internetverbinding hebben om CTT te kunnen gebruiken?

Het korte antwoord is &quot;**ja**&quot;.

Het proces CTT vereist connectiviteit aan de hieronder middelen:

+ De AEM as a Cloud Service-doelomgeving: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ De Azure-opslagservice: `casstorageprod.blob.core.windows.net`

Verwijs naar de documentatie voor meer informatie over [&#x200B; bronconnectiviteit &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=nl-NL#source-environment-connectivity).

## Middelverwerking Dynamische vragen over media

### V: Zullen middelen automatisch worden herverwerkt na opname in AEMaaCS?

Nee. Om de activa te verwerken, moet het verzoek om opnieuw te verwerken in werking worden gesteld.

### V: Zullen activa automatisch opnieuw worden gedesdexeerd na opname in AEMaaCS?

Ja. De elementen worden opnieuw geïndexeerd op basis van de indexdefinities die beschikbaar zijn in AEMaaCS.

### Q: De bron-AEM is geïntegreerd met Dynamic Media. Zijn er specifieke zaken die in overweging moeten worden genomen vóór de migratie van inhoud?

Ja, overweeg het volgende wanneer de bron AEM Dynamic Media Integration heeft.

+ AEMaaCS steunt slechts Dynamische Wijze Scene7 van Media. Als het bronsysteem op Hybride Wijze is, dan wordt de migratie DM aan wijzen Scene7 vereist.
+ Als de benadering uit bronklooninstanties moet migreren, dan is het veilig om integratie DM op kloon onbruikbaar te maken die voor CTT zou worden gebruikt. Deze stap is puur om het even welk schrijven aan DM te vermijden of lading op verkeer te vermijden DM.
+ Houd er rekening mee dat CTT knooppunten, metagegevens van een migratieset van AEM naar AEMaaCS migreert. Het zal geen handelingen op DM direct uitvoeren.

### V: Wat zijn verschillende migratiemethoden wanneer DM-integratie aanwezig is op Source AEM?

Lees de bovenstaande vraag en beantwoord deze eerder

(Dit zijn twee mogelijke opties, maar zijn niet beperkt tot deze twee). Het hangt van hoe de klant de UAT, Prestaties het testen, het beschikbare milieu wil benaderen en of een kloon voor migratie of niet wordt gebruikt. Beschouw deze twee als uitgangspunt voor de discussie

**Optie 1**

Als het aantal elementen/knooppunten in de bronomgeving zich op een lager niveau (~100K) bevindt, ervan uitgaande dat deze over een periode van 24 + 72 uur kunnen worden gemigreerd, inclusief extractie en opname, is de beste aanpak:

+ Migratie van productie rechtstreeks uitvoeren
+ Een eerste extractie en opname uitvoeren in AEMaaCS met `wipe=true`
   + Deze stap migreert alle knopen en binaire getallen
+ Doorgaan met werken aan on-premise / AMS Prod-auteur
+ Voer vanaf nu alle andere migratiecycli uit met `wipe=true`
   + Merk op deze verrichting migreert volledige knooppuntenopslag, maar slechts gewijzigde vlekken in tegenstelling tot volledige vlekken. De vorige set lobs is te vinden in de Azure blob store van de AEMaaCS-doelinstantie.
   + Gebruik dit bewijs van migraties voor het meten van de migratieduur, het testen en valideren van alle andere functies
+ Tot slot, vóór de week van go-live, voer vepe=ware migratie uit
   + De dynamische media aansluiten op AEMaaCS
   + DM config van AEM op-gebouwbron losmaken

Met deze optie kunt u migratie één voor één uitvoeren, wat On-prem Dev → AEMaaCS Dev, etc. betekent. en verplaatst de DM-configuraties vanuit de respectieve omgevingen

(In het geval dat de migratie gepland is voor uitvoering vanuit kloon)

**Optie 2**

+ Creeer kloon van de auteur van de Productie, verwijder configuratie DM uit Kloon
+ Migreren van kloon op locatie → AEMaaCS Dev / Stage
   + Het productie-DM-bedrijf voor validatiedoeleinden kort verbinden met AEMaaCS Dev/Stage
   + Zorg ervoor dat tijdens de DM-verbinding geen elementen in AEMaaCS worden opgenomen
   + Dit laat hen toe om CTT, DM specifieke bevestigingen te bevestigen
+ Zodra het testen op AEMaaCS volledig is
   + Een veegmigratie uitvoeren van werkgebied op locatie naar AEMaaCS Stage

Voer een veegmigratie uit van on-premise Dev naar AEMaaCS Dev.

De bovenstaande aanpak kan alleen worden gebruikt voor het meten van de migratieduur, maar moet later worden opgeschoond.

## Aanvullende bronnen

+ [&#x200B; Uiteinden en Tricks voor het Migreren aan Experience Manager in de Wolk (Top 2022) &#x200B;](https://business.adobe.com/nl/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [&#x200B; CTT de Video van de Reeks van de Deskundige &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html?lang=nl-NL)

+ [&#x200B; de Video&#39;s van de Reeks van de Deskundige op andere onderwerpen AEMaaCS &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/aem-experts-series.html?lang=nl-NL)
