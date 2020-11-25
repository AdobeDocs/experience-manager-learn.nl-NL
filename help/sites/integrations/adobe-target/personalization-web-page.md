---
title: Aanpassing van volledige webpaginabeleving
description: Leer hoe u een doelactiviteit maakt om uw AEM webpagina's met Adobe Target om te leiden naar nieuwe pagina's.
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---


# Aanpassing van volledige webpaginabeleving {#personalization-fpe}

Leer hoe u een activiteit maakt om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.

## Vereisten

Om volledige pagina&#39;s van een AEM website te personaliseren, moet de volgende opstelling worden voltooid:

1. [Adobe Target toevoegen aan uw AEM website](./add-target-launch-extension.md)
1. [Een Adobe Target-aanroep vanuit Launch activeren](./load-and-fire-target.md)

## Overzicht van scenario

De WKND-site heeft zijn homepage opnieuw ontworpen en wil de bezoekers van hun huidige homepage omleiden naar de nieuwe homepage. Tegelijkertijd moet u ook begrijpen hoe de vernieuwde homepage de betrokkenheid en inkomsten van gebruikers helpt verbeteren. Als markeerteken hebt u de taak gekregen om een activiteit te maken om de bezoekers om te leiden naar de nieuwe startpagina. Laten we de homepage van de WKND-site bekijken en leren hoe we een activiteit maken met Adobe Target.

## Stappen om een A/B-test te creëren met behulp van Visual Experience Composer (VEC)

1. Aanmelden bij Adobe Target en navigeren naar het tabblad Activiteiten
1. Klik op de knop Activiteit **** maken en kies vervolgens **A/B-testactiviteit**

   ![A/B-activiteit](assets/ab-target-activity.png)

1. Selecteer de optie **Visual Experience Composer** , geef de Activiteit-URL op en klik op **Volgende**

   ![URL van activiteit](assets/ab-test-url.png)

1. De visuele Composer van de Ervaring toont twee lusjes op de linkerkant nadat u een nieuwe activiteit creeert: *Ervaar A* en *Ervaring B*. Selecteer een ervaring in de lijst. U kunt nieuwe ervaringen aan de lijst toevoegen, door de **Add knoop van de Ervaring** te gebruiken.

   ![Ervingopties](assets/experience-options.png)

1. De opties van de mening beschikbaar voor Ervaring A, selecteren dan **Redirect aan optie URL** en verstrekken een URL voor de nieuwe homepage van de Plaats WKND.

   ![URL omleiden](assets/redirect-url.png)

1. Naam *Ervaring A* wijzigen in *Nieuwe WKND-startpagina* en *Ervaring B* naar *WKND-startpagina*

   ![avonturen](assets/new-experiences.png)

1. Klik op **Volgende** om over te schakelen naar gericht en een handmatige verkeerstoewijzing van 50-50 tussen de twee ervaringen te behouden.

   ![Doelstelling](assets/targeting.png)

1. Voor Doelstellingen en instellingen kiest u de rapportbron als Adobe Target en selecteert u de metrische waarde Doel als Omzetting met een paginaweergaveactie.

   ![Doelen](assets/goals.png)

1. Geef een naam op voor uw activiteiten en sla deze op.
1. Activeer je opgeslagen activiteit om je wijzigingen live te zetten.

   ![Doelen](assets/activate.png)

1. Open uw sitepagina (Activiteit URL van stap 3) op een nieuw tabblad en u moet een van de ervaringen (WKND-startpagina of Nieuwe WKND-startpagina) kunnen bekijken vanaf onze testactiviteit A/B. `us/en.html` omleidingen naar `us/home.html`.

   ![Doelen](assets/redirect-test.png)

## Samenvatting

Als markeerteken kunt u een activiteit maken om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.

## Ondersteunende koppelingen

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

