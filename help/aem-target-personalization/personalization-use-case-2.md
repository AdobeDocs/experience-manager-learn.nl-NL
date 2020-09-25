---
title: Personalisatie met Adobe Target
seo-title: Personalisatie met Adobe Target
description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Target.
seo-description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Target.
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---


# Aanpassing van de volledige Web-pagina Ervaringen die Adobe Target gebruiken

In het vorige hoofdstuk hebben we geleerd hoe we een op geolocatie gebaseerde activiteit in Adobe Target kunnen maken met inhoud die is gemaakt als Experience Fragments en geëxporteerd van AEM als HTML-aanbiedingen.

In dit hoofdstuk onderzoeken we het maken van activiteiten om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.

## Overzicht van scenario

De WKND-site heeft zijn homepage opnieuw ontworpen en wil de bezoekers van hun huidige homepage omleiden naar de nieuwe homepage. Tegelijkertijd moet u ook begrijpen hoe de vernieuwde homepage de betrokkenheid en inkomsten van gebruikers helpt verbeteren. Als markeerteken hebt u de taak gekregen om een activiteit te maken om de bezoekers om te leiden naar de nieuwe startpagina. Laten we de homepage van de WKND-site verkennen en leren hoe we een activiteit kunnen maken met Adobe Target.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target/Optimization Team)

### Startpagina WKND-site

![AEM doelscenario 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Vereisten

* **AEM**
   * [AEM auteur- en publicatieexemplaar](./implementation.md#getting-aem) dat wordt uitgevoerd op respectievelijk localhost 4502 en 4503.
   * [AEM geïntegreerd met Adobe Target met Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Target](https://experiencecloud.adobe.com)

## Activiteiten van de inhoudseditor

1. De Marketer start de discussie over het opnieuw ontwerpen van de WKND-introductiepagina met AEM Content Editor en geeft details over de vereisten.
   * ***Voorschrift*** : Introductiepagina van WKND-site opnieuw ontwerpen met op kaarten gebaseerd ontwerp.
2. Op basis van de vereisten maakt AEM Inhoudseditor vervolgens een nieuwe WKND-homepage met een op kaarten gebaseerd ontwerp en publiceert de nieuwe homepage.

## Marktactiviteiten

1. Marketer maakt een A/B-doelactiviteit met de omleidingsaanbieding als een Experience en 100% websiteverkeer toegewezen aan de nieuwe homepage met het succesdoel en de metriek toegevoegd.
   1. Navigeer vanuit uw Adobe Target-venster naar het tabblad **Activiteiten** .
   2. Klik op de knop Activiteit **** maken en selecteer het type activiteit als **A/B-test**

      ![Adobe Target - Activiteiten maken](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecteer het **kanaal van het Web** en kies de **Visuele Composer** van de Ervaring.
   4. Ga URL **van de** Activiteit in en klik **daarna** om Visual Experience Composer te openen.
      ![Adobe Target - Activiteiten maken](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Om Composer **van** Visuele Ervaring te laden, laat **Toestaan de Onveilige manuscripten** van de Lading op uw browser toe en laadt uw pagina opnieuw.
      ![Gericht op ervaring](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Merk op de WKND homepage van de Plaats open in de redacteur van Composer van de Visuele Ervaring.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Houd de muisaanwijzer boven **Experience B** en selecteer andere opties voor de weergave.
      ![Ervaring B](assets/personalization-use-case-2/redirect-url.png)
   8. Selecteer de optie **Omleiden naar URL** en voer de URL in naar de nieuwe WKND-startpagina. (http://localhost:4503/content/wknd/en1.html)
      ![Ervaring B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Sla** uw wijzigingen op en ga verder met de volgende stappen voor het maken van activiteiten.
   10. Selecteer de Methode **van de Toewijzing van het** Verkeer als hand en wijs 100% verkeer aan **Ervaring B**toe.
      ![Ervaring B Verkeer](assets/personalization-use-case-2/traffic.png)
   11. Klik op **Next**.
   12. Verstrek **Goal Metrics** voor uw Activiteit en sparen en sluit uw Test A/B.
      ![Metrisch testdoel A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Geef een naam op (**WKND Home Page Redesign**) voor uw activiteit en sla uw wijzigingen op.
   14. Zorg ervoor dat u uw activiteit **activeert** via het scherm Activiteitsgegevens.
      ![Activiteit activeren](assets/personalization-use-case-2/ab-activate.png)
   15. Navigeer naar de startpagina van WKND (http://localhost:4503/content/wknd/en.html) en u wordt omgeleid naar de opnieuw ontworpen startpagina van de WKND-site (http://localhost:4503/content/wknd/en1.html).
      ![WKND-startpagina opnieuw ontworpen](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Samenvatting

In dit hoofdstuk kan een markeerteken een activiteit maken om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.
