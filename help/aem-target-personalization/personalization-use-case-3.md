---
title: Personalisatie met Adobe Target Visual Experience Composer
seo-title: Personalisatie met Adobe Target Visual Experience Composer (VEC)
description: Een end-to-end zelfstudie die toont om gepersonaliseerde ervaring tot stand te brengen en te leveren gebruikend Adobe Target Visual Experience Composer (VEC).
seo-description: Een end-to-end zelfstudie die toont om gepersonaliseerde ervaring tot stand te brengen en te leveren gebruikend Adobe Target Visual Experience Composer (VEC).
feature: Ervaringsfragmenten
topic: Personalisatie
role: Developer
level: Intermediair
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '615'
ht-degree: 1%

---


# Personalisatie met behulp van Visual Experience Composer

In dit hoofdstuk, zullen wij het creëren van ervaringen gebruikend **Visual Experience Composer** door te slepen en te laten vallen, de lay-out en de inhoud van een Web-pagina van binnen Doel te ruilen.

## Overzicht van scenario

De WKND-homepage geeft lokale activiteiten weer of het beste wat je kunt doen rond een stad in de vorm van kaartlay-outs. Als markator, bent u de taak toegewezen om de homepage te wijzigen, door de kaartlay-outs te herschikken om te zien hoe het gebruikersbetrokkenheid beïnvloedt en omzetting drijft.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **Content Producer/Content Editor**  (Adobe Experience Manager)
* **Marketer**  (Adobe Target/Optimization Team)

### Startpagina WKND-site

![AEM doelscenario 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Vereisten

* **AEM**
   * [AEM publiceer ](./implementation.md#getting-aem) instormsessies op 4503
   * [AEM geïntegreerd met Adobe Target met Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - <https://>`<yourcompany>`.ExperienceCloud.adobe.com
   * Experience Cloud voorzien van [Adobe Target](https://experiencecloud.adobe.com)

## Marktactiviteiten

1. De Marketer maakt een A/B-doelactiviteit in Adobe Target.
   1. Navigeer vanuit uw Adobe Target-venster naar **Activiteiten** tabblad.
   2. Klik op **Activiteit maken** en selecteer het type activiteit als **A/B Test**

      ![Adobe Target - Activiteiten maken](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecteer het **Web** kanaal en kies **Visual Experience Composer**.
   4. Voer de **Activiteit-URL** in en klik **Volgende** om de Visual Experience Composer te openen.
      ![Adobe Target - Activiteiten maken](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Als u **Visual Experience Composer** wilt laden, schakelt u **Onveilige scripts laden** op uw browser toestaan in en laadt u de pagina opnieuw.
      ![Gericht op ervaring](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Merk op de WKND homepage van de Plaats open in de redacteur van Composer van de Visuele Ervaring.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **De ervaring** verstrekt de standaardWKND Homepage, en laten wij de inhoudslay-out voor  **Ervaring B** uitgeven.
      ![Ervaring B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klik op een van de kaartlay-outcontainers (*Beste roasters*) en selecteer **Herschikken** optie.
      ![Containerselectie](assets/personalization-use-case-3/container-selection.png)
   9. Klik op de container die u opnieuw wilt rangschikken en sleep deze naar de gewenste locatie. Laten wij de *Beste container Roasters* van 1e rij 1st kolom aan 1e rij 3de kolom opnieuw rangschikken. De *Best Roasters*-container komt nu naast de container *Photography Exhiisions*.
      ![Container omwisselen](assets/personalization-use-case-3/container-swap.png)

      **Na omwisselen**
      ![Container omgewisseld](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Op dezelfde manier wijzigt u de rangschikking van posities voor de andere kaartcontainers.
      ![Container omgewisseld](assets/personalization-use-case-3/after-swap-all.png)
   11. Voeg ook een koptekst toe onder de carrouselcomponent en boven de kaartlay-out.
   12. Klik op de carrouselcontainer en selecteer de optie **Inzet na > HTML** om HTML toe te voegen.
      ![Tekst toevoegen](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Tekst toevoegen](assets/personalization-use-case-3/after-changes.png)
   13. Klik **Volgende** om door te gaan met uw activiteit.
   14. Selecteer **Verkeerstoewijzingsmethode** als handmatige methode en wijs 100% verkeer toe aan **Experience B**.
      ![Ervaring B Verkeer](assets/personalization-use-case-2/traffic.png)
   15. Klik op **Next**.
   16. Verstrek **Goal Metrics** voor uw Activiteit en sparen en sluit uw Test A/B.
      ![Metrisch testdoel A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Geef een naam (**WKND-startpagina vernieuwen**) op voor uw activiteit en sla uw wijzigingen op.
   18. Van het scherm van de Details van de Activiteit, zorg ervoor om **uw activiteit te activeren**.
      ![Activiteit activeren](assets/personalization-use-case-3/save-activity.png)
   19. Navigeer naar de startpagina van WKND (http://localhost:4503/content/wknd/en.html) en u merkt de wijzigingen die we hebben toegevoegd aan de testactiviteit A/B van de WKND-startpagina vernieuwen.
      ![WKND-startpagina vernieuwd](assets/personalization-use-case-3/activity-result.png)
   20. Open uw browser console, en inspecteer het netwerklusje om doelreactie voor de Pagina van het Huis te zoeken WKND verfrist A/B de activiteit van de Test van de Pagina A/B.
      ![Netwerkactiviteit](assets/personalization-use-case-3/activity-result.png)

## Samenvatting

In dit hoofdstuk, kon een teller een ervaring tot stand brengen gebruikend Visual Experience Composer door te slepen en neer te zetten, de lay-out en de inhoud van een Web-pagina te ruilen zonder enige code te veranderen om een test in werking te stellen.
