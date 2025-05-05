---
title: Brand Portal gebruiken
description: Videodoorloop van de integratie van AEM Author en AEM Assets Brand Portal.
feature: Brand Portal
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
duration: 2460
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1702'
ht-degree: 0%

---

# Brand Portal gebruiken met AEM Assets{#using-brand-portal-with-aem-assets}

Videohulplijnen voor de integratie van Adobe Experience Manager (AEM) Assets Brand Portal.

## Functies en verbeteringen in Brand Portal september 2019

Brand Portal, september 2019, introduceert met name Asset Sourcing, die de snelheid van de inhoud verhoogt en een gemakkelijke en snelle uitwisseling van activa tussen auteurs van Experience Manager en derde creatieven en contribuanten mogelijk maakt.

### Brand Portal Asset Sourting{#asset-sourcing}

Brand Portal Asset Sourcing wordt gebruikt om middelen te verzamelen van externe agentschappen en teams, waardoor deze naadloos worden gesynchroniseerd naar Experience Manager Author voor controle en gebruik.

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

&lbrace;6.5 SP2 (6.5.2) van de Auteur 6.5 van Experience Manager of groter wordt vereist om Activa te gebruiken die **

Het overzicht [ laat de Auteur van Experience Manager voor Activa toe die ](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=nl-NL) voor instructies op hoe te om de Levering van Activa op de Auteur van Experience Manager te vormen en te plaatsen.

## Functies en verbeteringen in Brand Portal februari 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

De release van Brand Portal van februari 2019 richt zich op verbeteringen in het zoeken naar tekst en op verzoeken van klanten van topniveau.

### Verbeteringen voor zoeken

Brand Portal verbetert het zoeken met gedeeltelijk zoeken naar tekst op voorspelling van eigenschappen in het filtervenster. Als u gedeeltelijk zoeken in tekst wilt toestaan, moet u Gedeeltelijk zoeken in Voorspelling eigenschap in het zoekformulier inschakelen.

Lees verder om meer te weten over gedeeltelijk tekstonderzoek en vervangingsonderzoek.

#### Gedeeltelijke woordzoekopdracht

U kunt nu naar elementen zoeken door alleen een deel (dat een woord of twee is) van de gezochte woordgroep op te geven in het filtervenster.

**het geval van het Gebruik**: Gedeeltelijk woordonderzoek is nuttig wanneer u van de nauwkeurige combinatie woorden onzeker bent die in de gezochte uitdrukking voorkomen.

Als uw zoekformulier in Brand Portal bijvoorbeeld Eigenschapvoorspelling gebruikt voor gedeeltelijke zoekopdrachten naar de titel van elementen, worden met de term kamp alle elementen geretourneerd die voorkomen in het woordkamp in de titelzin.

#### Zoekopdracht met jokertekens

De Brand Portal staat het gebruik van het sterretje (*) toe in zoekquery samen met een deel van het woord in de gezochte uitdrukking.

**het geval van het Gebruik**:Als u niet zeker van de nauwkeurige woorden die in de gezochte uitdrukking voorkomen bent, kunt u een vervangingsonderzoek gebruiken om de hiaten in uw onderzoeksvraag te vullen.

Als u bijvoorbeeld klimmen* opgeeft, worden alle elementen geretourneerd waarvan de woorden beginnen met de tekens die in de titelzin klimmen als in het zoekformulier in Brand Portal Eigenschapvoorspelling wordt gebruikt voor gedeeltelijk zoeken naar de titel van de elementen.

Op dezelfde manier specificeren:

* \*klimt retourneert alle elementen met woorden die eindigen met tekens die in de titelzin klimmen.
* \*klimt\* retourneert alle elementen met woorden die de tekens bevatten die in hun titelzin klimmen.

#### Maphiërarchie inschakelen

