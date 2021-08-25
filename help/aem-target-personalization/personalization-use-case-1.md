---
title: Personalisatie met AEM Experience Fragments en Adobe Target
seo-title: Personalization using Adobe Experience Manager (AEM) Experience Fragments and Adobe Target
description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Experience Manager Experience Fragments en Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Experience Manager Experience Fragments and Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1692'
ht-degree: 0%

---


# Personalisatie met AEM Experience Fragments en Adobe Target

Met de mogelijkheid om AEM Experience Fragments naar Adobe Target te exporteren als HTML-aanbiedingen, kunt u het gebruiksgemak en de kracht van AEM combineren met krachtige mogelijkheden voor Automated Intelligence (AI) en Machine Learning (ML) in Target om ervaringen op schaal te testen en te personaliseren.

AEM brengt al uw inhoud en middelen in een centrale plaats samen om uw verpersoonlijkingsstrategie te voeden. Met AEM kunt u eenvoudig inhoud voor desktops, tablets en mobiele apparaten op één locatie maken zonder dat u code hoeft te schrijven. Het is niet nodig om pagina&#39;s te maken voor elk apparaat. AEM past automatisch elke ervaring aan met uw inhoud.

Met Doel kunt u gepersonaliseerde ervaringen op schaal bieden op basis van een combinatie van op regels gebaseerde en op AI gebaseerde methoden voor het leren van machines die gedrags-, context- en offlinevariabelen bevatten.  Met Doel kunt u eenvoudig A/B- en MVT-activiteiten (Multivariate) instellen en uitvoeren om de beste aanbiedingen, inhoud en ervaringen te bepalen.

De fragmenten van de ervaring vertegenwoordigen een enorme stap voorwaarts om inhoudmakers met verkopers te verbinden die bedrijfsresultaten drijven gebruikend Doel.

## Overzicht van scenario

De plaats van WKND is van plan om een **SkateFest Uitdaging** over Amerika door hun website aan te kondigen en zou hun plaatsgebruikers willen hebben omhoog voor de audio verschijnen die in elke staat wordt geleid. Als teller, bent u de taak toegewezen om een campagne op de WKND homepage van de plaats in werking te stellen, met banners berichten relevant voor de plaats van de gebruikers en een verbinding aan de pagina van gebeurtenisdetails. Laten we de homepage van de WKND-site verkennen en leren hoe we een persoonlijke ervaring voor een gebruiker kunnen maken en leveren op basis van zijn/haar huidige locatie.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **Content Producer/Content Editor**  (Adobe Experience Manager)
* **Marketer**  (Adobe Target/Optimization Team)

### Vereisten

