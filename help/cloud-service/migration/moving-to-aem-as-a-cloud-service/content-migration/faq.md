---
title: Veelgestelde vragen over migratie van as a Cloud Service inhoud AEM
description: Hiermee krijgt u antwoorden op veelgestelde vragen over de migratie van inhoud naar AEM as a Cloud Service.
version: Cloud Service
doc-type: article
topic: Migration
feature: Migration
role: Architect, Developer
level: Beginner
jira: KT-11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2296'
ht-degree: 0%

---

# Veelgestelde vragen over migratie van as a Cloud Service inhoud AEM

Hiermee krijgt u antwoorden op veelgestelde vragen over de migratie van inhoud naar AEM as a Cloud Service.

## Terminologie

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**: [Analysator van best practices](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [Inhoud overbrengen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Identity Management-systeem](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

Gebruik de onderstaande sjabloon voor het opgeven van meer informatie tijdens het maken van CTT-gerelateerde ondersteuningstickets voor Adoben.

![Ondersteuningstabel voor Adobe voor inhoudsmigratie](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## Algemene vragen over migratie van inhoud

### Q: Wat zijn de verschillende methodes om inhoud in AEM als Cloud Servicen te migreren?

Er zijn drie verschillende methoden beschikbaar

+ Het gereedschap Inhoud overbrengen gebruiken (AEM 6.3+ → AEMaaCS)
+ Via pakketbeheer (AEM → AEMaaCS)
+ Buiten de box Bulk Import Service for Assets (S3/Azure → AEMaaCS)

### Q: Is er een grens op de hoeveelheid inhoud die kan worden overgebracht gebruikend CTT?

Nee. CTT als gereedschap kan uit AEM bron worden gehaald en in AEMaaCS worden opgenomen. Er zijn echter specifieke limieten voor het AEMaaCS-platform die vóór de migratie in overweging moeten worden genomen.

Zie voor meer informatie [vereisten voor cloudmigratie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### V: Ik heb het laatste BPA-rapport van mijn bronsysteem, wat moet ik ermee doen?

Exporteer het rapport als CSV en upload het vervolgens naar Cloud Acceleration Manager. [gekoppeld aan uw IMS-organisatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html). Doorloop vervolgens het revisieproces als [beschreven in de gereedheidsfase](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

Controleer de evaluatie van de complexiteit van de code en inhoud die door het gereedschap wordt geleverd en noteer de bijbehorende actiepunten die leiden tot de evaluatie van de achtergrondcode voor het vernieuwen van code of de cloudmigratie.

### Q: Is het geadviseerd om op bronauteur te halen en in auteur AEMaaCS op te nemen en te publiceren?

Het wordt altijd aangeraden een-op-een-uitname uit te voeren tussen auteur- en publicatielagen. Desalniettemin is het acceptabel om de auteur van de bronproductie uit te pakken en die in Dev, Stage en Production CS in te voeren.

### Q: Is er een manier om de tijd te schatten, het vergt om de inhoud van bron AEM in AEMaaCS gebruikend CTT te migreren?

Aangezien het migratieproces afhankelijk is van internetbandbreedte, voor het CTT-proces toegewezen heap, beschikbaar vrij geheugen en schijf-IO die aan elk bronsysteem zijn onderworpen, wordt aanbevolen om het bewijs van migratie in een vroeg stadium uit te voeren en die gegevenspunten te extrapoleren om schattingen te maken.

### Q: Hoe wordt mijn bron AEM prestaties beïnvloed als ik het CTT-extractieproces start?

Het hulpmiddel CTT loopt in zijn eigen proces Java™ dat tot 4gb heap neemt, die door configuratie OSGi configureerbaar is. Dit getal kan veranderen, maar u kunt het Java™-proces opzoeken en daar achter komen.

Als AZCopy is geïnstalleerd en/of optie / validatiefunctie voor kopiëren is ingeschakeld, verbruikt het AZCopy-proces CPU-cycli.

Naast jvm gebruikt het gereedschap ook schijf-IO om de gegevens op te slaan op een tijdelijke tijdelijke ruimte en die na de extractiecyclus worden opgeschoond. Naast RAM, cpu en Schijf IO, gebruikt het hulpmiddel CTT ook de netwerkbandbreedte van het bronsysteem om gegevens in Azure blob store te uploaden.

De hoeveelheid middelen die het CTT-extractieproces nodig heeft, is afhankelijk van het aantal knooppunten, het aantal lobs en de geaggregeerde grootte ervan. Het is moeilijk om een formule te verstrekken en daarom wordt het geadviseerd om een klein Bewijs van migratie uit te voeren om de vereisten van de bronserver te bepalen upsize.

Als kloonomgevingen worden gebruikt voor migratie, heeft dit geen invloed op het gebruik van bronnen voor live productieservers, maar heeft het zijn eigen nadelen voor het synchroniseren van inhoud tussen live productie en kloon

### Q: In mijn systeem van de bronauteur, hebben wij SSO voor de gebruikers wordt gevormd om in de instantie van de Auteur voor authentiek te verklaren. Moet ik de eigenschap van de Toewijzing van de Gebruiker van CTT in dit geval gebruiken?

Het korte antwoord is: &quot;**Ja**&quot;.

De CTT-extractie en -inname **zonder** Door de gebruiker alleen de inhoud toe te wijzen, worden de bijbehorende beginselen (gebruikers, groepen) van bron AEM naar AEMaaCS gemigreerd. Maar deze gebruikers (id&#39;s) in Adobe IMS moeten toegang hebben (voorzien van) tot AEMaaCS-instantie om de verificatie te voltooien. De taak van [gereedschap voor gebruikerstoewijzing](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/legacy-user-mapping-tool/overview-user-mapping-tool-legacy.html) moet de lokale AEM gebruiker aan Gebruiker IMS in kaart brengen zodat de authentificatie en de vergunningen samenwerken.

In dit geval is de SAML-identiteitsprovider geconfigureerd tegen Adobe IMS voor het gebruik van Federated / Enterprise ID in plaats van rechtstreeks voor het AEM met behulp van de verificatiehandler.

### Q: In mijn systeem van de bronauteur, hebben wij basisauthentificatie die voor de gebruikers wordt gevormd om in de instantie van de Auteur met lokale AEM gebruikers voor authentiek te verklaren. Moet ik de eigenschap van de Toewijzing van de Gebruiker van CTT in dit geval gebruiken?

Het korte antwoord is: &quot;**Ja**&quot;.

De CTT-extractie en -opname zonder gebruikerstoewijzing migreert de inhoud, de bijbehorende beginselen (gebruikers, groepen) van bron AEM naar AEMaaCS. Maar deze gebruikers (id&#39;s) in Adobe IMS moeten toegang hebben (voorzien van) tot AEMaaCS-instantie om de verificatie te voltooien. De taak van [gereedschap voor gebruikerstoewijzing](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/legacy-user-mapping-tool/overview-user-mapping-tool-legacy.html) moet de lokale AEM gebruiker aan Gebruiker IMS in kaart brengen zodat de authentificatie en de vergunningen samenwerken.

In dit geval gebruiken de gebruikers persoonlijke Adobe ID en wordt de Adobe ID door IMS-beheerder gebruikt om toegang te verlenen tot AEMaaCS.

### Q: Wat betekenen de termen &quot;veeggen&quot; en &quot;overschrijven&quot; in de context van CTT?

In de context van [extractiefase](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=en#extraction-setup-phase)De opties zijn of om de gegevens in de testcontainer van vorige extractiecycli te overschrijven of het differentieel (toegevoegd/bijgewerkt/verwijderd) in de container toe te voegen. De container van het opvoeren is niets, maar de blob opslagcontainer verbonden aan migratiereeks. Elke migratieset krijgt een eigen staging container.

In de context van [innamefase](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/ingesting-content.html), De opties zijn + waarmee de volledige inhoudsopslagplaats van AEMaaCS wordt vervangen of de differentiële (toegevoegde/bijgewerkte/verwijderde) inhoud van de testmigratiecontainer wordt gesynchroniseerd.

### Q: Er zijn veelvoudige websites, bijbehorende activa, gebruikers, groepen in het bronsysteem. Is het mogelijk om ze in fasen te migreren naar AEMaaCS?

Ja, dat is mogelijk, maar er is een zorgvuldige planning nodig met betrekking tot:

+ Het creëren van de migratiesets veronderstellend de plaatsen, zijn de activa in hun respectieve hiërarchieën
   + Verifieer of het aanvaardbaar is om alle activa als deel van één migratiereeks te migreren en dan plaatsen te brengen die hen in fasen gebruiken
+ In de huidige staat maakt het auteursinvoerproces de auteursinstantie voor inhoudcreatie onbeschikbaar alhoewel de publicatielaag nog de inhoud kan dienen
   + Dit betekent dat de activiteiten voor het schrijven van inhoud worden bevroren totdat de opname is voltooid

Controleer het extractie- en innameproces voor de top-up zoals beschreven voordat u de migraties plant.

### V: Zijn mijn websites beschikbaar voor eindgebruikers, ook al gebeurt er inname in de auteur van AEMaaCS of in de publicatie?

Ja. Eindgebruikersverkeer wordt niet onderbroken door de activiteit van de inhoudsmigratie. De opname van de auteur bevriest de inhoud echter totdat deze is voltooid.

### Q: Het BPA rapport toont punten met betrekking tot ontbrekende originele vertoningen. Moet ik ze eerst opruimen aan de bron voordat ze worden geëxtraheerd?

Ja. De ontbrekende oorspronkelijke vertoning betekent dat het binaire element niet correct is geüpload. Beschouw het als slechte gegevens, gelieve te herzien, steun gebruikend de Manager van het Pakket (zoals vereist) en hen te verwijderen uit bron AEM alvorens extractie in werking te stellen. De onjuiste gegevens hebben negatieve resultaten bij de verwerking van de elementen.

### Q: Het BPA-rapport bevat items die verband houden met het ontbreken `jcr:content` knooppunt voor mappen. Wat moet ik met hen doen?

Wanneer `jcr:content` ontbreekt op mapniveau, elke actie voor het doorgeven van instellingen zoals verwerkingsprofielen, enz. de ouders zullen op dit niveau breken . Controleer de reden voor het ontbreken `jcr:content`. Hoewel deze mappen kunnen worden gemigreerd, wordt de gebruikerservaring door deze mappen afgebroken en worden later onnodige cycli voor probleemoplossing veroorzaakt.

### V: Ik heb een migratieset gemaakt. is het mogelijk de omvang ervan te controleren ?

Ja, er is een [Formaat controleren](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) onderdeel van de CTT.

### Q: Ik voer de migratie uit (extractie, inname). Kan worden gecontroleerd of al mijn geëxtraheerde inhoud in het doel is opgenomen?

Ja, er is een [validatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) onderdeel van de CTT.

### Q: Mijn klant heeft de vereiste om inhoud te verplaatsen tussen AEMaaCS-omgevingen, zoals van AEMaaCS Dev naar AEMaaCS Stage of naar AEMaaCS Prod. Kan ik het hulpmiddel van de inhoudoverdracht voor deze gebruiksgevallen gebruiken?

Helaas, nee. De gebruikscase van CTT is om inhoud van op-gebouw/AMS-ontvangen bron AEM 6.3+ aan AEMaaCS wolkenmilieu&#39;s te migreren. [Lees de CTT-documentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### V: Welke problemen worden verwacht tijdens de extractie?

De Fase van de extractie is een betrokken proces dat veelvoudige aspecten vereist om te werken zoals verwacht. Als u zich bewust bent van verschillende soorten problemen die zich kunnen voordoen en hoe u deze kunt beperken, wordt de migratie van inhoud over het algemeen succesvoller.

De openbare documentatie wordt voortdurend verbeterd op basis van de lessen, maar er zijn enkele probleemcategorieën op hoog niveau en mogelijke onderliggende redenen.

![Problemen met het uitpakken van as a Cloud Service content AEM](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### V: Welke problemen worden verwacht tijdens inname?

De inbraakfase komt volledig voor in het wolkenplatform en vereist hulp van de middelen die toegang tot infrastructuur AEMaaCS hebben. Maak een ondersteuningsticket voor meer hulp.

Hier zijn mogelijke categorieën van kwesties (gelieve te beschouwen dit niet als een exclusieve lijst)

![Problemen met het opnemen van as a Cloud Service content AEM](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### V: Moet mijn bronserver een uitgaande internetverbinding hebben om CTT te kunnen gebruiken?

Het korte antwoord is: &quot;**Ja**&quot;.

Het proces CTT vereist connectiviteit aan de hieronder middelen:

+ Doel AEM as a Cloud Service omgeving: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ De Azure-opslagservice: `casstorageprod.blob.core.windows.net`
+ Het eindpunt van de Toewijzing van de Gebruiker IO: `usermanagement.adobe.io`

Raadpleeg de documentatie voor meer informatie over [bronconnectiviteit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## Dynamic Media-gerelateerde vragen over verwerking van bedrijfsmiddelen

### V: Zullen middelen automatisch worden herverwerkt na opname in AEMaaCS?

Nee. Om de activa te verwerken, moet het verzoek om opnieuw te verwerken in werking worden gesteld.

### V: Zullen activa automatisch opnieuw worden gedesdexeerd na opname in AEMaaCS?

Ja. De elementen worden opnieuw geïndexeerd op basis van de indexdefinities die beschikbaar zijn in AEMaaCS.

### Q: De bron AEM is geïntegreerd met Dynamic Media. Zijn er specifieke zaken die in overweging moeten worden genomen vóór de migratie van inhoud?

Ja, bedenk het volgende wanneer de bron AEM Dynamic Media Integration heeft.

+ AEMaaCS ondersteunt alleen de Dynamic Media Scene7-modus. Als het bronsysteem op Hybride Wijze is, dan wordt de migratie DM aan de wijzen van Scene7 vereist.
+ Als de benadering uit bronklooninstanties moet migreren, dan is het veilig om integratie DM op kloon onbruikbaar te maken die voor CTT zou worden gebruikt. Deze stap is puur om het even welk schrijven aan DM te vermijden of lading op verkeer te vermijden DM.
+ Houd er rekening mee dat CTT knooppunten, metagegevens van een migratieset van bron-AEM naar AEMaaCS migreert. Het zal geen handelingen op DM direct uitvoeren.

### Q: Wat zijn verschillende migratiebenaderingen wanneer de integratie DM op Bron AEM aanwezig is?

Lees de bovenstaande vraag en beantwoord deze eerder

(Dit zijn twee mogelijke opties, maar zijn niet beperkt tot deze twee). Het hangt van hoe de klant de UAT, Prestaties het testen, het beschikbare milieu wil benaderen en of een kloon voor migratie of niet wordt gebruikt. Beschouw deze twee als uitgangspunt voor de discussie

**Optie 1**

Als het aantal elementen/knooppunten in de bronomgeving zich op een lager niveau (~100K) bevindt, ervan uitgaande dat deze over een periode van 24 + 72 uur kunnen worden gemigreerd, inclusief extractie en opname, is de beste aanpak:

+ Migratie van productie rechtstreeks uitvoeren
+ Een eerste extractie en opname uitvoeren in AEMaaCS met `wipe=true`
   + Deze stap migreert alle knopen en binaire getallen
+ Doorgaan met werken aan on-premise / AMS Prod-auteur
+ Voer vanaf nu alle andere proefdrukken van migratiecycli uit met `wipe=true`
   + Merk op deze verrichting migreert volledige knooppuntenopslag, maar slechts gewijzigde vlekken in tegenstelling tot volledige vlekken. De vorige set lobs is te vinden in de Azure blob store van de AEMaaCS-doelinstantie.
   + Gebruik dit bewijs van migraties voor het meten van de migratieduur, gebruikerstoewijzing, tests, validatie van alle andere functies
+ Tot slot, vóór de week van go-live, voer vepe=ware migratie uit
   + De Dynamic Media aansluiten op AEMaaCS
   + Verbinding met DM-config van AEM bron op locatie verbreken

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

+ [Tips en trucs voor migreren naar Experience Manager in de cloud (top 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT-video met experts](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [Video&#39;s over andere AEMaaCS-onderwerpen in de Expert-reeks](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/aem-experts-series.html)
