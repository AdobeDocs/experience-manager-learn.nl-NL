---
title: Personalisatie met behulp van Visual Experience Composer
description: Leer hoe te om een Activiteit van Adobe Target tot stand te brengen gebruikend Visual Experience Composer.
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 0%

---


# Personalisatie met behulp van Visual Experience Composer {#personalization-vec}

Leer hoe te om een Activiteit van het Doel van de Test te creëren A/B gebruikend Visuele Composer van de Ervaring (VEC).


## Overzicht van scenario

Op de homepage van de WKND-site worden lokale activiteiten of de beste manier om rond een stad te doen weergegeven in de vorm van informatiekaarten. Als markeerteken, bent u toegewezen de taak om de homepage te wijzigen, door tekstveranderingen in het leerapparaat van de avontuursectie aan te brengen en te begrijpen hoe het omzetting verbetert.

## Stappen om een A/B-test te creëren met behulp van Visual Experience Composer (VEC)

1. Aanmelden bij Adobe Target en navigeren naar het tabblad Activiteiten
2. Klik op de knop Activiteit **** maken en kies vervolgens **A/B-testactiviteit**

   ![A/B-activiteit](assets/ab-target-activity.png)

3. Selecteer de optie **Visual Experience Composer** , geef de Activiteit-URL op en klik op **Volgende**

   ![URL van activiteit](assets/ab-test-url.png)

4. De visuele Composer van de Ervaring toont twee lusjes op de linkerkant nadat u een nieuwe activiteit creeert: *Ervaar A* en *Ervaring B*. Selecteer een ervaring in de lijst. U kunt nieuwe ervaringen aan de lijst toevoegen, door de **Add knoop van de Ervaring** te gebruiken.

   ![Ervaring A](assets/experience.png)

5. Selecteer een afbeelding of tekst op de pagina die u wilt wijzigen of gebruik de code-editor om het element te selecteren en te HTML.

   ![Element](assets/select-element.png)

6. Wijzig de tekst van *Camping in Western Australia* in *Adventures of Australia*. Een lijst van veranderingen die aan een Ervaring worden toegevoegd zal onder Wijzigingen worden getoond. U kunt op het gewijzigde item klikken en dit bewerken om de CSS-kiezer weer te geven en de nieuwe inhoud die eraan is toegevoegd.

   ![avonturen](assets/adventures.png)

7. Naam *Ervaring A* wijzigen in *avontuur*
8. Werk ook de tekst over *Experience B* van *Camping in West-Australië* bij om de Australische wilde natuur *te* ontdekken.

   ![Verkennen](assets/explore.png)

9. Klik op **Volgende** om naar Targeting te gaan en zorg ervoor dat het verkeer tussen de twee ervaringen handmatig wordt toegewezen aan een snelheid van 50-50.

   ![Doelstelling](assets/targeting.png)

10. Voor Doelstellingen en instellingen kiest u de rapportbron als Adobe Target en selecteert u de metrische waarde Doel als Omzetting met een paginaweergaveactie.

   ![Doelen](assets/goals.png)

11. Geef een naam op voor uw activiteiten en sla deze op.
12. Activeer je opgeslagen activiteit om je wijzigingen live te zetten.

   ![Doelen](assets/activate.png)

13. Open uw sitepagina (Activiteit URL van stap 3) in een nieuw tabblad en u zou één van beide ervaringen (avontuur of Onderzoek) van onze testactiviteit A/B moeten kunnen bekijken.

   ![Doelen](assets/publish.png)

## Samenvatting

In dit hoofdstuk, kon een teller een ervaring tot stand brengen gebruikend Visual Experience Composer door te slepen en neer te zetten, de lay-out en de inhoud van een Web-pagina te ruilen zonder enige code te veranderen om een test in werking te stellen.

## Ondersteunende koppelingen

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)