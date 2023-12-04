---
title: Personalisatie met Adobe Target
description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 165
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '580'
ht-degree: 0%

---

# Aanpassing van de volledige Web-pagina Ervaringen die Adobe Target gebruiken

In het vorige hoofdstuk, leerden wij hoe te om een geo-plaats gebaseerde activiteit tot stand te brengen binnen Adobe Target gebruikend inhoud die als Fragments van de Ervaring wordt gecreeerd en uit AEM als Aanbiedingen van de HTML wordt uitgevoerd.

In dit hoofdstuk onderzoeken we het maken van activiteiten om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.

## Overzicht van scenario

De WKND-site heeft zijn homepage opnieuw ontworpen en wil de bezoekers van hun huidige homepage omleiden naar de nieuwe homepage. Tegelijkertijd moet u ook begrijpen hoe de vernieuwde homepage de betrokkenheid en inkomsten van gebruikers helpt verbeteren. Als markeerteken hebt u de taak gekregen om een activiteit te maken om de bezoekers om te leiden naar de nieuwe startpagina. Laten we de homepage van de WKND-site verkennen en leren hoe we een activiteit kunnen maken met Adobe Target.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target / Optimalisatieteam)

### Startpagina WKND-site

![AEM doelscenario 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Vereisten

* **AEM**
   * [AEM auteur- en publicatieexemplaar](./implementation.md#getting-aem) uitgevoerd op respectievelijk localhost 4502 en 4503.
   * [AEM geïntegreerd met Adobe Target met Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud voorzien van de volgende oplossingen
      * [Adobe Target](https://experiencecloud.adobe.com)

## Activiteiten van de inhoudseditor

1. De Marketer start de discussie over het opnieuw ontwerpen van de WKND-introductiepagina met AEM Content Editor en geeft details over de vereisten.
   * ***Vereiste*** : Introductiepagina van WKND-site opnieuw ontwerpen met op kaarten gebaseerd ontwerp.
2. Op basis van de vereisten maakt AEM Inhoudseditor vervolgens een nieuwe WKND-homepage met een op kaarten gebaseerd ontwerp en publiceert de nieuwe homepage.

## Marktactiviteiten

1. Marketer maakt een A/B-doelactiviteit met de omleidingsaanbieding als een Experience en 100% websiteverkeer toegewezen aan de nieuwe homepage met het succesdoel en de metriek toegevoegd.
   1. Navigeer vanuit uw Adobe Target-venster naar **Activiteiten** tab.
   2. Klikken **Activiteit maken** en selecteer het type activiteit als **A/B-test**
      ![Adobe Target - Activiteiten maken](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecteer de **Web** kanaal en kies **Visual Experience Composer**.
   4. Voer de **URL van activiteit** en klik op **Volgende** om Visual Experience Composer te openen.
      ![Adobe Target - Activiteiten maken](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Voor **Visual Experience Composer** om te laden, inschakelen **Onveilige scripts laden** op uw browser en laad de pagina opnieuw.
      ![Gericht op ervaring](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Merk op de WKND homepage van de Plaats open in de redacteur van Composer van de Visuele Ervaring.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Overslaan **Ervaring B** en selecteert u andere opties.
      ![Ervaring B](assets/personalization-use-case-2/redirect-url.png)
   8. Selecteren **Omleiden naar URL** en voert u de URL in naar de nieuwe WKND-startpagina. http://localhost:4503/content/wknd/en1.html)
      ![Ervaring B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Opslaan** uw wijzigingen aanbrengt en verdergaat met de volgende stappen van Activity Creation.
   10. Selecteer de **Methode voor verkeerstoewijzing** als handmatig en 100% verkeer toewijzen aan **Ervaring B**.
      ![Ervaring B Verkeer](assets/personalization-use-case-2/traffic.png)
   11. Klik op **Next**.
   12. Verlenen **Goederenstatistieken** voor uw Activiteit en sparen en sluit uw Test A/B.
      ![Metrisch testdoel A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Geef een naam op (**WKND-startpagina opnieuw ontwerpen**) voor uw activiteit en sla uw wijzigingen op.
   14. Zorg ervoor dat in het scherm Activiteitsdetails **Activeren** uw activiteit.
      ![Activiteit activeren](assets/personalization-use-case-2/ab-activate.png)
   15. Navigeer naar de startpagina van WKND (http://localhost:4503/content/wknd/en.html) en u wordt omgeleid naar de opnieuw ontworpen startpagina van de WKND-site (http://localhost:4503/content/wknd/en1.html).
      ![WKND-startpagina opnieuw ontworpen](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Samenvatting

In dit hoofdstuk kan een markeerteken een activiteit maken om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met behulp van Adobe Target.
