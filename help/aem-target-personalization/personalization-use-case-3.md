---
title: Personalization met Adobe Target Visual Experience Composer
description: Een end-to-end zelfstudie die toont om gepersonaliseerde ervaring tot stand te brengen en te leveren gebruikend Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 112
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 0%

---

# Personalization die Visual Experience Composer gebruikt

In dit hoofdstuk, zullen wij het creëren van ervaringen onderzoeken gebruikend **Visuele Composer van de Ervaring** door te slepen en te laten vallen, de lay-out en de inhoud van een Web-pagina van binnen Doel te ruilen.

## Overzicht van scenario

De WKND-homepage geeft lokale activiteiten weer of het beste wat je kunt doen rond een stad in de vorm van kaartlay-outs. Als markator, bent u de taak toegewezen om de homepage te wijzigen, door de kaartlay-outs te herschikken om te zien hoe het gebruikersbetrokkenheid beïnvloedt en omzetting drijft.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **de Producent van de Inhoud/de Redacteur van de Inhoud** (Adobe Experience Manager)
* **Marketer** (het Team van Adobe Target / van de Optimalisering)

### Startpagina WKND-site

![ AEM Scenario 1 van het Doel ](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Vereisten

* **AEM**
   * [ AEM publiceer instantie ](./implementation.md#getting-aem) lopend op 4503
   * [AEM geïntegreerd met Adobe Target met tags](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisioned met [ Adobe Target ](https://experiencecloud.adobe.com)

## Marktactiviteiten

1. De Marketer maakt een A/B-doelactiviteit in Adobe Target.
   1. Van uw venster van Adobe Target, navigeer aan **Activiteiten** tabel.
   2. Klik **creeer de knoop van de Activiteit** en selecteer het activiteitstype als **Test A/B**
      ![ Adobe Target - creeer Activiteit ](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecteer het **kanaal van het 0} Web {en kies** Visuele Composer van de Ervaring **.**
   4. Ga **Activiteit URL** in en klik **daarna** om de Visuele Composer van de Ervaring te openen.
      ![ Adobe Target - creeer Activiteit ](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Voor **Visuele Composer van de Ervaring** om te laden, laat **toe Lading Onveilige manuscripten** op uw browser en herlaad uw pagina.
      ![ Ervaring richtend Activiteit ](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Merk op de WKND homepage van de Plaats open in de redacteur van Composer van de Visuele Ervaring.
      ![ VEC ](assets/personalization-use-case-2/vec.png)
   7. **Ervaring A** verstrekt de standaardWKND Homepage, en laat de inhoudslay-out voor **Ervaring B** uitgeven.
      ![ Ervaring B ](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klik op één van de container van de kaartlay-out (*Beste Roasters*) en selecteer **herschikt** optie.
      ![ Selectie van de Container ](assets/personalization-use-case-3/container-selection.png)
   9. Klik op de container die u opnieuw wilt rangschikken en sleep deze naar de gewenste locatie. Laat de *Beste Roasters* container van 1st rij 1st kolom aan 1st rij 3de kolom opnieuw rangschikken. Nu is de *Beste container van Roasters* naast *container van de Extracties van de Fotografie*.
      ![ het Wisselen van de Container ](assets/personalization-use-case-3/container-swap.png)
      **na Wisselen**
      ![ Gewisselde Container ](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Op dezelfde manier wijzigt u de rangschikking van posities voor de andere kaartcontainers.
      ![ Gewisselde Container ](assets/personalization-use-case-3/after-swap-all.png)
   11. Voeg ook een koptekst toe onder de carrouselcomponent en boven de kaartlay-out.
   12. Klik op de carrouselcontainer en selecteer **Inzet na > HTML** optie om HTML toe te voegen.
      ![ voeg Tekst ](assets/personalization-use-case-3/add-text.png) toe

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![ voeg Tekst ](assets/personalization-use-case-3/after-changes.png) toe
   13. Klik **daarna** om met uw activiteit verder te gaan.
   14. Selecteer de **Methode van de Toewijzing van het Verkeer** als hand en wijs 100% verkeer aan **Ervaring B** toe.
      ![ Ervaring B Verkeer ](assets/personalization-use-case-2/traffic.png)
   15. Klik op **Next**.
   16. Verstrek **Goal Metrics** voor uw Activiteit en sparen en sluit uw Test A/B.
      ![ Metrisch van het Doel van de Test A/B ](assets/personalization-use-case-2/goal-metric.png)
   17. Verstrek een naam (**WKND Vernieuwen de Homepage van het Huis**) voor uw Activiteit en sparen uw veranderingen.
   18. Van het scherm van de Details van de Activiteit, zorg ervoor **activeer** uw activiteit.
      ![ activeer Activiteit ](assets/personalization-use-case-3/save-activity.png)
   19. Navigeer naar de startpagina van WKND (http://localhost:4503/content/wknd/en.html) en u merkt de wijzigingen die we hebben toegevoegd aan de testactiviteit A/B van de WKND-startpagina vernieuwen.
      ![ Vernieuwde de Homepage van WKND ](assets/personalization-use-case-3/activity-result.png)
   20. Open uw browser console, en inspecteer het netwerklusje om doelreactie voor de Pagina van het Huis te zoeken WKND verfrist A/B de activiteit van de Test van de Test A.
      ![ Activiteit van het Netwerk ](assets/personalization-use-case-3/activity-result.png)

## Samenvatting

In dit hoofdstuk, kon een teller een ervaring tot stand brengen gebruikend Visual Experience Composer door te slepen en neer te zetten, de lay-out en de inhoud van een Web-pagina te ruilen zonder enige code te veranderen om een test in werking te stellen.
