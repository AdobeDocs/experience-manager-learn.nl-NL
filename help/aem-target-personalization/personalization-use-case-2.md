---
title: Personalisatie met Adobe Target
seo-title: Personalisatie met Adobe Target
description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Target.
seo-description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Target.
feature: Ervaringsfragmenten
topic: Personalisatie
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 1%

---


# Aanpassing van de volledige Web-pagina Ervaringen die Adobe Target gebruiken

In het vorige hoofdstuk hebben we geleerd hoe we een op geolocatie gebaseerde activiteit in Adobe Target kunnen maken met inhoud die is gemaakt als Experience Fragments en geëxporteerd van AEM als HTML-aanbiedingen.

In dit hoofdstuk onderzoeken we het maken van activiteiten om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.

## Overzicht van scenario

De WKND-site heeft zijn homepage opnieuw ontworpen en wil de bezoekers van hun huidige homepage omleiden naar de nieuwe homepage. Tegelijkertijd moet u ook begrijpen hoe de vernieuwde homepage de betrokkenheid en inkomsten van gebruikers helpt verbeteren. Als markeerteken hebt u de taak gekregen om een activiteit te maken om de bezoekers om te leiden naar de nieuwe startpagina. Laten we de homepage van de WKND-site verkennen en leren hoe we een activiteit kunnen maken met Adobe Target.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **Content Producer/Content Editor**  (Adobe Experience Manager)
* **Marketer**  (Adobe Target/Optimization Team)

### Startpagina WKND-site

![AEM doelscenario 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Vereisten

* **AEM**
   * [AEM auteur en publiceer ](./implementation.md#getting-aem) instormsessies op respectievelijk localhost 4502 en 4503.
   * [AEM geïntegreerd met Adobe Target met Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - <https://>`<yourcompany>`.ExperienceCloud.adobe.com
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Target](https://experiencecloud.adobe.com)

## Activiteiten van de inhoudseditor

1. De Marketer start de discussie over het opnieuw ontwerpen van de WKND-introductiepagina met AEM Content Editor en geeft details over de vereisten.
   * ***Voorschrift*** : Introductiepagina van WKND-site opnieuw ontwerpen met op kaarten gebaseerd ontwerp.
2. Op basis van de vereisten maakt AEM Inhoudseditor vervolgens een nieuwe WKND-homepage met een op kaarten gebaseerd ontwerp en publiceert de nieuwe homepage.

## Marktactiviteiten

1. Marketer maakt een A/B-doelactiviteit met de omleidingsaanbieding als een Experience en 100% websiteverkeer toegewezen aan de nieuwe homepage met het succesdoel en de metriek toegevoegd.
   1. Navigeer vanuit uw Adobe Target-venster naar **Activiteiten** tabblad.
   2. Klik op **Activiteit maken** en selecteer het type activiteit als **A/B Test**

      ![Adobe Target - Activiteiten maken](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecteer het kanaal **Web** en kies **Visual Experience Composer**.
   4. Voer de **Activiteit-URL** in en klik **Volgende** om de Visual Experience Composer te openen.
      ![Adobe Target - Activiteiten maken](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Als u **Visual Experience Composer** wilt laden, schakelt u **Onveilige scripts laden** op uw browser toestaan in en laadt u de pagina opnieuw.
      ![Gericht op ervaring](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Merk op de WKND homepage van de Plaats open in de redacteur van Composer van de Visuele Ervaring.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Houd de muisaanwijzer boven **Beleef B** en selecteer de gewenste weergaveopties.
      ![Ervaring B](assets/personalization-use-case-2/redirect-url.png)
   8. Selecteer **Omleiden aan URL** optie en ga URL aan de nieuwe WKND Homepage in. (http://localhost:4503/content/wknd/en1.html)
      ![Ervaring B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **** Sla uw wijzigingen op en ga verder met de volgende stappen voor het maken van activiteiten.
   10. Selecteer **Verkeerstoewijzingsmethode** als handmatige methode en wijs 100% verkeer toe aan **Experience B**.
      ![Ervaring B Verkeer](assets/personalization-use-case-2/traffic.png)
   11. Klik op **Next**.
   12. Verstrek **Goal Metrics** voor uw Activiteit en sparen en sluit uw Test A/B.
      ![Metrisch testdoel A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Geef een naam (**WKND Home Page Redesign**) op voor uw activiteit en sla uw wijzigingen op.
   14. Van het scherm van de Details van de Activiteit, zorg ervoor om **uw activiteit te activeren**.
      ![Activiteit activeren](assets/personalization-use-case-2/ab-activate.png)
   15. Navigeer naar de startpagina van WKND (http://localhost:4503/content/wknd/en.html) en u wordt omgeleid naar de opnieuw ontworpen startpagina van de WKND-site (http://localhost:4503/content/wknd/en1.html).
      ![WKND-startpagina opnieuw ontworpen](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Samenvatting

In dit hoofdstuk kan een markeerteken een activiteit maken om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.
