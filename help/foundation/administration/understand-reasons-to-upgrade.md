---
title: Redenen voor upgrade begrijpen
description: Een uitsplitsing op hoog niveau van de belangrijkste functies voor klanten die een upgrade naar de nieuwste versie van Adobe Experience Manager 6 overwegen.
version: Experience Manager 6.5
topic: Upgrade
feature: Release Information
role: Leader, Architect, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 538
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '2576'
ht-degree: 1%

---

# Redenen voor upgrade begrijpen

Een uitsplitsing op hoog niveau van de belangrijkste functies voor klanten die een upgrade naar de nieuwste versie van Adobe Experience Manager 6 overwegen.

## Belangrijke functies voor de upgrade naar AEM 6.5

+ [ Adobe Experience Manager 6.5 de Nota&#39;s van de Versie ](https://helpx.adobe.com/nl/experience-manager/6-5/release-notes.html)

### Verbeteringen in de Stichting

Adobe Experience Manager 6.5 blijft de stabiliteit, prestaties en draagbaarheid van het systeem verbeteren door:

+ **Java 11** steun (terwijl het handhaven van steun Java 8).

### Websites maken en beheren

AEM Sites introduceert een aantal functies die zijn ontworpen om het maken en ontwikkelen van websites te versnellen:

+ **steun van de Redacteur van het KUUROORD** &lbrace;staat KUUROORD (enig-paginatoepassingen) toe om volledig in AEM worden ontworpen, ondersteunend een rijke, Marketer-vriendschappelijke auteurservaring.
+_ **JavaScript SDK**, een Uitrusting van het Begin van het Project van het KUUROORD en het steunen bouwt hulpmiddelen, staan front-end ontwikkelaars toe om SPA Redacteur-Compatibele enige pagina-toepassingen onafhankelijk van AEM te ontwikkelen.
+ **Componenten van de Kern** voegt een veelheid van nieuwe componenten, de Bibliotheek van de a **Component** evenals een verscheidenheid van verhogingen aan bestaande Componenten van de Kern toe.
+ Verdere **verbeteringen van de Vertalingen** stroomlijnen vertaling van AEM Sites.

### Vloeiende ervaringen

AEM blijft Fluid Experience met nieuwe en verbeterde gereedschappen die het gebruik van inhoud buiten AEM vergemakkelijken.

+ {de Fragmenten van de Inhoud **de Vergelijking/Diff van de steunversie en Annotaties van 0}.**
+ **AEM HTTP API** steunt het blootstellen van **de Fragmenten van de Inhoud** direct in DAM als **JSON**.
  **steun van Fragmenten van de Ervaring** Fulltext Onderzoek **en** de Invalidatie van het Geheime voorgeheugen van AEM Dispatcher **voor het van verwijzingen voorzien** Pagina&#39;s **.**

### Beheer van bedrijfsmiddelen

AEM Assets blijft voortbouwen op zijn rijke reeks mogelijkheden voor vermogensbeheer om het gebruik, het beheer en het begrip van het DAM te verbeteren. AEM 6.5 blijft de integratie tussen Adobe Creative Cloud en creatieve workflows verbeteren.

+ **de Verbinding van de Activa van Adobe** verbindt direct creatieve producten met AEM Assets van de hulpmiddelen van Adobe Creative Cloud.
+ **Adobe Stock** de integratie staat directe toegang tot beelden van Adobe Stock direct van de ervaring van AEM Assets toe, die tot een naadloze ervaring van de inhoudsontdekking leidt.
+ **de Desktop App van AEM** geeft versie 2.0 vrij en herziet zichzelf terwijl het verbeteren van prestaties en stabiliteit.
+ **Verbonden Assets** steunt afzonderlijke instanties van AEM Sites om tot activa van een verschillende instantie van AEM Assets foutloos toegang te hebben en te gebruiken.
+ Bijgewerkte videosteun in **Dynamische Media**, met inbegrip van **360 Video** en **Douane Videominiaturen**.

### Inhoudsinformatie

AEM blijft zijn integratie met slimme technologieën opbouwen, door gebruik te maken van machinaal leren en kunstmatige intelligentie om alle ervaringen te verbeteren.

+ **voegt de Vermogensverbinding van Adobe** toe **Visueel Onderzoek van de Gelijksgelijkenis**, dat voor gelijkaardige beelden toestaat gemakkelijk worden ontdekt en binnen **hulpmiddelen van Adobe Creative Cloud** worden gebruikt.

### Integrations

AEM is beter in staat om te integreren met andere Adobe-services:

+ **Fragmenten van de Ervaring** verdiept hun integratie met **Adobe Target** door **Uitvoer als JSON** aan Adobe Target en de capaciteit te steunen om de op fragment-Gebaseerde aanbiedingen van de Ervaring **van** Adobe Target **te schrappen.**

### AMS CLOUD MANAGER

[ Cloud Manager ](https://adobe.ly/2HODmsv), exclusief aan Adobe Managed Services (AMS) klanten, biedt de volgende eigenschappen aan:

+ Cloud Manager steunt breidt de plaatsingssteun van AEM van AEM Sites tot **AEM Assets** uit, met inbegrip van **geautomatiseerde prestaties het testen van activa verwerking**.
+ **auto-schrapt** van AEM publiceer rij bij vooraf bepaalde drempels, verzekert een optimale eindgebruikerervaring.
+ **niet-productiepijpleidingen** staan ontwikkelingsteams toe om hefboomwerking Cloud Manager onophoudelijk code-kwaliteit te controleren en aan lagere milieu&#39;s (Ontwikkeling en QA) op te stellen.
+ **CI/CD de Pijpleiding APIs** staat klanten toe om met Cloud Manager programmatically in werking te stellen, die integratiemogelijkheden met de infrastructuur van de on-premise ontwikkeling verdiepen.

## Functies van Stichting

Hieronder ziet u een matrix van de belangrijkste basisfuncties die AEM biedt. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [ de versienota&#39;s van de Stichting van AEM ](https://helpx.adobe.com/nl/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+ </sup> significante verhogingen aan de eigenschap in deze versie.***

***✔<sup>SP </sup> wijst op de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap is.***

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
                <strong> Java 11 Steun:</strong> AEM steunt Java 11 (evenals Java 8).
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
                <strong> <a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank"> de Bewaarplaats van de Inhoud van Oak </a>:</strong> verstrekt veel grotere prestaties en scalability toen predecessorJasje 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html"> oak-run.jar de Steun van de Index </a>:</strong> Verbeterde re/indexering, statistiekinzameling, en consistentie het controleren van indexen van Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank"> Indexen van het Onderzoek van de Douane </a>: </strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank"> Online Opruiming van de Revisie </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank"> TarMK of MongoMK opslagplaats </a>:</strong>
                <br> Opties voor het gebruik van eenvoudige, krachtige opslag op basis van bestanden van TarMK (versie van TarPM van de volgende generatie)
                <br> of horizontaal schalen met een gegevensopslagruimte met MongoDB-back-up met MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank"> prestaties en stabiliteit MongoMK </a>:</strong>
            Sinds de introductie in AEM 6.0 zijn er verdere verbeteringen aangebracht in MongoMK.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore"> Amazon S3 DataStore </a>:</strong>
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
            <td><strong> de Pariteit van de Eigenschap van de Aanraak UI:</strong>
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
            <td><strong> Omnissearch:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank"> Dashboard van Verrichtingen </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank"> Verbeteringen van de Verbetering </a>:</strong>
            Verbeteringen voor upgrades maken upgrades van AEM eenvoudiger en sneller op locatie mogelijk.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/htl/using/overview.html" target="_blank"> HTML Taal van het Malplaatje </a>:</strong>
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
            <td><strong> <a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank"> Sling Models </a>:</strong>
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
            <td><strong> <a href="https://adobe.ly/2HODmsv" target="_blank"> Cloud Manager </a>: </strong>
                Exclusief voor klanten van Adobe Managed Services (AMS) versnelt Cloud Manager de ontwikkeling en implementatie via een geavanceerde CI/CD-pijpleiding.</td>
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

Hieronder vindt u een matrix met belangrijke beveiligingsfuncties die door AEM worden aangeboden. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [ de versienota&#39;s van de Veiligheid van de Veiligheid ](https://helpx.adobe.com/nl/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔geeft aan dat de functie in deze versie aanzienlijk is verbeterd.***

***✔<sup>+ </sup> wijst op de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap is.***

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
        <td><strong><a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank"> Gebruikers van de Dienst </a></strong>
            <br> Als u machtigingen voor compartimenteren, voorkomt u onnodig gebruik van beheerdersrechten.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"> Zeer belangrijk Beheer van de Opslag </a></strong>
            <br> Wereldwijde vertrouwde opslag, certificaten en sleutels die allemaal worden beheerd in de opslagplaats.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"> <strong> CSRF </strong> <strong> bescherming </strong> </a>
            <br> Beveiliging voor smeden voor meerdere sites buiten de box.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"> <strong> CORS </strong> <strong> steun </strong> </a>
            <br> Ondersteuning voor het delen van bronnen met verschillende oorsprong voor meer flexibiliteit bij de toepassing.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong> <a href="https://experienceleague.adobe.com/docs/?lang=nl-NL" target="_blank"> Verbeterde de authentificatiesteun van SAML </a><br>
 </strong> Verbeterde opnieuw richten SAML, optimaliseerde groepsinfo, en zeer belangrijke encryptiekwesties opgelost.
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
        <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank"> LDAP als Configuratie OSGi </a><br>
 </strong> vereenvoudigt beheer en updates van authentificatie LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong> OSGi de steun van de Encryptie voor duidelijk-tekstwachtwoorden <br>
 </strong> de Wachtwoorden en andere gevoelige waarden kunnen in gecodeerde vorm worden bewaard en automatisch worden gedecrypteerd.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank"> de verhogingen van de GECG </a><br>
 </strong> de implementatie van de Groep van de sluiten-Gebruiker is herschreven om prestaties en scalability kwesties te richten.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank"> SSL Tovenaar </a></strong>
            <br> UI om de installatie en het beheer van SSL te vereenvoudigen.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank"> Encapsulated Symbolische Steun </a></strong>
            <br> Niet langer nodig voor 'kleverige' sessies voor ondersteuning van horizontale verificatie in publicatievarianten.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank"> Steun van de Authentificatie van Adobe IMS </a><br>
 </strong> Uitsluitend aan Adobe Managed Services (AMS), beheer centraal toegang tot de instanties van de Auteur van AEM via het Systeem van Adobe IMS (Identity Management).</td>
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

Hieronder ziet u een matrix van belangrijke Sites-functies die door AEM worden aangeboden. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [ de versienota&#39;s van AEM Sites ](https://helpx.adobe.com/nl/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+ </sup> significante verhogingen aan de eigenschap in deze versie.***

***✔<sup>SP </sup> wijst op de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap is.***

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
            <td><strong> <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank"> Aanraak Geoptimaliseerde Authoring van de Pagina </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank"> Responsieve Authoring van de Plaats </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank"> Bewerkbare Malplaatjes </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/core-components/user-guide.html" target="_blank"> de Componenten van de Kern </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank"> Redacteur van het KUUROORD </a>:</strong>
            Creëer authorable, enaging Webervaringen gebruikend het kader van de Toepassing van de enig-Pagina (SPA) die op React wordt voortgebouwd.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong> Systeem van de Stijl:</strong>
            Verhoog het hergebruik van AEM-componenten door hun visuele weergave te definiëren met behulp van het in-context-stijlsysteem.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔ <sup> SP </sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/msm.html" target="_blank"> Multisite Manager (MSM) </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/translation.html" target="_blank"> Vertaling van de Inhoud </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank"> ContextHub </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank"> Lanceringen </a>:</strong>
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
            <td><strong> de Fragmenten van de Inhoud:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank"> Fragmenten van de Ervaring </a>:</strong>
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
            <td><strong> de Diensten van de Inhoud:</strong>
            Exporteer inhoud uit AEM als JSON voor gebruik op verschillende apparaten en toepassingen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔ <sup> SP </sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> de Integratie van Adobe Analytics en Inzicht van de Inhoud:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank"> de Integratie van Adobe Target </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank"> de Integratie van Adobe Campaign </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank"> Markeringen in de Integratie van Adobe Experience Platform </a>:</strong>
            Integreer met de Adobe-service voor het beheer van tags van de volgende generatie.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> Screens:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank"> eCommerce </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/communities/using/overview.html" target="_blank"> Gemeenschappen </a>:</strong>
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

## Assets-functies

Hieronder vindt u een matrix met belangrijke Assets-functies die AEM biedt. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [ de versienota&#39;s van AEM Assets ](https://helpx.adobe.com/nl/experience-manager/6-5/release-notes/assets.html)

***✔geeft aan dat de functie in deze versie aanzienlijk is verbeterd.***

***✔<sup>+ </sup> wijst op de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap is.***

<table>
    <thead>
        <tr>
            <td>Assets-functie</td>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank"> Geoptimaliseerde UI van de Aanraak </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/metadata.html" target="_blank"> Geavanceerd Beheer van Meta-gegevens </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"> Taak </a> en het Beheer van het Werkschema:</strong>
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
            <td><strong> Schaalbaarheid en Prestaties:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank"> HTTP API van Assets </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/link-sharing.html" target="_blank"> het Aandeel van de Verbinding </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/brand-portal/using/brand-portal.html" target="_blank"> Brand Portal </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank"> Verbonden Assets </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank"> Inzichten van Activa </a>:</strong>
            Gebruik Adobe Analytics om de interactie van klanten met digitale middelen en weergave in AEM vast te leggen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank"> Meertalige Assets </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank"> Slimme Markeringen en Moderatie </a>:</strong>
            Gebruik Adobe AI om afbeeldingen automatisch van tags te voorzien met nuttige metagegevens.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank"> Slim Vertaal Onderzoek </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/indesign.html" target="_blank"> de Integratie van Adobe InDesign Server </a>:</strong>
            Produccatalogi genereren. Maak brochures, flyers en druk advertenties op basis van InDesign-sjablonen.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/desktop-app/aem-desktop-app.html" target="_blank"> App van de Desktop van AEM </a>:</strong>
            Synchroniseer middelen met het lokale bureaublad voor bewerking met Creative Suite-producten.
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank"> de Bibliotheek van de Beelden van Adobe </a>:</strong>
                <br> Photoshop- en Acrobat PDF-bibliotheken die worden gebruikt voor het manipuleren van bestanden van hoge kwaliteit.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> <a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank"> de Verbinding van Activa van Adobe </a>:</strong>
            Open AEM Assets rechtstreeks vanuit Adobe Create Cloud-toepassingen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank"> de Integratie van Adobe Stock </a>:</strong>
            Toegang tot en gebruik Adobe Stock-afbeeldingen naadloos rechtstreeks vanuit AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔ <sup> SP </sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+ </sup> significante verhogingen aan de eigenschap in deze versie.***

***✔<sup>SP </sup> wijst op de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap is.***


<table>
    <thead>
        <tr>
            <td>Dynamische mediafunctie</td>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/managing-assets.html" target="_blank"> Beelden </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/video-profiles.html" target="_blank"> Video </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/interactive-images.html" target="_blank"> Interactieve Media </a>:</strong>
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
            <td><strong> Reeksen (<a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/image-sets.html" target="_blank"> Beeld </a>, <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/spin-sets.html" target="_blank"> Spin </a>, <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank"> Gemengde Media </a>):</strong>
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
            <td><strong> <a href="https://experienceleague.adobe.com/docs/?lang=nl-NL" target="_blank"> Kijkers </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank"> Levering </a>:</strong>
            Flexibele opties voor het koppelen of insluiten van dynamische media-inhoud en levering via HTTP/2-protocol.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> Verbetering van Scene7 aan Dynamische Media:</strong>
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

***✔<sup>+ </sup> significante verhogingen aan de eigenschap in deze versie.***

***✔<sup>SP </sup> wijst op de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap is.***

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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank"> Aangepaste Redacteur van Forms </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank"> Document van Verslag </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/forms/using/themes.html" target="_blank"> Redacteur van het Thema </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/forms/using/template-editor.html" target="_blank"> Redacteur van het Malplaatje </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank"> de Integratie van Acrobat Sign </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/forms/using/cm-overview.html" target="_blank"> het Beheer van de Correspondentie </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank"> de Integratie van Gegevens van de Derde </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank"> Werkschema (op OSGi) voor de Verwerking van Forms </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank"> Integratie met Marketing Cloud </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank"> de Manager van de Vorm </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank"> AEM Forms App </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/aem-forms/6-5/adaptive-document.html" target="_blank"> Interactieve Mededelingen </a>:</strong>
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
            <td><strong> Werkschema (J2EE) voor de Verwerking van Forms:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank"> de Veiligheid van het Document van AEM Forms </a>:</strong>
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
            <td><strong> <a href="https://helpx.adobe.com/nl/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank"> het Testen Frameworks </a>:</strong>
            Met het Calvin-framework en de Chrome-insteekmodule kunt u adaptieve formulieren ondersteunen en debuggen.</td>
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