* **AEM**
   * [AEM auteur en publiceer ](./implementation.md#getting-aem) instormsessies op respectievelijk localhost 4502 en 4503.
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
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
   * [Het ](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) publiek bepaalt het doel voor uw activiteit en wordt overal gebruikt waar het richten beschikbaar is. Doelpubliek is een gedefinieerde set bezoekerscriteria. Aanbiedingen kunnen worden gericht op specifieke doelgroepen (of segmenten). Alleen bezoekers die bij dat publiek horen, zien de ervaring die voor hen is bedoeld.  U kunt bijvoorbeeld een aanbieding doen aan een publiek dat is samengesteld uit bezoekers die een bepaalde browser gebruiken of van een specifieke geo-locatie.
   * Een [Aanbieding](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) is de inhoud die op uw webpagina&#39;s wordt weergegeven tijdens campagnes of activiteiten. Wanneer u uw webpagina&#39;s test, meet u het succes van elke ervaring met verschillende aanbiedingen op uw locaties. Een voorstel kan verschillende typen inhoud bevatten, zoals:
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

1. Naar uw organisaties navigeren [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. Meld u aan met uw Adobe ID en zorg ervoor dat u zich in de juiste organisatie bevindt.
1. Klik vanuit de oplossingsschakelaar op **Doel** en **launch** Adobe Target.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Navigeer naar het tabblad **Aanbiedingen** en zoek naar &quot;WKND&quot;-aanbiedingen. U moet de lijst met Experience Fragments-variaties kunnen bekijken, geëxporteerd van AEM als HTML-aanbiedingen. Elke aanbieding komt overeen met een status. Bijvoorbeeld, *WKND SkateFest Californië* is de aanbieding die aan een bezoeker van de Plaats WKND van Californië wordt gediend.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Klik in de hoofdnavigatie op **Soorten publiek**.

   Een Marketer moet 50 verschillende soorten publiek creëren voor bezoekers van de WKND-site die uit elk land in de Verenigde Staten van Amerika komen.

1. Als u een publiek wilt maken, klikt u op **De knop Publiek maken** en geeft u een naam op voor uw publiek.

   **Indeling Audience Name: WKND-\&lt;>state *\>***

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Klik **Regel toevoegen > Geo**.
1. Klik **Selecteer**, dan selecteer één van de volgende opties:
   * Land
   * **Staat** *(Selecteer Staat voor de Campagne van de SlaatFest van de Plaats WKND)*
   * Plaats
   * Postcode
   * Breedte
   * Lengtegraad
   * DMA
   * Mobiele vervoerder

   **Geo**  - Gebruik doelgroepen voor gebruikers op basis van hun geografische locatie, waaronder hun land, staat/provincie, stad, postcode, DMA of mobiele luchtvaartmaatschappij. Met Geolocation-parameters kunt u activiteiten en ervaringen richten op basis van de geografie van uw bezoekers. Dit gegeven wordt verzonden met elk verzoek van het Doel en is gebaseerd op het IP van de bezoeker adres. Selecteer deze parameters net als alle doelwaarden.

   >[!NOTE]
   >IP van een bezoeker adres wordt overgegaan met een brievenbusverzoek, eens per bezoek (zitting), om geo richtende parameters voor die bezoeker op te lossen.

1. Selecteer de operator als **overeenkomsten** en geef een geschikte waarde op (bijvoorbeeld: Californië) en **Sla uw wijzigingen op.** Geef in ons geval de naam van het frame op.

   ![Adobe Target- Geo-regel](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >U kunt veelvoudige regels hebben die aan een publiek worden toegewezen.

1. Herhaal stap 6-9 om een publiek voor de andere staten te maken.

   ![Adobe Target- WKND-publiek](assets/personalization-use-case-1/adobe-target-audiences-50.png)

Op dit moment hebben we met succes een publiek gemaakt voor alle bezoekers van de WKND-site in verschillende staten in de Verenigde Staten van Amerika en hebben we ook de bijbehorende HTML-aanbieding voor elke staat. Laten we nu een Experience Targeting-activiteit maken om het publiek te richten op een overeenkomstige aanbieding voor de startpagina van de WKND-site.

### Een activiteit maken met Geo-gerichte

1. Navigeer vanuit uw Adobe Target-venster naar **Activiteiten** tabblad.
1. Klik **Activiteit maken** en selecteer het type activiteit **Beleving gericht**.
1. Selecteer het kanaal **Web** en kies **Visual Experience Composer**.
1. Voer de **Activiteit-URL** in en klik **Volgende** om de Visual Experience Composer te openen.

   Introductiepagina van WKND-site: http://localhost:4503/content/wknd/en.html

   ![Gericht op ervaring](assets/personalization-use-case-1/target-activity.png)

1. Als u **Visual Experience Composer** wilt laden, schakelt u **Onveilige scripts laden** op uw browser toestaan in en laadt u de pagina opnieuw.

   ![Gericht op ervaring](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Merk op de WKND homepage van de Plaats open in de redacteur van Composer van de Visuele Ervaring.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Om een publiek aan uw VEC toe te voegen, klik op **Voeg Ervaring het richten** onder Soorten publiek toe, en selecteer het publiek WKND-Californië en klik **Volgende**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Klik op de WKND-sitepagina in VEC, selecteer het HTML-element om de aanbieding voor WKND-Californië-publiek toe te voegen, en kies **Vervangen door** en selecteer vervolgens de **HTML-aanbieding**.

   ![Gericht op ervaring](assets/personalization-use-case-1/vec-selecting-div.png)

1. Selecteer **WKND SkateFest California** HTML-aanbieding voor het **WKND-California** publiek van de aanbieding uitgezochte UI en klik **Done**.
1. U zou nu **WKND SkateFest Californië** HTML- Aanbieding moeten kunnen zien die aan uw WKND-plaatspagina voor het publiek WKND-Californië wordt toegevoegd.
1. Herhaal stap 7-10 om de Ervaring die voor de andere staten richt toe te voegen en de overeenkomstige aanbieding van HTML te kiezen.
1. Klik **Volgende** om verder te gaan, en u kunt een afbeelding voor Soorten publiek aan Ervaringen zien.
1. Klik **Volgende** om naar Doelstellingen en Instellingen te gaan.
1. Kies uw rapporteringsbron en identificeer een primair doel voor uw activiteit. Voor ons Scenario, selecteren de Rapporterende Bron als **Adobe Target**, die activiteit als **Conversie**, actie zoals bekeken een pagina, en URL die aan de pagina van Details WKND SkateFest richten.

   ![Doel en doel - Doel](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >U kunt ook Adobe Analytics kiezen als rapportagebron.

1. Houd de cursor boven de huidige naam van de activiteit en u kunt de naam ervan wijzigen in **WKND SkateFest - USA**, en vervolgens **Opslaan en Sluiten**.
1. Van het scherm van de Details van de Activiteit, zorg ervoor om **uw activiteit te activeren**.

   ![Activiteit activeren](assets/personalization-use-case-1/activate-activity.png)

1. Uw WKND SkateFest-campagne is nu live voor alle bezoekers van de WKND-site.
1. Navigeer naar [Startpagina van de Plaats van WKND](http://localhost:4503/content/wknd/en.html), en u zou de WKND SkateFest die Aanbieding moeten kunnen zien van uw geo-plaats (*staat wordt gebaseerd: Californië*).

   ![Activiteit QA](assets/personalization-use-case-1/wknd-california.png)

### Doelactiviteit QA

1. Onder **Activiteitsdetails > Overzicht** tabel, klik op **Activiteit QA** knoop, en u kunt de directe verbinding QA aan al uw ervaringen krijgen.

   ![Activiteit QA](assets/personalization-use-case-1/activity-qa.png)

1. Navigeer naar [Startpagina van de Plaats van WKND](http://localhost:4503/content/wknd/en.html), en u zou de WKND SkateFest die Aanbieding van WKND moeten kunnen zien van uw geo-plaats (staat) wordt gebaseerd.
1. Bekijk de onderstaande video om te begrijpen hoe een aanbieding op uw pagina wordt geleverd, hoe u de reactietkens kunt aanpassen en hoe u een kwaliteitscontrole kunt uitvoeren.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Samenvatting

In dit hoofdstuk, kon een inhoudsredacteur al inhoud tot stand brengen om de campagne WKND SkateFest binnen Adobe Experience Manager te steunen en het uit te voeren naar Adobe Target als Aanbiedingen van HTML, voor het creëren van de Gerichte Ervaring, die van gebruikers wordt gebaseerd.
