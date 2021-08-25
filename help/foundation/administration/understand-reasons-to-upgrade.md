---
title: Redenen voor upgrade begrijpen
description: Een uitsplitsing op hoog niveau van de belangrijkste functies voor klanten die een upgrade naar de nieuwste versie van Adobe Experience Manager overwegen.
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '3462'
ht-degree: 1%

---


# Redenen voor upgrade begrijpen

Een uitsplitsing op hoog niveau van de belangrijkste functies voor klanten die een upgrade naar de nieuwste versie van Adobe Experience Manager overwegen.

## Belangrijke functies voor upgrades naar AEM 6.5

+ [Opmerkingen bij de release Adobe Experience Manager 6.5](https://helpx.adobe.com/experience-manager/6-5/release-notes.html)

### Verbeteringen in de Stichting

Adobe Experience Manager 6.5 blijft de stabiliteit, prestaties en draagbaarheid van het systeem verbeteren door:

+ **Java 11** -ondersteuning (met behoud van Java 8-ondersteuning).

### Websites maken en beheren

AEM Sites introduceert een aantal functies die zijn ontworpen om het maken en ontwikkelen van websites te versnellen:

+ **SPA** Editorsupport maakt het mogelijk SPA (toepassingen van één pagina) volledig te ontwerpen in AEM, zodat een rijke, marktvriendelijke ontwerpervaring wordt ondersteund.
+_ **JavaScript SDK&#39;s**, een SPA Start-kit voor projecten en ondersteunende buildgereedschappen, stellen front-end ontwikkelaars in staat SPA Editor-compatibele single page-applications onafhankelijk van AEM te ontwikkelen.
+ **De Componenten van de kern** voegt een veelheid van nieuwe componenten, een  **Component** Bibliotheek evenals een verscheidenheid van verhogingen aan bestaande Componenten van de Kern toe.
+ Verdere **Vertalingen**-verbeteringen stroomlijnen de vertaling van AEM Sites.

### Vloeiende ervaringen

AEM blijft Fluid Ervaring omarmen met nieuwe en verbeterde gereedschappen die het gebruik van inhoud buiten AEM vergemakkelijken.

+ **Content** Fragmentsondersteuning Version Comparison/Diff and Annotations.
+ **AEM HTTP-** API-middelen ondersteunt het direct toegankelijk maken van  **Content** Fragmentsmiddelen in de DAM als  **JSON**.
   **Ervaar** Fragmentsondersteuning voor  **fullText** Searchand  **AEM Dispatcher Cache** Invalidatie voor het verwijzen naar  **pagina**&#39;s.

### Beheer van bedrijfsmiddelen

AEM Assets blijft voortbouwen op zijn rijke reeks mogelijkheden voor vermogensbeheer om het gebruik, het beheer en het begrip van het DAM te verbeteren. AEM 6.5 blijft de integratie tussen Adobe Creative Cloud en creatieve workflows verbeteren.

+ **Adobe Asset** Link verbindt creatieve personen rechtstreeks met AEM Assets vanaf Adobe Creative Cloud-tools.
+ **Dankzij de Adobe** Stockintegration hebt u rechtstreeks vanuit de AEM Assets-ervaring toegang tot Adobe Stock-afbeeldingen, zodat u naadloze content kunt detecteren.
+ **AEM Desktop** Appreleases versie 2.0 en re-envisioning zelf terwijl het verbeteren van prestaties en stabiliteit.
+ **Connected** Assets ondersteunt afzonderlijke AEM Sites-instanties voor naadloze toegang tot en gebruik van middelen van een andere AEM Assets-instantie.
+ Bijgewerkte videoondersteuning in **Dynamic Media**, inclusief **360 Video** en **Aangepaste videominiaturen**.

### Inhoudsinformatie

AEM blijft bouwen aan zijn integratie met slimme technologieën, door gebruik te maken van machinaal leren en kunstmatige intelligentie om alle ervaringen te verbeteren.

+ **Adobe Asset** Link voegt zoekfunctie voor  **visuele gelijkenis toe**, zodat vergelijkbare afbeeldingen gemakkelijk kunnen worden gedetecteerd en gebruikt in  **Adobe Creative Cloud-gereedschappen**.

### Integrations

AEM groeit zijn vermogen om met andere diensten van de Adobe te integreren:

+ **Ervaar Fragmentaties** verdiept hun integratie met  **Adobe** Targetby die  **Uitvoer als** JSONaan Adobe Target steunt en de capaciteit om de op fragment-Gebaseerde  **aanbiedingen van de Ervaring van** Adobe Target **te** schrappen.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), exclusief voor klanten van Adobe Managed Services (AMS), biedt de volgende functies:

