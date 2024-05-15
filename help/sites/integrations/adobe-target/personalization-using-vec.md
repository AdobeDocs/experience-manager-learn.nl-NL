---
title: Personalisatie met behulp van Visual Experience Composer
description: Leer hoe te om een Activiteit van Adobe Target tot stand te brengen gebruikend Visual Experience Composer.
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Personalisatie met behulp van Visual Experience Composer {#personalization-vec}

Leer hoe te om een Activiteit van het Doel van de Test te creëren A/B gebruikend Visuele Composer van de Ervaring (VEC).

## Vereisten

Als u VEC op een AEM website wilt gebruiken, moet u de volgende installatie uitvoeren:

1. [Adobe Target toevoegen aan uw AEM website](./add-target-launch-extension.md)
1. [Een Adobe Target-aanroep vanuit tags activeren](./load-and-fire-target.md)

## Overzicht van scenario

Op de homepage van de WKND-site worden lokale activiteiten of de beste dingen die u rondom een stad kunt doen, weergegeven in de vorm van informatiekaarten. Als markeerteken, bent u toegewezen de taak om de homepage te wijzigen, door tekstveranderingen in het leerapparaat van de avontuursectie aan te brengen en te begrijpen hoe het omzetting verbetert.

## Stappen om een A/B-test te creëren met behulp van Visual Experience Composer (VEC)

1. Aanmelden bij [Adobe Experience Cloud](https://experience.adobe.com/)tikken op __Doel__, navigeert u naar de __Activiteiten__ tab

   + Als u niet ziet __Doel__ op het dashboard van het Experience Cloud, zorg ervoor dat de correcte organisatie van de Adobe in de organisatieschakelaar in het hoogste recht wordt geselecteerd, en dat de gebruiker toegang tot Doel heeft verleend binnen [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Klikken **Activiteit maken** en kies vervolgens **A/B-test** activiteit

   ![A/B-activiteit](assets/ab-target-activity.png)

1. Selecteer de **Visual Experience Composer** , geeft u de URL van de activiteit op en klikt u op **Volgende**

   ![URL van activiteit](assets/ab-test-url.png)

1. De visuele Composer van de Ervaring toont twee lusjes op de linkerkant nadat u een activiteit creeert: *Ervaring A* en *Ervaring B*. Selecteer een ervaring in de lijst. U kunt nieuwe ervaringen aan de lijst toevoegen door **Ervaring toevoegen** knop.

   ![Ervaring A](assets/experience.png)

1. Selecteer een afbeelding of tekst op de pagina die u wilt wijzigen of gebruik de code-editor om het element te selecteren en te HTML.

   ![Element](assets/select-element.png)

1. De tekst wijzigen vanuit *Kamperen in West-Australië* tot *Vluchtelingen van Australië*. Een lijst met wijzigingen die aan een ervaring zijn toegevoegd, wordt weergegeven onder Wijzigingen. U kunt op het gewijzigde item klikken en dit bewerken om de CSS-kiezer weer te geven en de nieuwe inhoud die eraan is toegevoegd.

   ![avonturen](assets/adventures.png)

1. Naam wijzigen *Ervaring A* tot *Adventure*
1. Werk de tekst op dezelfde manier bij *Ervaring B* van *Kamperen in West-Australië* tot *Ontdek de Australische wilde natuur*.

   ![Verkennen](assets/explore.png)

1. Klikken **Volgende** om naar gericht te bewegen en een Handmatige verkeerstoewijzing van 50-50 tussen de twee ervaringen te houden.

   ![Targeting](assets/targeting.png)

1. Voor Doelstellingen en instellingen kiest u de rapportbron als Adobe Target en selecteert u de metrische waarde Doel als Omzetting met een paginaweergaveactie.

   ![Doelen](assets/goals.png)

1. Geef een naam op voor uw activiteiten en sla deze op.
1. Activeer je opgeslagen activiteit om je wijzigingen live te zetten.

   ![Doelen](assets/activate.png)

1. Open uw sitepagina (Activiteit URL van stap 3) in een nieuw tabblad en u zou één van beide ervaringen (avontuur of Onderzoek) van onze testactiviteit A/B moeten kunnen bekijken.

   ![Doelen](assets/publish.png)

## Samenvatting

In dit hoofdstuk kon een teller een ervaring tot stand brengen gebruikend Visual Experience Composer door te slepen en neer te zetten, de lay-out en de inhoud van een Web-pagina te ruilen zonder enige code te veranderen om een test in werking te stellen.

## Ondersteunende koppelingen

+ [Adobe Experience Cloud-foutopsporing - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
