---
title: Personalisatie met AEM Experience Fragments en Adobe Target
seo-title: Personalisatie met Adobe Experience Manager (AEM) Experience Fragments en Adobe Target
description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Experience Manager Experience Fragments en Adobe Target.
seo-description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Experience Manager Experience Fragments en Adobe Target.
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '1729'
ht-degree: 0%

---


# Personalisatie met AEM Experience Fragments en Adobe Target

Met de mogelijkheid om AEM Experience Fragments naar Adobe Target te exporteren als HTML-aanbiedingen, kunt u het gebruiksgemak en de kracht van AEM combineren met krachtige mogelijkheden voor Automated Intelligence (AI) en Machine Learning (ML) in Target om ervaringen op schaal te testen en te personaliseren.

AEM brengt al uw inhoud en middelen in een centrale plaats samen om uw verpersoonlijkingsstrategie te voeden. Met AEM kunt u eenvoudig inhoud voor desktops, tablets en mobiele apparaten op één locatie maken zonder dat u code hoeft te schrijven. Het is niet nodig om pagina&#39;s te maken voor elk apparaat. AEM past automatisch elke ervaring aan met uw inhoud.

Met Doel kunt u gepersonaliseerde ervaringen op schaal bieden op basis van een combinatie van op regels gebaseerde en op AI gebaseerde methoden voor het leren van machines die gedrags-, context- en offlinevariabelen bevatten.  Met Doel kunt u eenvoudig A/B- en MVT-activiteiten (Multivariate) instellen en uitvoeren om de beste aanbiedingen, inhoud en ervaringen te bepalen.

De fragmenten van de ervaring vertegenwoordigen een enorme stap voorwaarts om inhoudmakers met verkopers te verbinden die bedrijfsresultaten drijven gebruikend Doel.

## Overzicht van scenario

De plaats van WKND is van plan om een **Uitdaging** SkateFest door heel Amerika door hun website aan te kondigen en zou hun plaatsgebruikers willen hebben omhoog voor de auditie die in elke staat wordt uitgevoerd. Als teller, bent u de taak toegewezen om een campagne op de WKND homepage van de plaats in werking te stellen, met banners berichten relevant voor de plaats van de gebruikers en een verbinding aan de pagina van gebeurtenisdetails. Laten we de homepage van de WKND-site verkennen en leren hoe we een persoonlijke ervaring voor een gebruiker kunnen maken en leveren op basis van zijn/haar huidige locatie.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target/Optimization Team)

### Vereisten