+ Cloud Manager biedt ondersteuning voor AEM implementatieondersteuning van AEM Sites tot **AEM Assets**, inclusief **geautomatiseerde prestatietests voor de verwerking van bedrijfsmiddelen**.
+ **Automatisch** schalen van de AEM-publicatielaag op vooraf gedefinieerde drempelwaarden zorgt voor een optimale ervaring voor de eindgebruiker.
+ **Niet-productie** pipetten stellen ontwikkelingsteams in staat om Cloud Manager te gebruiken om voortdurend de kwaliteit van de code te controleren en te implementeren in lagere omgevingen (ontwikkeling en kwaliteitscontrole).
+ **CI/CD Pipeline** APIsallow-klanten om programmatically met de Manager van de Wolk in dienst te nemen, die integratiemogelijkheden met de infrastructuur van de on-premise ontwikkeling verdiepen.

## Functies van stichting

Hieronder ziet u een matrix van de belangrijkste basisfuncties die AEM biedt. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

+ [Opmerkingen bij de release AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔ <sup>+</sup> belangrijke verbeteringen aan de functie in deze versie.***

***✔ <sup></sup> SPdenotes is de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap.***

<table>
    <thead>
        <tr>
            <td>Functie van Stichting</td>
            <td>5,6 x</td>
            <td>6.0</td>
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Opslagplaats</a> voor ongewenste inhoud:</strong> biedt veel betere prestaties en schaalbaarheid en biedt vervolgens een voorganger Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Ondersteuning voor</a> eiken-run.jar-index:</strong> Verbeterde re/indexering, statistische verzameling en consistentiecontrole van eiken-indexen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Aangepaste zoekindexen</a>:  </strong>
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
                Opslagruimte beheren zonder serverdowntime.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK of MongoMK opslagplaats opslag</a>:</strong>
                <br> Opties om eenvoudige, krachtige dossier-gebaseerde opslag van TarMK (volgende-generatieversie van TarPM) te gebruiken 
                <br> of horizontaal met een MongoDB gesteunde bewaarplaats met MongoMK te schrapen.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Prestaties en stabiliteit</a> van MongoMK: er zijn </strong>
            voortdurend verbeteringen aangebracht in MongoMK sinds de introductie met AEM 6.0.</td>
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
            Gebruik uitbreidbare cloudopslagoplossing om binaire elementen op te slaan.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Touch UI-kenmerkpariteit:</strong>
                voortdurende verbeteringen van de ontwerpinterface voor snelheid met verhoogde productiviteit en pariteit van functies met klassieke gebruikersinterface.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omgaan met zoeken:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Operations-dashboard</a>:onderhoud </strong>
 uitvoeren, serverstatus controleren en prestaties analyseren vanuit AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Verbeteringen</a> upgraden:verbeteringen </strong>
            voor upgrades maken het mogelijk AEM eenvoudiger en sneller te upgraden.</td>
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
            een moderne sjabloonengine die presentatie en logica van elkaar scheidt. Hiermee wordt de ontwikkeltijd van componenten aanzienlijk verkort. Incrementele functies die bij elke release worden toegevoegd.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling Models</a>:</strong>
            een flexibel kader voor het modelleren van middelen JCR in bedrijfsvoorwerpen en logica. Incrementele functies die bij elke release worden toegevoegd.
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Wolkenbeheer</a>:  </strong>
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

***✔ <sup>+</sup> geeft aan dat de functie beschikbaar is via een Service Pack of Feature Pack.***

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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">ServicegebruikersCompartimentaliseert machtigingen om onnodig gebruik van beheerdersrechten te voorkomen. </a></strong>
            <br> </td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Key Store </a></strong>
            <br> ManagementWereldwijde vertrouwde opslag, certificaten en sleutels worden allemaal beheerd in de opslagplaats.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CSRFprotectionCross-Site Request-bescherming uit de doos.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>Ondersteuning </strong> <strong></strong></a>
            <br> CORSsupportCross-Origin Resource Sharing voor meer toepassingsflexibiliteit.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Verbeterde SAML-</a><br>
 </strong>verificatieondersteuningVerbeterde SAML-omleiding, geoptimaliseerde groepsgegevens en problemen met sleutelversleuteling zijn opgelost. 
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP als OSGi-</a><br>
 </strong>configuratieVereenvoudigt het beheer en de updates van LDAP-verificatie.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>Ondersteuning voor OSGi-codering voor <br>
 </strong>wachtwoorden voor normale tekst en andere vertrouwelijke waarden kan worden opgeslagen in gecodeerde vorm en automatisch worden gedecodeerd.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG-</a><br>
 </strong>verbeteringenDe implementatie van een gesloten gebruikersgroep is opnieuw geschreven om prestatieproblemen en schaalbaarheidsproblemen te verhelpen.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> WizardUI om installatie en beheer van SSL te vereenvoudigen.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Encapsulated Token </a></strong>
            <br> SupportNiet meer nodig voor 'kleverige' sessies ter ondersteuning van horizontale verificatie voor publicatievarianten.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS-</a><br>
 </strong>verificatieondersteuningExclusief voor Adobe Managed Services (AMS), centraal de toegang tot AEM-auteurinstanties beheren via Adobe IMS (Identity Management System).</td>
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

***✔ <sup>+</sup> belangrijke verbeteringen aan de functie in deze versie.***

***✔ <sup></sup> SPdenotes is de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap.***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Tik op Geoptimaliseerde pagina-authoring</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Responsieve site-authoring</a>:in </strong>
                de lay-outmodus kunnen editors componenten vergroten of verkleinen op basis van apparaatbreedten voor responsieve sites.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Bewerkbare sjablonen</a>:gespecialiseerde auteurs </strong>
            toestaan paginasjablonen te maken en te bewerken.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Core Components</a>:</strong>
            Siteontwikkeling versnellen Beschikbaar op GitHub voor frequente versieschema en flexibiliteit.</td>
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
            Maak een authorabele webervaringen met behulp van SPA-frameworks (Single Page Application) die zijn gebaseerd op React of Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Stijlsysteem:hergebruik van AEM component </strong>
            vergroten door hun visuele weergave te definiëren met het in-context-stijlsysteem.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">MSM (Multi-Site Manager)</a>:meerdere websites </strong>
            beheren die gemeenschappelijke inhoud delen (bijvoorbeeld meerdere talen, meerdere merken).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Content Translation</a>:</strong>
            Plug and Play-framework is geïntegreerd met toonaangevende vertaalservices van derden.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:het kader van de </strong>
            context van de volgende generatiecliënt voor verpersoonlijking van inhoud.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Startpagina</a>:</strong>
            Inhoud ontwikkelen voor een toekomstige release zonder het dagelijks ontwerpen te onderbreken.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Inhoudsfragmenten:</strong>
            redactionele inhoud losgekoppeld van de presentatie maken en beheren, zodat u deze eenvoudig opnieuw kunt gebruiken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Geniet van fragmenten</a>:</strong>
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
            <td><strong>Content Services:Inhoud </strong>
            exporteren vanuit AEM als JSON voor gebruik op verschillende apparaten en toepassingen.</td>
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
            stapsgewijze wizard om doelgerichte ervaringen te maken en herbruikbare aanbiedingsbibliotheken te maken.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign Integration</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe Start Integration</a>:</strong>
            Integreer met de volgende generatie tapebeheerservice van de Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Schermen:ervaringen </strong>
            beheren voor digitale signage en kiosken.</td>
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
            Bied merkboodschap, persoonlijke boodschappen via internet, mobiele apparaten en sociale aanraakpunten.
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
            forums, threaded commentaar, gebeurtenissencalenders en vele andere functies maken een grote betrokkenheid met bezoekers van de site mogelijk.</td>
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

***✔ <sup>+</sup> geeft aan dat de functie beschikbaar is via een Service Pack of Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Elementfunctie</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Tik op Geoptimaliseerde interface</a>:middelen </strong>
            beheren op een desktopcomputer of op apparaten met aanraakbediening.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Geavanceerd metagegevensbeheer</a>:</strong>
            metagegevenssjablonen, de metagegevensschemaeditor en het bewerken van bulkmetagegevens.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> Taakbeheer en  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> workflowbeheer:</strong>
            vooraf gebouwde workflows en taken voor het controleren en goedkeuren van digitale middelen met behulp van AEM Projecten.</td>
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
            verbeterde ondersteuning voor inslikken, uploaden en opslaan op schaal.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Elementen-HTTP-API</a>:</strong>
            programmatisch communiceren met middelen via HTTP en JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Delen</a> van koppelingen:</strong>
            eenvoudig ad-hocdelen van digitale elementen zonder dat u zich hoeft aan te melden.</td>
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
            Cloud-service SAAS-oplossing voor naadloze delen en distributie van digitale middelen.</td>
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
            AEM Sites-instanties kunnen naadloos toegang krijgen tot elementen van een andere AEM Assets-instantie en deze gebruiken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Asset Insights</a>:Adobe Analytics </strong>
            benutten om de interactie van digitale middelen en weergave door klanten in AEM vast te leggen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Meertalige elementen</a>:ondersteuning voor </strong>
            vertaling van metagegevens van elementen automatisch met taalwortels.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Slimme tags en modernisering</a>:gebruik Adobe Sensei </strong>
            om afbeeldingen automatisch van tags te voorzien met nuttige metagegevens.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Smart Translation Search</a>:zoektermen </strong>
            automatisch vertalen bij zoeken naar AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server-integratie</a>:productcatalogi </strong>
            genereren. Maak brochures, flyers en druk advertenties op basis van InDesign-sjablonen.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM Desktop App</a>:middelen </strong>
            synchroniseren met het lokale bureaublad voor bewerking met Creative Suite producten.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe Imaging Library</a>:</strong>
                <br> Photoshop and Acrobat PDF libraries used for high-quality file manipulation.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe-elementkoppeling</a>:</strong>
            AEM Assets rechtstreeks openen vanuit Adobe Cloud-toepassingen maken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock Integration</a>:</strong>
            Naadloos toegang tot en gebruik Adobe Stock imagery rechtstreeks vanuit AEM.</td>
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

***✔ <sup>+</sup> belangrijke verbeteringen aan de functie in deze versie.***

***✔ <sup></sup> SPdenotes is de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap.***


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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Afbeeldingen</a>:</strong>
            Dynamisch afbeeldingen leveren in verschillende formaten en formaten, inclusief Slim uitsnijden.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
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
            <td><strong>Sets (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Image</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Spin</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Gemengd Media</a>):</strong>
            Hiermee kunnen gebruikers inzoomen, pannen, roteren en een kijkervaring van 360 graden simuleren.</td>
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
            Aangepaste, van het merk voorziene mediaspelers en voorinstellingen met ondersteuning voor verschillende schermen/apparaten.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Levering</a>:</strong>
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
            mogelijkheid om master middelen te migreren en bestaande S7-URL's te blijven gebruiken.</td>
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

***✔ <sup>+</sup> belangrijke verbeteringen aan de functie in deze versie.***

***✔ <sup></sup> SPdenotes is de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap.***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Adaptieve Forms Editor</a>:aantrekkelijke, responsieve en adaptieve formulieren </strong>
            maken op basis van apparaat- en browserinstellingen.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Document of Record</a>:</strong>
            Maak een document voor langdurige opslag van een ervaring voor het vastleggen van gegevens of voor afdrukken geschikte versie.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">Thema-editor</a>:herbruikbare thema's </strong>
            maken om stijlcomponenten en deelvensters van een formulier te maken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Sjablooneditor</a>:best practices voor adaptieve formulieren </strong>
            standaardiseren en implementeren</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign-integratie</a>:</strong>
            implementatie van geïntegreerde Adobe Sign-formuliergebaseerde ondertekeningsscenario's toestaan.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondentiebeheer</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Gegevensintegratie</a> van derden:</strong>
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
            vereenvoudigde implementatie van goedkeuringsprocessen voor formulieren.</td>
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
            Eén locatie voor het beheer van alle formulieren/documenten/correspondentie, zoals het inschakelen van analyses, vertaling, A/B-tests, revisies en publiceren.
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
            toestaan voor online/offline verwerking van formulieren in een app op iOS, Android of Windows.</td>
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
            Maak uitgebreide communicatie, zoals gerichte instructies met interactieve elementen, zoals grafieken (voorheen Adaptieve documenten genoemd).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Workflow (J2EE) voor Forms-verwerking:complexe formulieren </strong>
            maken/documentafhankelijke workflows met behulp van een intuïtieve IDE.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms-documentbeveiliging</a>:</strong>
            veilige toegang tot en machtiging voor PDF- en Office-documenten.
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
            gebruik het Calvin-framework en de Chrome-insteekmodule voor ondersteuning en foutopsporing voor adaptieve formulieren.</td>
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

## Functies van Gemeenschappen

Hieronder ziet u een matrix met belangrijke AEM Communities Add-on-functies die AEM biedt. Sommige van deze functies zijn geïntroduceerd in oudere versies van incrementele verbeteringen die in elke versie zijn toegevoegd.

***✔ <sup>+</sup> belangrijke verbeteringen aan de functie in deze versie.***

***✔ <sup></sup> SPdenotes is de eigenschap beschikbaar via een Pak van de Dienst of het Pak van de Eigenschap.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Functie Gemeenschappen</td>
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
            <td rowspan="7">Functies van Gemeenschappen</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Forums</a>:</strong> (Sociaal Component Framework) Maak nieuwe onderwerpen of bekijk, volg, zoek en verplaats bestaande onderwerpen.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">Vraag</a>:</strong>
                Vragen stellen, weergeven en beantwoorden.</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>:blogartikelen en opmerkingen </strong>
                maken aan de publiczijde.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideatie</a>:ideeën </strong>
                maken en delen met de gemeenschap of bestaande ideeën weergeven, volgen en er commentaar op geven.
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Kalender</a>:</strong>
                (Social Component Framework) Verstrek informatie over gebeurtenissen uit de gebruikersgemeenschap aan bezoekers van de site.
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Bestandsbibliotheek</a>:bestanden </strong>
                uploaden, beheren en downloaden binnen de communitysite.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Gebruikersgroepen</a>: 
            </strong>Een reeks gebruikers kan tot lidgroepen behoren, en kan collectief rollen worden toegewezen.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Toewijzing</a>:Leermiddelen </strong>
            maken en toewijzen aan leden van de gebruikersgemeenschap.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Enablement</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> Catalogus en  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Resourcebeheer</a>:</strong>
            Toegangsbronnen uit catalogus.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Leerpadebeheer</a>:cursussen of groepen bronnen voor activering </strong>
            beheren.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Enablement Reporting</a>:</strong>
            Reporting on enablement resources and learning paths.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Betrokkenheid bij Enablement</a>:opmerkingen </strong>
            toevoegen over bronnen voor activering.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Enablement Analytics</a>:</strong>
            Video Analytics, Progress Reporting, and Assignment Reporting</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> Opmerkingen en bijlagen:</strong>
            (Sociaal Componentkader) Als lid van de gemeenschap deelt hij de mening en kennis over de inhoud op de communautaire site.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conversie van inhoudsfragment:UGC-bijdragen </strong>
            converteren naar inhoudsfragmenten.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Revisies</a>:</strong>
                (Social Component Framework) Als lid van de community kunt u een stuk inhoud beoordelen met een combinatie van opmerkingen en beoordelingsfuncties.</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Waarderingen</a>:/strong&gt; (Social Component Framework) Als lid van de gemeenschap waardeert u een stuk inhoud.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Stemmen</a>:</strong>
                (Sociaal Component Framework) Als lid van de gemeenschap stem of onderstem een stuk inhoud.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tags</a>:labels (trefwoorden of labels) </strong>
            aan inhoud koppelen om snel inhoud te zoeken.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Zoeken</a>:</strong>
            voorspellende en suggestieve zoekopdrachten.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">translatie</a>:</strong>
            machinevertaling van door de gebruiker gegenereerde inhoud.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">Beheer</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Sitebeheer</a>:sites </strong>
            maken met functies voor gemeenschappen.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Sjablonen</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> Sitesjablonen en  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> groepssjablonen voor het maken van volledig functionele communitysites op basis van wizards.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Bewerkbare sjablonen:</strong>
            communitybeheerders in staat stellen rijke ervaringen op te bouwen met AEM bewerkbare sjablonen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Groepen of subgemeenschappen</a>:</strong>
            dynamisch subgemeenschappen maken binnen gemeenschappen.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderatie</a>:door de gebruiker gegenereerde inhoud </strong>
            modereren.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Bulkmoderatie</a>:De console van de </strong>
            Moderatie om gebruiker geproduceerde inhoud bulksgewijs te beheren.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Spam-detectie en Winstgevencilters</a>:</strong>
            automatische spamdetectie.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Lid Management</a>:</strong>
            Gebruikersprofielen en groepen beheren vanuit het beheergebied van leden.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Responsief ontwerp</a>:</strong>
            AEM Communities-sites reageren hierop.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analyse</a>:</strong>
            Integreren met Adobe Analytics voor belangrijke inzichten in het gebruik van gemeenschapssites.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">Leden</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Scores en Badging</a>:</strong>
             (Geavanceerde scoring van Adobe Sensei) Identificeer leden van de gemeenschap als experts en belonen hen.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Activiteiten en  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">Meldingen</a>:</strong>
            Bekijk recente activiteitenstroom, en krijg op de hoogte gebracht over gebeurtenissen van belang.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Berichten</a>:</strong>
            Directe overseinen aan gebruikers en groepen.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Sociale aanmeldingen</a>:</strong>
            Aanmelden met hun Facebook- of Twitter-account.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Platform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongo Storage)</a>:</strong>
            Door de gebruiker gegenereerde inhoud (UGC) wordt direct in een lokale MongoDB-instantie voortgezet</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (de Opslag van het Gegevensbestand)</a>:</strong>
            Gebruiker-geproduceerde inhoud (UGC) wordt voortgeduurd direct in een lokale MySQL gegevensbestandinstantie.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Cloud Storage)</a>:</strong>
                Door de gebruiker gegenereerde inhoud (UGC) blijft extern aanwezig in een cloudservice die wordt gehost en beheerd door Adobe.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                Community-inhoud wordt opgeslagen in JCR en UGC is toegankelijk via een auteur- (of publicatieexemplaar)-instantie waarnaar deze is gepost.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Gebruiker en de Synchronisatie</a> van de Groep:</strong>
            Synchroniseer gebruikers en groepen over publiceer instanties wanneer het gebruiken van een Publish landbouwbedrijftopologie.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities voegt verbeteringen toe via releases om organisaties in staat te stellen hun gebruikers te betrekken en in te schakelen door:

+ **@** mentionsupport in door de gebruiker gegenereerde inhoud.
+ Verbeterde toegankelijkheid via **Toetsenbordnavigatie** in **Enablement**-componenten.
+ Verbeterde **Bulkmodernisering** met **Aangepaste filters**.
+ **Bewerkbare** sjablonen stellen gemeenschapsbeheerders in staat om rijke ervaringen van de gemeenschap in AEM op te bouwen.
+ Gebruikers kunnen nu **directe berichten in bulk** naar alle leden van een groep verzenden.
