---
title: Personalisatie met Adobe Target Visual Experience Composer
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

# Personalisatie met behulp van Visual Experience Composer

In dit hoofdstuk, zullen wij het creëren van ervaringen onderzoeken gebruikend **Visual Experience Composer** door de lay-out en inhoud van een webpagina vanuit Target te slepen en neer te zetten, te wisselen en te wijzigen.

## Overzicht van scenario

De WKND-homepage geeft lokale activiteiten weer of het beste wat je kunt doen rond een stad in de vorm van kaartlay-outs. Als markator, bent u de taak toegewezen om de homepage te wijzigen, door de kaartlay-outs te herschikken om te zien hoe het gebruikersbetrokkenheid beïnvloedt en omzetting drijft.

### Betrokken gebruikers

Voor deze oefening, moeten de volgende gebruikers worden betrokken en om sommige taken uit te voeren u administratieve toegang zou kunnen vereisen.

* **Content Producer/Content Editor** (Adobe Experience Manager)
* **Marketer** (Adobe Target / Optimalisatieteam)

### Startpagina WKND-site

![AEM doelscenario 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Vereisten

* **AEM**
   * [AEM-instantie publiceren](./implementation.md#getting-aem) 4503
   * [AEM geïntegreerd met Adobe Target met tags](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Toegang tot uw organisaties Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud voorzien van [Adobe Target](https://experiencecloud.adobe.com)

## Marktactiviteiten

1. De Marketer maakt een A/B-doelactiviteit in Adobe Target.
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
   7. **Ervaring A** biedt de standaard WKND-startpagina en laten we de inhoudslay-out bewerken voor **Ervaring B**.
      ![Ervaring B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Klik op een van de kaartlay-outcontainers (*Beste roasters*) en selecteert u **Opnieuw rangschikken** -optie.
      ![Containerselectie](assets/personalization-use-case-3/container-selection.png)
   9. Klik op de container die u opnieuw wilt rangschikken en sleep deze naar de gewenste locatie. Laten we de *Beste roasters* container van eerste rij 1e kolom tot eerste rij 3e kolom. Nu de *Beste roasters* container staat naast *Fotografie-tentoonstellingen* container.
      ![Container omwisselen](assets/personalization-use-case-3/container-swap.png)
      **Na omwisselen**
      ![Container omgewisseld](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Op dezelfde manier wijzigt u de rangschikking van posities voor de andere kaartcontainers.
      ![Container omgewisseld](assets/personalization-use-case-3/after-swap-all.png)
   11. Voeg ook een koptekst toe onder de carrouselcomponent en boven de kaartlay-out.
   12. Klik op de carrouselcontainer en selecteer de optie **Inkrimpen na > HTML** HTML toevoegen.
      ![Tekst toevoegen](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Tekst toevoegen](assets/personalization-use-case-3/after-changes.png)
   13. Klikken **Volgende** om door te gaan met uw activiteiten.
   14. Selecteer de **Methode voor verkeerstoewijzing** als handmatig en 100% verkeer toewijzen aan **Ervaring B**.
      ![Ervaring B Verkeer](assets/personalization-use-case-2/traffic.png)
   15. Klik op **Next**.
   16. Verlenen **Goederenstatistieken** voor uw Activiteit en sparen en sluit uw Test A/B.
      ![Metrisch testdoel A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Geef een naam op (**WKND-startpagina vernieuwen**) voor uw activiteit en sla uw wijzigingen op.
   18. Zorg ervoor dat in het scherm Activiteitsdetails **Activeren** uw activiteit.
      ![Activiteit activeren](assets/personalization-use-case-3/save-activity.png)
   19. Navigeer naar de startpagina van WKND (http://localhost:4503/content/wknd/en.html) en u merkt de wijzigingen die we hebben toegevoegd aan de testactiviteit A/B van de WKND-startpagina vernieuwen.
      ![WKND-startpagina vernieuwd](assets/personalization-use-case-3/activity-result.png)
   20. Open uw browser console, en inspecteer het netwerklusje om doelreactie voor de Pagina van het Huis te zoeken WKND verfrist A/B de activiteit van de Test van de Test A.
      ![Netwerkactiviteit](assets/personalization-use-case-3/activity-result.png)

## Samenvatting

In dit hoofdstuk, kon een teller een ervaring tot stand brengen gebruikend Visual Experience Composer door te slepen en neer te zetten, de lay-out en de inhoud van een Web-pagina te ruilen zonder enige code te veranderen om een test in werking te stellen.
