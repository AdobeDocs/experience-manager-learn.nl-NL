---
title: Redenen voor upgrade begrijpen
description: Een uitsplitsing op hoog niveau van de belangrijkste functies voor klanten die een upgrade naar de nieuwste versie van Adobe Experience Manager 6 overwegen.
version: 6.5
topic: Upgrade
feature: Release Information
role: Leader, Architect, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 538
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2588'
ht-degree: 1%

---

# Redenen voor upgrade begrijpen

Een uitsplitsing op hoog niveau van de belangrijkste functies voor klanten die een upgrade naar de nieuwste versie van Adobe Experience Manager 6 overwegen.

## Belangrijke functies voor upgrades naar AEM 6.5

+ [Opmerkingen bij de release Adobe Experience Manager 6.5](https://helpx.adobe.com/experience-manager/6-5/release-notes.html)

### Verbeteringen in de Stichting

Adobe Experience Manager 6.5 blijft de stabiliteit, prestaties en draagbaarheid van het systeem verbeteren door:

+ **Java 11** ondersteuning (met behoud van ondersteuning voor Java 8).

### Websites maken en beheren

AEM Sites introduceert een aantal functies die zijn ontworpen om het maken en ontwikkelen van websites te versnellen:

+ **SPA Editor** Dankzij de ondersteuning kunnen SPA (toepassingen van één pagina) volledig worden ontworpen in AEM, waardoor een rijke, marktvriendelijke ontwerpervaring wordt ondersteund.
+_ **JavaScript SDK&#39;s**, een SPA Project Start Kit en ondersteunende bouwhulpmiddelen, bieden front-end ontwikkelaars de mogelijkheid SPA Editor-compatibele single page-applications onafhankelijk van AEM te ontwikkelen.
+ **Kernonderdelen** voegt een groot aantal nieuwe componenten toe, a **Componentbibliotheek** en diverse verbeteringen aan bestaande Core Components.
+ Verdere **Vertalingen** Verbeteringen stroomlijnen de vertaling van AEM Sites.

### Vloeiende ervaringen

AEM blijft Fluid Ervaring omarmen met nieuwe en verbeterde gereedschappen die het gebruik van inhoud buiten AEM vergemakkelijken.

+ **Inhoudsfragmenten** Vergelijking/Diff en annotaties van ondersteunde versies.
+ **HTTP-API voor AEM** steunen blootstellen **Inhoudsfragmenten** rechtstreeks in de DAM als **JSON**.
  **Ervaar fragmenten** ondersteuning **fullText zoeken** en **Ongeldige validatie cache van AEM Dispatcher** ter referentie **Pagina&#39;s**.

### Beheer van bedrijfsmiddelen

AEM Assets blijft voortbouwen op zijn rijke reeks mogelijkheden voor vermogensbeheer om het gebruik, het beheer en het begrip van het DAM te verbeteren. AEM 6.5 blijft de integratie tussen Adobe Creative Cloud en creatieve workflows verbeteren.

+ **Adobe-itemkoppeling** Creative Cloud maakt rechtstreeks verbinding met AEM Assets vanuit Adobe Creative Cloud-tools.
+ **Adobe Stock** Dankzij de integratie hebt u rechtstreeks vanuit de AEM Assets-ervaring toegang tot Adobe Stock-images, zodat u probleemloos inhoud kunt detecteren.
+ **App AEM** versie 2.0 loslaat en zichzelf opnieuw instelt en tegelijkertijd de prestaties en stabiliteit verbetert.
+ **Verbonden elementen** ondersteunt afzonderlijke AEM Sites-instanties voor naadloze toegang tot en gebruik van elementen van een andere AEM Assets-instantie.
+ Bijgewerkte videoondersteuning in **Dynamic Media**, inclusief **360-video** en **Aangepaste videominiaturen**.

### Inhoudsinformatie

AEM blijft bouwen aan zijn integratie met slimme technologieën, door gebruik te maken van machinaal leren en kunstmatige intelligentie om alle ervaringen te verbeteren.

+ **Adobe-itemkoppeling** add **Zoeken op visuele gelijkenis**, zodat vergelijkbare afbeeldingen gemakkelijk kunnen worden gedetecteerd en gebruikt binnen **Adobe Creative Cloud-gereedschappen**.

### Integrations

AEM groeit zijn vermogen om met andere diensten van Adobe te integreren:

+ **Ervaar fragmenten** hun integratie met **Adobe Target** door **Exporteren als JSON** aan Adobe Target en de mogelijkheid om **aanbiedingen op basis van fragmenten verwijderen** van **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), een exclusieve Adobe Managed Services (AMS)-klanten, biedt de volgende functies:

