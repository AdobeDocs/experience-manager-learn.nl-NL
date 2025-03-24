---
title: Personalization die Visual Experience Composer gebruikt
description: Leer hoe te om een Activiteit van Adobe Target tot stand te brengen gebruikend Visual Experience Composer.
version: Experience Manager as a Cloud Service
jira: KT-6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
duration: 101
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Personalization die Visual Experience Composer gebruikt {#personalization-vec}

Leer hoe te om een Activiteit van het Doel van de Test te creëren A/B gebruikend Visuele Composer van de Ervaring (VEC).

## Vereisten

Als u VEC op een AEM-website wilt gebruiken, moet u de volgende installatie uitvoeren:

1. [Adobe Target toevoegen aan uw AEM-website](./add-target-launch-extension.md)
1. [Een Adobe Target-aanroep vanuit tags activeren](./load-and-fire-target.md)

## Overzicht van scenario

Op de homepage van de WKND-site worden lokale activiteiten of de beste dingen die u rondom een stad kunt doen, weergegeven in de vorm van informatiekaarten. Als markeerteken, bent u toegewezen de taak om de homepage te wijzigen, door tekstveranderingen in het leerapparaat van de avontuursectie aan te brengen en te begrijpen hoe het omzetting verbetert.

## Stappen om een A/B-test te creëren met behulp van Visual Experience Composer (VEC)

1. Login aan [ Adobe Experience Cloud ](https://experience.adobe.com/), ontvang op __Doel__, navigeer aan het __Activiteiten__ lusje

   + Als u niet __Doel__ op het dashboard van Experience Cloud ziet, zorg ervoor dat de correcte organisatie van Adobe in de organisatieschakelaar in het hoogste recht wordt geselecteerd, en dat de gebruiker toegang tot Doel in [ Adobe Admin Console ](https://adminconsole.adobe.com/) is verleend.

1. Klik **creëren de knoop van de Activiteit** en kies dan **A/B de Activiteit van de Test**

   ![ Activiteit A/B ](assets/ab-target-activity.png)

1. Selecteer de **Visuele Composer van de Ervaring** optie, verstrek de Activiteit URL, en klik dan **daarna**

   ![ Activiteit URL ](assets/ab-test-url.png)

1. De visuele Composer van de Ervaring toont twee lusjes op de linkerkant nadat u een activiteit creeert: *Ervaring A* en *Ervaring B*. Selecteer een ervaring in de lijst. U kunt nieuwe ervaringen aan de lijst toevoegen door **te gebruiken voeg Ervaring** knoop toe.

   ![ Ervaring A ](assets/experience.png)

1. Selecteer een afbeelding of tekst op de pagina die u wilt wijzigen of gebruik de code-editor om het HTML-element te selecteren en te selecteren.

   ![ Element ](assets/select-element.png)

1. Verander de tekst van *Camping in West Australië* aan *avonturen van Australië*. Een lijst met wijzigingen die aan een ervaring zijn toegevoegd, wordt weergegeven onder Wijzigingen. U kunt op het gewijzigde item klikken en dit bewerken om de CSS-kiezer weer te geven en de nieuwe inhoud die eraan is toegevoegd.

   ![ avonturen ](assets/adventures.png)

1. Verander *Ervaring A* aan *avontuur*
1. Op dezelfde manier werk de tekst op *Ervaring B* van *Camping in West Australië* bij *ontdekken de Australische Wilderheid*.

   ![ ontdekken ](assets/explore.png)

1. Klik **daarna** om zich aan het richten te bewegen en een Handmatige verkeerstoewijzing van 50-50 tussen de twee ervaringen te houden.

   ![ gericht ](assets/targeting.png)

1. Voor Doelstellingen en instellingen kiest u de rapportbron als Adobe Target en selecteert u de metrische waarde Doel als Omzetting met een paginaweergaveactie.

   ![ Doelen ](assets/goals.png)

1. Geef een naam op voor uw activiteiten en sla deze op.
1. Activeer je opgeslagen activiteit om je wijzigingen live te zetten.

   ![ Doelen ](assets/activate.png)

1. Open uw sitepagina (Activiteit URL van stap 3) in een nieuw tabblad en u zou één van beide ervaringen (avontuur of Onderzoek) van onze testactiviteit A/B moeten kunnen bekijken.

   ![ Doelen ](assets/publish.png)

## Samenvatting

In dit hoofdstuk kon een teller een ervaring tot stand brengen gebruikend Visual Experience Composer door te slepen en neer te zetten, de lay-out en de inhoud van een Web-pagina te ruilen zonder enige code te veranderen om een test in werking te stellen.

## Ondersteunende koppelingen

+ [ Foutopsporing van Adobe Experience Cloud - Chrome ](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
