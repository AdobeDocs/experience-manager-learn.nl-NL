---
title: Personalisatie met Adobe Target Visual Experience Composer
seo-title: Personalisatie met Adobe Target Visual Experience Composer (VEC)
description: Een end-to-end zelfstudie die toont om gepersonaliseerde ervaring tot stand te brengen en te leveren gebruikend Adobe Target Visual Experience Composer (VEC).
seo-description: Een end-to-end zelfstudie die toont om gepersonaliseerde ervaring tot stand te brengen en te leveren gebruikend Adobe Target Visual Experience Composer (VEC).
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 0%

---


# Personalisatie met behulp van Visual Experience Composer

In dit hoofdstuk, zullen wij het creëren van ervaringen gebruikend Composer **van de Ervaring van** Visuele door te slepen en te laten vallen, het ruilen, en het wijzigen van de lay-out en de inhoud van een Web-pagina van binnen Doel onderzoeken.

## Overzicht van scenario

De WKND-homepage geeft lokale activiteiten weer of het beste wat je kunt doen rond een stad in de vorm van kaartlay-outs. Als markator, bent u de taak toegewezen om de homepage te wijzigen, door de kaartlay-outs te herschikken om te zien hoe het gebruikersbetrokkenheid beïnvloedt en omzetting drijft.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target/Optimization Team)

### Startpagina WKND-site

![AEM doelscenario 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Vereisten

* **AEM**
   * [AEM publicatie-instantie](./implementation.md#getting-aem) die wordt uitgevoerd op 4503
   * [AEM geïntegreerd met Adobe Target met Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud voorzien van [Adobe Target](https://experiencecloud.adobe.com)

## Marktactiviteiten

1. De Marketer maakt een A/B-doelactiviteit in Adobe Target.
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
   7. **Experience A** biedt de standaard WKND-startpagina en laten we de inhoudslay-out voor **Experience B**bewerken.
      ![Ervaring B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klik op een van de kaartlay-outcontainers (*Beste broodjes*) en selecteer **Opnieuw rangschikken** optie.
      ![Containerselectie](assets/personalization-use-case-3/container-selection.png)
   9. Klik op de container die u opnieuw wilt rangschikken en sleep deze naar de gewenste locatie. Laten wij de *Beste container van Roasters* van 1e rij 1e kolom aan 1e rij 3de kolom herschikken. De container *Best Roasters* staat nu naast de container *Photography Exhiisions* .
      ![Container omwisselen](assets/personalization-use-case-3/container-swap.png)

      **Na omwisselen**
      ![Container omgewisseld](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Op dezelfde manier wijzigt u de rangschikking van posities voor de andere kaartcontainers.
      ![Container omgewisseld](assets/personalization-use-case-3/after-swap-all.png)
   11. Voeg ook een koptekst toe onder de carrouselcomponent en boven de kaartlay-out.
   12. Klik op de carrouselcontainer en selecteer de optie **Invoegen na > HTML** om HTML toe te voegen.
      ![Tekst toevoegen](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Tekst toevoegen](assets/personalization-use-case-3/after-changes.png)
   13. Klik op **Volgende** om door te gaan met uw activiteiten.
   14. Selecteer de Methode **van de Toewijzing van het** Verkeer als hand en wijs 100% verkeer aan **Ervaring B**toe.
      ![Ervaring B Verkeer](assets/personalization-use-case-2/traffic.png)
   15. Klik op **Next**.
   16. Verstrek **Goal Metrics** voor uw Activiteit en sparen en sluit uw Test A/B.
      ![Metrisch testdoel A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Geef een naam op (**WKND Home Page Refresh**) voor uw activiteit en sla uw wijzigingen op.
   18. Zorg ervoor dat u uw activiteit **activeert** via het scherm Activiteitsgegevens.
      ![Activiteit activeren](assets/personalization-use-case-3/save-activity.png)
   19. Navigeer naar de startpagina van WKND (http://localhost:4503/content/wknd/en.html) en u merkt de wijzigingen die we hebben toegevoegd aan de testactiviteit A/B van de WKND-startpagina vernieuwen.
      ![WKND-startpagina vernieuwd](assets/personalization-use-case-3/activity-result.png)
   20. Open uw browser console, en inspecteer het netwerklusje om doelreactie voor de Pagina van het Huis te zoeken WKND verfrist A/B de activiteit van de Test van de Pagina A/B.
      ![Netwerkactiviteit](assets/personalization-use-case-3/activity-result.png)

## Samenvatting

In dit hoofdstuk, kon een teller een ervaring tot stand brengen gebruikend Visual Experience Composer door te slepen en neer te zetten, de lay-out en de inhoud van een Web-pagina te ruilen zonder enige code te veranderen om een test in werking te stellen.
