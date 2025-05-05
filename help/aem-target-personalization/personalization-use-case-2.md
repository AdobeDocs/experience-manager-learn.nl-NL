---
title: Personalization met Adobe Target
description: Een end-to-end zelfstudie waarin wordt getoond hoe u persoonlijke ervaringen kunt creëren en leveren met Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 114
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# Personalization van volledige webpagina&#39;s met Adobe Target

In het vorige hoofdstuk, leerden wij hoe te om een geo-plaats gebaseerde activiteit tot stand te brengen binnen Adobe Target gebruikend inhoud die als Fragments van de Ervaring wordt gecreeerd en uit AEM als Aanbiedingen van de HTML wordt uitgevoerd.

In dit hoofdstuk onderzoeken we het maken van activiteiten om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.

## Overzicht van scenario

De WKND-site heeft zijn homepage opnieuw ontworpen en wil de bezoekers van hun huidige homepage omleiden naar de nieuwe homepage. Tegelijkertijd moet u ook begrijpen hoe de vernieuwde homepage de betrokkenheid en inkomsten van gebruikers helpt verbeteren. Als markeerteken hebt u de taak gekregen om een activiteit te maken om de bezoekers om te leiden naar de nieuwe startpagina. Laten we de homepage van de WKND-site verkennen en leren hoe we een activiteit kunnen maken met Adobe Target.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **de Producent van de Inhoud/de Redacteur van de Inhoud** (Adobe Experience Manager)
* **Marketer** (het Team van Adobe Target / van de Optimalisering)

### WKND-startpagina

![ AEM Scenario 1 van het Doel ](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Vereisten

* **AEM**
   * [ AEM auteur en publiceer instantie ](./implementation.md#getting-aem) lopend op localhost 4502 en 4503.
   * [AEM geïntegreerd met Adobe Target met tags](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud voorzien van de volgende oplossingen
      * [ Adobe Target ](https://experiencecloud.adobe.com)

## Inhoudsbewerkingsactiviteiten

1. De Marketer start de discussie over het opnieuw ontwerpen van de WKND-introductiepagina met AEM Content Editor en geeft details over de vereisten.
   * ***Vereiste*** : De Pagina van het Huis van WKND van de Plaats met op kaart-gebaseerd ontwerp opnieuw ontwerpen.
2. Op basis van de vereisten maakt AEM Inhoudseditor vervolgens een nieuwe WKND-homepage met een op kaarten gebaseerd ontwerp en publiceert de nieuwe homepage.

## Marktactiviteiten

1. Marketer maakt een A/B-doelactiviteit met de omleidingsaanbieding als een Experience en 100% websiteverkeer toegewezen aan de nieuwe homepage met het succesdoel en de metriek toegevoegd.
   1. Van uw venster van Adobe Target, navigeer aan **Activiteiten** tabel.
   2. Klik **creeer de knoop van de Activiteit** en selecteer het activiteitstype als **Test A/B**

      ![ Adobe Target - creeer Activiteit ](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecteer het **kanaal van het 0&rbrace; Web &lbrace;en kies** Visuele Composer van de Ervaring **.**
   4. Ga **Activiteit URL** in en klik **daarna** om de Visuele Composer van de Ervaring te openen.

      ![ Adobe Target - creeer Activiteit ](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Voor **Visuele Composer van de Ervaring** om te laden, laat **toe Lading Onveilige manuscripten** op uw browser en herlaad uw pagina.

      ![ Ervaring richtend Activiteit ](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Merk op de WKND homepage van de Plaats open in de redacteur van Composer van de Visuele Ervaring.

      ![ VEC ](assets/personalization-use-case-2/vec.png)
   7. Beweeg over **Ervaring B** en selecteer mening andere opties.

      ![ Ervaring B ](assets/personalization-use-case-2/redirect-url.png)
   8. Selecteer **Redirect aan URL** optie en ga URL aan de nieuwe WKND Homepage in. http://localhost:4503/content/wknd/en1.html)

      ![ Ervaring B ](assets/personalization-use-case-2/redirect-url-2.png)
   9. **sparen** uw veranderingen en ga met de volgende stappen van de Making van de Activiteit verder.
   10. Selecteer de **Methode van de Toewijzing van het Verkeer** als hand en wijs 100% verkeer aan **Ervaring B** toe.

       ![ Ervaring B Verkeer ](assets/personalization-use-case-2/traffic.png)
   11. Klik op **Next**.
   12. Verstrek **Goal Metrics** voor uw Activiteit en sparen en sluit uw Test A/B.

       ![ Metrisch van het Doel van de Test A/B ](assets/personalization-use-case-2/goal-metric.png)
   13. Verstrek een naam (**WKND het Herontwerp van de Homepage van het Huis**) voor uw Activiteit en sla uw veranderingen op.
   14. Van het scherm van de Details van de Activiteit, zorg ervoor **activeer** uw activiteit.

       ![ activeer Activiteit ](assets/personalization-use-case-2/ab-activate.png)
   15. Navigeer naar de startpagina van WKND (http://localhost:4503/content/wknd/en.html) en u wordt omgeleid naar de opnieuw ontworpen startpagina van de WKND-site (http://localhost:4503/content/wknd/en1.html).

      ![ WKND Homepage opnieuw ontworpen ](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Samenvatting

In dit hoofdstuk kan een markeerteken een activiteit maken om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met behulp van Adobe Target.