* **AEM**
   * [AEM auteur- en publicatieexemplaar](./implementation.md#getting-aem) dat wordt uitgevoerd op respectievelijk localhost 4502 en 4503.
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Target](https://experiencecloud.adobe.com)

### Startpagina WKND-site

![AEM doelscenario 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. Marketer initieert de WKND SkateFest campagnegatie met AEM Content Editor en geeft details over de vereisten.
   * ***Voorschrift***: Promote WKND SkateFest-campagne op de WKND-homepage van de WKND-site met persoonlijke inhoud voor bezoekers uit elk land in de Verenigde Staten. Voeg een nieuw inhoudsblok onder de carrousel van de Homepagina toe met een achtergrondafbeelding, tekst en een knop.
      * **Achtergrondafbeelding**: Afbeelding moet relevant zijn voor de status van waaruit de gebruiker de WKND-sitepagina bezoekt.
      * **Tekst**: &quot;Aanmelden voor de Audition&quot;
      * **Knop**: &quot;Gebeurtenisdetails&quot; die naar de WKND SkateFest-pagina verwijzen
      * **WKND SkateFest Page**: een nieuwe pagina met gebeurtenisdetails, met inbegrip van de auditieplaats, datum, en tijd.
1. Op basis van de vereisten maakt AEM Content Editor een Experience Fragment voor het inhoudsblok en exporteert het als een voorstel naar Adobe Target. Om gepersonaliseerde inhoud voor alle staten in de Verenigde Staten te dienen, kan de inhoudontwerper één master variatie van het Fragment van de Ervaring tot stand brengen en dan 50 andere variaties, één voor elke staat creëren. Inhoud voor elke statusvariatie met relevante afbeeldingen en tekst kan vervolgens handmatig worden bewerkt. Bij het ontwerpen van een Experience Fragment hebben inhoudseditors via de optie Asset Finder snel toegang tot alle elementen die in AEM Assets beschikbaar zijn. Wanneer een Experience Fragment naar Adobe Target wordt geëxporteerd, worden alle variaties ook als aanbiedingen naar Adobe Target overgebracht.

1. Nadat het Fragment van de Ervaring van AEM naar Adobe Target als Voorstellen, kunnen de marketers tot Activiteiten in Doel leiden gebruikend deze Voorstellen. Op basis van de WKND-campagne SkateFest-site moet de marketeter een persoonlijke ervaring maken en leveren aan bezoekers van de WKND-site vanuit elke staat. Om een Ervaring tot stand te brengen richtend activiteit, moet de marktmeter het publiek identificeren. Voor onze WKND SkateFest-campagne moeten we 50 verschillende soorten publiek maken, gebaseerd op hun locatie waar ze de WKND-website bezoeken.
   * [Het publiek](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) bepaalt het doel voor uw activiteit en wordt gebruikt overal waar het richten beschikbaar is. Doelpubliek is een gedefinieerde set bezoekerscriteria. Aanbiedingen kunnen worden gericht op specifieke doelgroepen (of segmenten). Alleen bezoekers die bij dat publiek horen, zien de ervaring die voor hen is bedoeld.  U kunt bijvoorbeeld een aanbieding doen aan een publiek dat is samengesteld uit bezoekers die een bepaalde browser gebruiken of van een specifieke geo-locatie.
   * Een [voorstel](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) is de inhoud die tijdens campagnes of activiteiten op uw webpagina&#39;s wordt weergegeven. Wanneer u uw webpagina&#39;s test, meet u het succes van elke ervaring met verschillende aanbiedingen op uw locaties. Een voorstel kan verschillende typen inhoud bevatten, zoals:
      * Afbeelding
      * Tekst
      * **HTML**
         * *HTML-aanbiedingen worden gebruikt voor de activiteit van dit scenario*
      * Koppeling
      * Knop

## Activiteiten van de inhoudseditor

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Publiceer het Experience Fragment voordat u het exporteert naar Adobe Target.

## Marktactiviteiten

### Een publiek maken met geo-gerichte {#marketer-audience}

1. Ga naar uw organisaties, [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.ExperienceCloud.adobe.com)
1. Meld u aan met uw Adobe ID en zorg ervoor dat u zich in de juiste organisatie bevindt.
1. Klik vanuit de oplossingsschakelaar op **Doel** en **start** Adobe Target.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Navigeer naar het tabblad **Aanbiedingen** en zoek naar WKND-aanbiedingen. U moet de lijst met Experience Fragments-variaties kunnen bekijken, geëxporteerd van AEM als HTML-aanbiedingen. Elke aanbieding komt overeen met een status. Bijvoorbeeld, *WKND SkateFest Californië* is de aanbieding die aan een bezoeker van de Plaats van WKND uit Californië wordt gediend.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Klik in de hoofdnavigatie op **Soorten publiek**.

   Een Marketer moet 50 verschillende soorten publiek creëren voor bezoekers van de WKND-site die uit elk land in de Verenigde Staten van Amerika komen.

1. Als u een publiek wilt maken, klikt u op de knop Publiek **** maken en geeft u een naam op voor uw publiek.

   **Indeling Audience Name: WKND-\&lt;*state*\>**

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Klik op Regel **toevoegen > Geo**.
1. Klik op **Selecteren** en selecteer een van de volgende opties:
   * Land
   * **Staat** *(Uitgezochte Staat voor de Campagne van de SkateFest van de Plaats WKND)*
   * Plaats
   * Postcode
   * Breedte
   * Lengtegraad
   * DMA
   * Mobiele vervoerder

   **Geo** - Gebruik doelgroepen voor gebruikers op basis van hun geografische locatie, waaronder hun land, staat/provincie, stad, postcode, DMA of mobiele luchtvaartmaatschappij. Met Geolocation-parameters kunt u activiteiten en ervaringen richten op basis van de geografie van uw bezoekers. Dit gegeven wordt verzonden met elk verzoek van het Doel en is gebaseerd op het IP van de bezoeker adres. Selecteer deze parameters net als alle doelwaarden.

   >[!NOTE]
   >IP van een bezoeker adres wordt overgegaan met een brievenbusverzoek, eens per bezoek (zitting), om geo richtende parameters voor die bezoeker op te lossen.

1. Selecteer de operator als **overeenkomend** en geef een geschikte waarde op (bijvoorbeeld (Californië) en **Sla** uw wijzigingen op. Geef in ons geval de naam van het frame op.

   ![Adobe Target- Geo-regel](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >U kunt veelvoudige regels hebben die aan een publiek worden toegewezen.

1. Herhaal stap 6-9 om een publiek voor de andere staten te maken.

   ![Adobe Target- WKND-publiek](assets/personalization-use-case-1/adobe-target-audiences-50.png)

Op dit moment hebben we met succes een publiek gemaakt voor alle bezoekers van de WKND-site in verschillende staten in de Verenigde Staten van Amerika en hebben we ook de bijbehorende HTML-aanbieding voor elke staat. Laten we nu een Experience Targeting-activiteit maken om het publiek te richten op een overeenkomstige aanbieding voor de startpagina van de WKND-site.

### Een activiteit maken met Geo-gerichte

1. Navigeer vanuit uw Adobe Target-venster naar het tabblad **Activiteiten** .
1. Klik op Activiteit **** maken en selecteer het type activiteit **bij** beleving.
1. Selecteer het **kanaal van het Web** en kies de **Visuele Composer** van de Ervaring.
1. Ga URL **van de** Activiteit in en klik **daarna** om Visual Experience Composer te openen.

   Introductiepagina van WKND-site: http://localhost:4503/content/wknd/en.html

   ![Gericht op ervaring](assets/personalization-use-case-1/target-activity.png)

1. Om Composer **van** Visuele Ervaring te laden, laat **Toestaan de Onveilige manuscripten** van de Lading op uw browser toe en laadt uw pagina opnieuw.

   ![Gericht op ervaring](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Merk op de WKND homepage van de Plaats open in de redacteur van Composer van de Visuele Ervaring.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Als u een publiek aan uw VEC wilt toevoegen, klikt u op Ervaring **toevoegen die gericht** is onder Soorten publiek, selecteert u het WKND-Californië-publiek en klikt u op **Volgende**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Klik op de WKND-sitepagina in VEC, selecteer het HTML-element om de aanbieding voor WKND-Californië-publiek toe te voegen, kies **Vervangen door** optie en selecteer vervolgens de **HTML-aanbieding**.

   ![Gericht op ervaring](assets/personalization-use-case-1/vec-selecting-div.png)

1. Selecteer de **WKND SkateFest California** HTML-aanbieding voor het **WKND-Californische** publiek in de aanbieding, selecteer UI en klik op **Gereed**.
1. Nu moet u de **WKND SkateFest California** HTML-aanbieding kunnen zien die aan uw WKND-sitepagina is toegevoegd voor het WKND-Californische publiek.
1. Herhaal stap 7-10 om de Ervaring die voor de andere staten richt toe te voegen en de overeenkomstige aanbieding van HTML te kiezen.
1. Klik op **Volgende** om door te gaan en u ziet een toewijzing voor Soorten publiek aan Ervaringen.
1. Klik op **Volgende** om naar Doelstellingen en instellingen te gaan.
1. Kies uw rapporteringsbron en identificeer een primair doel voor uw activiteit. Voor ons Scenario, selecteren de Rapporterende Bron als **Adobe Target**, die activiteit als **Omzetting**, actie zoals bekeken een pagina, en URL die aan de pagina van Details WKND SkateFest richten meten.

   ![Doel en doel - Doel](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >U kunt ook Adobe Analytics kiezen als rapportagebron.

1. Houd de muisaanwijzer boven de huidige naam van de activiteit en u kunt de naam ervan wijzigen in **WKND SkateFest - VS**. Vervolgens **slaat en sluit** u de wijzigingen op.
1. Zorg ervoor dat u uw activiteit **activeert** via het scherm Activiteitsgegevens.

   ![Activiteit activeren](assets/personalization-use-case-1/activate-activity.png)

1. Uw WKND SkateFest-campagne is nu live voor alle bezoekers van de WKND-site.
1. Navigeer naar de startpagina [van de](http://localhost:4503/content/wknd/en.html)WKND-site en u zou de WKND SkateFest-aanbieding moeten kunnen zien die is gebaseerd op uw geo-locatie (*status: Californië*).

   ![Activiteit QA](assets/personalization-use-case-1/wknd-california.png)

### Doelactiviteit QA

1. Onder de Details van de **Activiteit > het lusje van het Overzicht** , klik op de knoop van QA **van de** Activiteit, en u kunt de directe verbinding QA aan al uw ervaringen krijgen.

   ![Activiteit QA](assets/personalization-use-case-1/activity-qa.png)

1. Navigeer naar de startpagina [van de](http://localhost:4503/content/wknd/en.html)WKND-site en u zou de WKND SkateFest-aanbieding moeten kunnen zien op basis van uw geo-locatie (status).
1. Bekijk de onderstaande video om te begrijpen hoe een aanbieding op uw pagina wordt geleverd, hoe u de reactietkens kunt aanpassen en hoe u een kwaliteitscontrole kunt uitvoeren.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Samenvatting

In dit hoofdstuk, kon een inhoudsredacteur al inhoud tot stand brengen om de campagne WKND SkateFest binnen Adobe Experience Manager te steunen en het uit te voeren naar Adobe Target als Aanbiedingen van HTML, voor het creëren van de Gerichte Ervaring, die van gebruikers wordt gebaseerd.