+ Cloud Manager-ondersteuning breidt AEM implementatieondersteuning uit van AEM Sites naar **AEM Assets**, inclusief **geautomatiseerde prestatietests voor de verwerking van bedrijfsmiddelen**.
+ **Automatisch schalen** van het AEM publicatieniveau op vooraf gedefinieerde drempelwaarden, zorgen voor een optimale ervaring voor de eindgebruiker.
+ **Niet-productiepijpleidingen** Ontwikkelingsteams toestaan om Cloud Manager te gebruiken om de kwaliteit van code voortdurend te controleren en te implementeren in lagere omgevingen (Development and QA).
+ **CI/CD Pipeline-API&#39;s** klanten toestaan programmatisch contact op te nemen met Cloud Manager, waardoor de integratiemogelijkheden worden vergroot met on-premise ontwikkelinfrastructuren.

## Functies van Stichting

Hieronder ziet u een matrix van de belangrijkste basisfuncties die AEM biedt. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [Opmerkingen bij de release AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> belangrijke verbeteringen in de functie in deze versie.***

***✔<sup>SP</sup> geeft aan dat de functie beschikbaar is via een Service Pack of Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Functie van Stichting</td>
            <td>5,6 x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Java 11-ondersteuning:</strong> AEM ondersteunt Java 11 (en Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Opslagplaats voor eik-inhoud</a>:</strong> Verstrekt veel grotere prestaties en scalability toen predecessorJasrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">eiken-run.jar Index Support</a>:</strong> Verbeterde re/indexering, het verzamelen van statistieken, en consistentie het controleren van indexen van eiken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Aangepaste zoekindexen</a>: </strong>
                Mogelijkheid om aangepaste indexdefinities toe te voegen om queryprestaties en zoekrelevantie te optimaliseren.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Onlinerevisie opschonen</a>:</strong>
                Beheer van opslagruimten zonder serverdowntime.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Opslag in TarMK- of MongoMK-opslagruimte</a>:</strong>
                <br> Opties voor het gebruik van eenvoudige, krachtige opslag op basis van bestanden van TarMK (versie van TarPM van de volgende generatie)
                <br> of horizontaal schalen met een door MongoDB ondersteunde gegevensopslagruimte met MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Prestaties en stabiliteit van MongoMK</a>:</strong>
            Sinds de introductie van MongoMK met AEM 6.0 zijn er verdere verbeteringen aangebracht.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            Gebruik een uitbreidbare cloudopslagoplossing om binaire elementen op te slaan.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Pariteit aanraakinterface:</strong>
                Voortdurende verbeteringen in de ontwerpgebruikersinterface voor snelheid met verhoogde productiviteit en pariteit van functies met klassieke gebruikersinterface.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnissearch:</strong>
                Snel zoeken en navigeren door AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Operations-dashboard</a>:</strong>
 Onderhoud uitvoeren, serverstatus controleren en prestaties analyseren vanuit AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Verbeteringen voor upgrade</a>:</strong>
            Verbeteringen voor upgrades maken het mogelijk AEM eenvoudiger en sneller te upgraden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/htl/using/overview.html" target="_blank">HTML-sjabloontaal</a>:</strong>
            Een moderne sjabloonengine die presentatie en logica van elkaar scheidt. Hiermee wordt de ontwikkeltijd van componenten aanzienlijk verkort. Incrementele functies die bij elke release worden toegevoegd.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Verkoopmodellen</a>:</strong>
            Een flexibel raamwerk voor het modelleren van JCR-bronnen in zakelijke objecten en logica. Incrementele functies die bij elke release worden toegevoegd.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Cloud Manager is exclusief voor klanten van Adobe Managed Services (AMS) en versnelt de ontwikkeling en implementatie via een geavanceerde CI/CD-pijplijn.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Beveiligingsfuncties

Hieronder ziet u een matrix van de belangrijkste beveiligingsfuncties die AEM biedt. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [Opmerkingen bij de beveiligingsrelease](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔ geeft aan dat de functie in deze versie aanzienlijk is verbeterd.***

***✔<sup>+</sup> geeft aan dat de functie beschikbaar is via een Service Pack of Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Beveiligingsonderdeel</td>
            <td>5,6 x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Servicegebruikers</a></strong>
            <br> Bij het samenvoegen van machtigingen wordt onnodig gebruik van beheerdersrechten vermeden.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Belangrijk opslagbeheer</a></strong>
            <br> Wereldwijde vertrouwde opslag, certificaten en sleutels worden allemaal beheerd in de opslagplaats.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>bescherming</strong></a>
            <br> Bescherming tegen vervalsingen voor verzoeken tussen sites buiten de box.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>ondersteuning</strong></a>
            <br> Ondersteuning voor het delen van bronnen met verschillende oorsprong voor meer toepassingsflexibiliteit.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Verbeterde ondersteuning voor SAML-verificatie</a><br>
 </strong>Verbeterde SAML-omleiding, geoptimaliseerde groepsgegevens en problemen met sleutelversleuteling zijn opgelost.
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP als OSGi-configuratie</a><br>
 </strong>Vereenvoudigt het beheer en de updates van LDAP-verificatie.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>OSGi-codering voor onbewerkte-tekstwachtwoorden<br>
 </strong>Wachtwoorden en andere vertrouwelijke waarden kunnen in gecodeerde vorm worden opgeslagen en automatisch worden gedecodeerd.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Verbeteringen voor CUG</a><br>
 </strong>De implementatie van een gesloten gebruikersgroep is opnieuw geschreven om prestatieproblemen en schaalbaarheidsproblemen te verhelpen.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL-wizard</a></strong>
            <br> UI om installatie en beheer van SSL te vereenvoudigen.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Ondersteuning voor ingekapselde token</a></strong>
            <br> Niet meer nodig voor 'kleverige' sessies ter ondersteuning van horizontale verificatie in publicatievarianten.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Ondersteuning voor Adobe IMS-verificatie</a><br>
 </strong>Uitsluitend voor Adobe Managed Services (AMS): centraal toegang tot AEM Author-instanties beheren via Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Sitefuncties

Hieronder ziet u een matrix van belangrijke Sites-functies die AEM biedt. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [Opmerkingen bij de release van AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> belangrijke verbeteringen in de functie in deze versie.***

***✔<sup>SP</sup> geeft aan dat de functie beschikbaar is via een Service Pack of Feature Pack.***

<table>
    <thead>
        <tr>
            <td><strong>Functie Sites</strong></td>
            <td>5,6 x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Touch Optimized Page Authoring</a>:</strong>
            Hiermee kunnen editors tablets en computers met aanraakschermen gebruiken.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Responsieve site-authoring</a>:</strong>
                In de lay-outmodus kunnen editors de grootte van componenten wijzigen op basis van apparaatbreedten voor responsieve sites.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Bewerkbare sjablonen</a>:</strong>
            Sta gespecialiseerde auteurs toe om paginasjablonen te maken en te bewerken.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Kernonderdelen</a>:</strong>
            Siteontwikkeling versnellen. Beschikbaar op GitHub voor frequente versieschema en flexibiliteit.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA Editor</a>:</strong>
            Creëer authorable, enaging Web Experience using Single-Page Application (SPA) frameworks die gebaseerd zijn op React.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Stijlsysteem:</strong>
            Verhoog het hergebruik van AEM component door de visuele weergave te definiëren met behulp van het in-context-stijlsysteem.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">MSM (Multi-Site Manager)</a>:</strong>
            Meerdere websites beheren die gemeenschappelijke inhoud delen (d.w.z. meerdere talen, meerdere merken).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Inhoud omzetten</a>:</strong>
            Plug and play framework is geïntegreerd met toonaangevende vertaalservices van derden.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Clientcontextframework van de volgende generatie voor het aanpassen van inhoud.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Starten</a>:</strong>
            Inhoud ontwikkelen voor een toekomstige release zonder het dagelijks ontwerpen te verstoren.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Content Fragments:</strong>
            Maak en curseer redactionele inhoud die losgekoppeld is van de presentatie, zodat u deze eenvoudig opnieuw kunt gebruiken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Ervaar fragmenten</a>:</strong>
            Maak herbruikbare ervaringen en variaties die zijn geoptimaliseerd voor desktopcomputers, mobiele apparaten en sociale kanalen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Inhoudsservices:</strong>
            Exporteer inhoud van AEM als JSON voor gebruik op verschillende apparaten en toepassingen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics Integration and Content Insights:</strong>
                Eenvoudige integratie van Adobe Analytics en DTM. Geef prestatiegegevens weer in de ontwerpomgeving.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target-integratie</a>:</strong>
            De stapsgewijze tovenaar om gerichte ervaringen tot stand te brengen, herbruikbare aanbiedingsbibliotheken te creëren.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign-integratie</a>:</strong>
            Eenvoudige integratie met de e-mailcampagneoplossing van de volgende generatie.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Tags in Adobe Experience Platform-integratie</a>:</strong>
            Integreer met de volgende generatie cloudservice voor tagbeheer van de Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Schermen:</strong>
            Ervaringen beheren voor digitale signalen en kiosken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>:</strong>
            Lever merkbare, persoonlijke boodschappenbeleving op het web, mobiele en sociale aanraakpunten.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Gemeenschappen</a>:</strong>
            Forums, threaded commentaar, gebeurtenissencalenders en veel andere functies maken een grote betrokkenheid bij bezoekers van de site mogelijk.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Elementen

Hieronder ziet u een matrix van de belangrijkste elementen die door AEM worden aangeboden. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [Opmerkingen bij de release van AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔ geeft aan dat de functie in deze versie aanzienlijk is verbeterd.***

***✔<sup>+</sup> geeft aan dat de functie beschikbaar is via een Service Pack of Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Functie voor elementen</td>
            <td>5,6 x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Touch Optimized UI</a>:</strong>
            Elementen beheren op een desktopcomputer of op apparaten met aanraakbediening.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Geavanceerde metagegevensbeheer</a>:</strong>
            Metagegevenssjablonen, de metagegevensschemaeditor en de bewerking van bulkmetagegevens.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Taak</a> en workflowbeheer:</strong>
            Vooraf gebouwde workflows en taken voor het beoordelen en goedkeuren van digitale middelen met behulp van AEM Projecten.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Schaalbaarheid en prestaties:</strong>
            Verbeterde ondersteuning voor inslikken, uploaden en opslaan op schaal.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Elementen HTTP-API</a>:</strong>
            Programmaticaal communiceren met middelen via HTTP en JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Delen van koppeling</a>:</strong>
            Eenvoudig ad-hocdelen van digitale middelen zonder dat u zich hoeft aan te melden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            SAAS-oplossing voor cloudservice voor naadloze delen en distributie van digitale middelen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Verbonden elementen</a>:</strong>
            AEM Sites-instanties hebben naadloos toegang tot en kunnen elementen van een ander AEM Assets-exemplaar gebruiken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Asset Insights</a>:</strong>
            Gebruik Adobe Analytics om de interactie van klanten met digitale middelen en de weergave in AEM vast te leggen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Meertalige activa</a>:</strong>
            Ondersteuning voor vertaling van metagegevens van elementen wordt automatisch ondersteund met de taalbasis.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Slimme tags en modernisering</a>:</strong>
            Gebruik Adobe Sensei om afbeeldingen automatisch van tags te voorzien met nuttige metagegevens.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Smart Translation Search</a>:</strong>
            Zoektermen automatisch vertalen bij het zoeken naar AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server-integratie</a>:</strong>
            Produccatalogi genereren. Maak brochures, flyers en druk advertenties op basis van sjablonen voor InDesigns.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">App AEM</a>:</strong>
            Synchroniseer middelen met het lokale bureaublad voor bewerking met Creative Suite producten.
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Afbeeldingsbibliotheek Adobe</a>:</strong>
                <br> Photoshop- en Acrobat-PDF-bibliotheken die worden gebruikt voor bestanden van hoge kwaliteit.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe-itemkoppeling</a>:</strong>
            Open AEM Assets rechtstreeks vanuit de Adobe Cloud-toepassingen maken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock-integratie</a>:</strong>
            Toegang tot en gebruik Adobe Stock-beelden naadloos rechtstreeks vanuit AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+</sup> belangrijke verbeteringen in de functie in deze versie.***

***✔<sup>SP</sup> geeft aan dat de functie beschikbaar is via een Service Pack of Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Dynamic Media-functie</td>
            <td>5,6 x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3 +FP</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imaging</a>:</strong>
            U kunt dynamisch afbeeldingen leveren in verschillende formaten en formaten, waaronder Slim uitsnijden.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Video</a>:</strong>
            Geavanceerde videocodering en adaptieve videostreaming</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Interactieve media</a>:</strong>
            Maak interactieve banners, video's met klikbare inhoud om belangrijke aanbiedingen te tonen.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Sets (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Afbeelding</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Draaien</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Gemengde media</a>):</strong>
            Gebruikers kunnen zoomen, pannen, roteren en een kijkervaring van 360 graden simuleren.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Viewers</a>:</strong>
            Aangepaste mediaspelers en voorinstellingen met branding met ondersteuning voor verschillende schermen/apparaten.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Aflevering</a>:</strong>
            Flexibele opties voor het koppelen of insluiten van Dynamic Media-inhoud en levering via HTTP/2-protocol.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Upgrade van Scene7 naar Dynamic Media:</strong>
            Mogelijkheid om hoofdelementen te migreren en door te gaan met het gebruik van bestaande S7-URL's.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Forms-functies

Hieronder ziet u een matrix met belangrijke AEM Forms Add-on-functies die AEM biedt. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

***✔<sup>+</sup> belangrijke verbeteringen in de functie in deze versie.***

***✔<sup>SP</sup> geeft aan dat de functie beschikbaar is via een Service Pack of Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Forms-functie</td>
            <td>5,6 x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Adaptieve Forms Editor</a>:</strong>
            Maak aantrekkelijke, responsieve en adaptieve formulieren op basis van apparaat- en browserinstellingen.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Document van record</a>:</strong>
            Maak een document voor langdurige opslag van gegevens die u ophaalt of voor afdrukken gereed hebt.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Thema-editor</a>:</strong>
            Maak herbruikbare thema's om stijlcomponenten en deelvensters van een formulier te maken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Sjablooneditor</a>:</strong>
            Standaardiseren en implementeren van beste praktijken voor adaptieve formulieren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Acrobat Sign-integratie</a>:</strong>
            Implementatie van op Acrobat Sign gebaseerde ondertekeningsscenario's voor geïntegreerde formulieren toestaan.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondentenbeheer</a>:</strong>
            Met AEM Forms kunt u persoonlijke en interactieve klantcorrespondentie maken, beheren en leveren.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integratie van gegevens van derden</a>:</strong>
            Met behulp van gegevensintegratie worden gegevens opgehaald van verschillende gegevensbronnen op basis van gebruikersinvoer in een formulier. Bij het verzenden van het formulier worden de vastgelegde gegevens teruggeschreven naar de gegevensbronnen.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Workflow (op OSGi) voor Forms-verwerking</a>:</strong>
            Vereenvoudigde implementatie van goedkeuringsprocessen voor formulieren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integratie met Marketing Cloud</a>:</strong>
            Integratie met Adobe Analytics en Adobe Target om de ervaringen van klanten te verbeteren en te meten.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Formulierbeheer</a>:</strong>
            Eén locatie voor het beheer van alle formulieren/documenten/correspondentie, zoals het inschakelen van analyses, vertaling, A/B-tests, revisies en publicaties.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms App</a>:</strong>
            Verwerking van online of offline formulieren toestaan in een app op iOS, Android of Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Interactieve communicatie</a>:</strong>
            Creeer rijke mededelingen zoals gerichte verklaringen met interactieve elementen zoals grafieken (vroeger genoemd geworden AanpassingsDocumenten).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Workflow (J2EE) voor Forms-verwerking:</strong>
            Complexe formulieren/documentafhankelijke workflows maken met behulp van een intuïtieve IDE.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Beveiliging van AEM Forms-documenten</a>:</strong>
            Veilige toegang tot en machtiging voor PDF- en Office-documenten.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Testkaders</a>:</strong>
            Gebruik het Calvin-framework en de Chrome-insteekmodule om adaptieve formulieren te ondersteunen en te debuggen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