Beheerders kunnen nu configureren hoe de mappen bij het aanmelden worden weergegeven aan gebruikers zonder beheer (Editors, Viewers en Gastgebruikers).
[ laat de Configuratie van de Hiërarchie van de Omslag toe ](https://helpx.adobe.com/nl/experience-manager/brand-portal/using/brand-portal-general-configuration.html) wordt toegevoegd in Algemene Montages, in het admin hulpmiddelenpaneel. Als de configuratie:

* Als deze optie is ingeschakeld, is de mappenstructuur die begint in de hoofdmap zichtbaar voor gebruikers zonder beheerdersrechten. Aldus, die hen een navigatie ervaring gelijkend op beheerders verlenen.
* Uitgeschakeld. Alleen de gedeelde mappen worden op de bestemmingspagina weergegeven.

[ laat de functionaliteit van de Hiërarchie van de Omslag toe ](https://helpx.adobe.com/nl/experience-manager/brand-portal/using/brand-portal-general-configuration.html) (wanneer toegelaten) helpt u de omslagen met de zelfde namen onderscheiden die van verschillende hiërarchieën worden gedeeld. Bij het aanmelden zien niet-beheerders nu de virtuele bovenliggende mappen (en vooroudermappen) van de gedeelde mappen.

De gedeelde mappen worden in de desbetreffende directory&#39;s in virtuele mappen ingedeeld. U kunt deze virtuele mappen herkennen met een vergrendelingspictogram.

De standaardminiatuur van de virtuele mappen is de miniatuurafbeelding van de eerste gedeelde map.

### Ondersteuning voor dynamische media-video-uitvoeringen

Gebruikers van wie de AEM Author-instantie zich in de hybride modus Dynamische media bevindt, kunnen naast de originele videobestanden ook een voorvertoning van de dynamische media-uitvoeringen weergeven en deze downloaden.

Om voorproef en download van dynamische media vertoningen op specifieke huurdersrekeningen toe te staan, moeten de beheerders Dynamische Configuratie van Media (videodienst URL (DM-Gateway URL) en registratie identiteitskaart specificeren om de dynamische video) in Videoconfiguratie van admin hulpmiddelenpaneel te halen.

Dynamische mediavideo&#39;s kunnen worden voorvertoond op:

* Pagina met elementgegevens
* Weergave van de kaart van het element
* Voorvertoningspagina voor delen koppelen

Dynamische videocodes voor media kunnen worden gedownload van:

* Brand Portal
* Gedeelde koppeling

### Gepland publiceren naar Brand Portal

Assets (en omslagen) publiceren werkschema van [ AEM (6.4.2.0) ](https://helpx.adobe.com/nl/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) de instantie van de Auteur aan Brand Portal kan voor een recentere datum, tijd worden gepland.

Gepubliceerde middelen kunnen ook op een latere datum (tijd) uit het portaal worden verwijderd door de workflow Unpublish from Brand Portal te plannen.

### Configureerbare alias van huurder in URL

Organisaties kunnen hun portaal-URL aanpassen door een alternatief voorvoegsel in de URL te hebben. Om een alias voor huurdersnaam in hun bestaande portaal URL te krijgen, moeten de organisaties de steun van Adobe contacteren.

Merk op dat alleen het voorvoegsel van de Brand Portal URL kan worden aangepast en niet de volledige URL.
Zo kan een organisatie met een bestaand domein `wknd.brand-portal.adobe.com` op verzoek `wkndinc.brand-portal.adobe.com` maken.

Nochtans, kan de instantie van de Auteur van AEM [ worden gevormd ](https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) slechts met huurder identiteitskaart URL en niet met huurder alias (afwisselend) URL.

**Geval van het Gebruik** : De organisaties kunnen aan hun brandingsbehoeften voldoen door portaal URL te krijgen aangepast, in plaats van aan URL te kleven die door Adobe wordt verstrekt.

## Functies en verbeteringen in Brand Portal december 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### Toegang voor gasten

AEM Brand Portal biedt gasttoegang tot het portaal. Een gastgebruiker vereist geen geloofsbrieven om het portaal in te gaan en kan tot alle openbare omslagen en inzamelingen toegang hebben en downloaden. Gastgebruikers kunnen elementen toevoegen aan hun lichtbak (privéverzameling) en deze downloaden. Ze kunnen ook zoeken naar slimme tags en voorspelden van zoekopdrachten weergeven die door beheerders zijn ingesteld. De gastzitting staat gebruikers niet toe om inzamelingen tot stand te brengen en bewaarde onderzoeken of hen verder te delen, tot omslag en inzamelingsmontages toegang te hebben, en activa als verbindingen te delen.

### Versnelde download

Brand Portal-gebruikers kunnen Aspera-gebaseerde snelle downloads gebruiken om snelheden tot 25x sneller te krijgen en genieten van een naadloze downloadervaring, ongeacht hun locatie over de hele wereld. Als gebruikers de elementen sneller van Brand Portal of een gedeelde koppeling willen downloaden, moeten ze de optie Downloadversnelling inschakelen selecteren in het dialoogvenster Downloaden, op voorwaarde dat downloadversnelling is ingeschakeld in hun organisatie.

* [ Gids om downloads van Brand Portal te versnellen ](https://helpx.adobe.com/nl/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [ de Server van de Test van Aspera Connect ](https://test-connect.asperasoft.com/)

### Rapport gebruikersaanmelding

Er is een nieuw rapport geïntroduceerd om gebruikersaanmeldingen bij te houden. Het rapport Gebruikersaanmelding kan nuttig zijn om organisaties in staat te stellen de gedelegeerde beheerders en andere gebruikers van Brand Portal te controleren en te controleren.

In de rapportlogs worden namen, e-mailadressen, personen (beheerder, viewer, editor, gast), groepen, laatste aanmelding, activiteitstatus en aantal aanmelding van elke gebruiker weergegeven.

### Toegang tot originele uitvoeringen

Beheerders kunnen gebruikerstoegang tot originele afbeeldingsbestanden beperken (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.6 winkelen) en toegang geven tot rendities met lage resolutie die ze downloaden van Brand Portal of gedeelde koppeling. Deze toegang kan op het niveau van de gebruikersgroep van het lusje van Groepen van de pagina van Rollen van de Gebruiker in het paneel van Admin hulpmiddelen worden gecontroleerd.

### Nieuwe configuraties

Er worden zes nieuwe configuraties toegevoegd voor beheerders om de volgende functies in of uit te schakelen voor specifieke huurders:

* Toegang voor gasten toestaan
* Gebruikers toegang tot Brand Portal aanvragen
* Beheerders toestaan elementen te verwijderen uit Brand Portal
* Openbare collecties maken
* Maken van openbare slimme verzamelingen toestaan
* Downloadversnelling toestaan

### Andere verbeteringen

* *de hiërarchieweg van de Omslag op kaart en lijstmeningen* - laat gebruikers toe om de plaats van de omslagen te kennen die binnen een instantie van Brand Portal worden opgeslagen. Hiermee kunnen gebruikers mappen met dezelfde naam onderscheiden in verschillende maphiërarchie.
* *optie van het Overzicht* — verstrekt niet-admin gebruikersmeta-gegevens over de activa/de omslag door de activa/de omslag te selecteren en dan de overzichtsoptie van de toolbar te selecteren. Op dit moment worden titel, datum en pad weergegeven

### Adobe I/O Hosts-gebruikersinterface voor het configureren van Auth-integratie

Brand Portal gebruikt Adobe I/O [ https://legacy-oauth.cloud.adobe.io/ ](https://legacy-oauth.cloud.adobe.io/) interface om toepassing tot stand te brengen JWT, die het vormen de integratie van Auth toelaat om de integratie van AEM Assets met Brand Portal toe te staan. Eerder werd de interface voor het configureren van OAuth-integratie gehost in `https://marketing.adobe.com/developer/` . Meer over het integreren van AEM Assets met Brand Portal voor het publiceren van activa en inzamelingen aan Brand Portal verwijs [ de integratie van AEM Assets met Brand Portal ](https://helpx.adobe.com/nl/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html) vormen.

## Functies en verbeteringen in Brand Portal februari 2018{#brand-portal-features-and-enhancements-632}

Nieuwe functies verbeteren de functionaliteit die gericht is op het uitlijnen van Brand Portal met AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### Navigatieverbeteringen

* Bijgewerkte gebruikersinterface die wordt uitgelijnd op de AEM en gebruikmaakt van de interface van Coral3.
* Snelle en eenvoudige toegang tot beheertools via het nieuwe Adobe-logo.
* Productnavigatie door middel van een bedekking
* Snelle navigatie naar bovenliggende mappen vanuit een onderliggende map.
* Optie voor Omnzoekopdrachten om naar beheergereedschappen en inhoud te navigeren.
* Met de Kaartweergave, de lijstweergave en de kolomweergave kunt u gemakkelijk door geneste mappen bladeren.
* Het sorteren van middelen in de Mening van de Lijst is niet meer beperkt tot het aantal activa die op het scherm worden getoond. Alle elementen in een map worden gesorteerd.

### Verbeteringen zoeken

* Met de zoekfunctie kunt u snel zoeken naar elementen en bestanden in Brand Portal.
* In het kader van het algemene onderzoek kunt u ook zoeken naar elementen in een bepaalde map of locatie
* Automatische trefwoordsuggesties maken het zoeken gemakkelijker
* Verbeter uw algemene zoekopdracht met extra filters. Optie om het onderzoeksresultaat in een Slimme Inzameling te bewaren voor u om uw onderzoek in een recentere tijd opnieuw te bezoeken.
* Ondersteunt zoeken naar slimme tags voor elementen
* AEM Smart gelabelde middelen kunnen worden gedeeld van AEM naar Brand Portal en slimme tags gebruiken voor het zoeken naar middelen in Brand Portal.

### Verbeteringen voor het delen van bestanden

* De gebruiker kan een middel delen gebruikend de optie van het verbindingsaandeel.
* Tijdens het delen van elementen moet de gebruiker een vervaldatum instellen voor elk element. Biedt gebruikers meer controle over de gedeelde elementen.
* Een externe gebruiker met de koppeling Asset Share kan de afbeelding downloaden en de eigenschappen ervan bekijken.
* De oorspronkelijke geneste maphiërarchie blijft behouden voor gedownloade elementmappen.

### Rapportage- en beheerscapaciteiten

* Metagegevensschema van AEM Assets kan nu van AEM naar Brand Portal worden gepubliceerd.
* Beheerders kunnen drie typen rapporten maken en beheren: gedownloade, verlopen en gepubliceerde elementen
* Capaciteit om de kolom te vormen die in het rapport moet worden omvat.
* Voorinstellingen voor afbeeldingen maken voor elementen in Brand Portal.
* Mogelijkheid om het Rail-formulier voor zoekopdrachten in Admin of Search Forms te wijzigen en aanvullende filteropties op te nemen.
* Aangepaste achtergrond voor uw merk bijwerken en voorvertonen
* Gebruiksrapport voor informatie over het aantal gebruikers, de gebruikte opslagruimte en het totale aantal elementen.

## Aanvullende bronnen{#additional-resources}

* [ wat in Brand Portal ](https://helpx.adobe.com/nl/experience-manager/brand-portal/using/whats-new.html) nieuw is
* [ de replicatieagenten van de Auteur van AEM ](https://helpx.adobe.com/nl/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [ Gids aan Versnelde Download ](https://helpx.adobe.com/nl/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [ AEM Assets Brand Portal Adobe Docs ](https://helpx.adobe.com/nl/experience-manager/brand-portal/using/brand-portal.html)
* [ AEM Assets Dynamic Media Adobe Docs ](https://experienceleague.adobe.com/docs/?lang=nl-NL)
* [ download Aspera verbindt ](https://downloads.asperasoft.com/connect2/)
* [ de Server van de Test van Aspera Connect ](https://test-connect.asperasoft.com/)
