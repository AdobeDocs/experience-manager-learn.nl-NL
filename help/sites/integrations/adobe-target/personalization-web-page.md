---
title: Personalization of full web page Experience
description: Leer hoe u een doelactiviteit maakt om uw AEM webpagina's met Adobe Target om te leiden naar nieuwe pagina's.
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 89
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Personalization of full web page Experience {#personalization-fpe}

Leer hoe u een activiteit maakt om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.

## Vereisten

Om volledige pagina&#39;s van een AEM website te personaliseren, moet de volgende opstelling worden voltooid:

1. [Adobe Target toevoegen aan uw AEM website](./add-target-launch-extension.md)
1. [Een Adobe Target-aanroep vanuit tags activeren](./load-and-fire-target.md)

## Overzicht van scenario

De WKND-site heeft zijn homepage opnieuw ontworpen en wil de bezoekers van hun huidige homepage omleiden naar de nieuwe homepage. Tegelijkertijd moet u ook begrijpen hoe de vernieuwde homepage de betrokkenheid en inkomsten van gebruikers helpt verbeteren. Als markeerteken hebt u de taak gekregen om een activiteit te maken om de bezoekers om te leiden naar de nieuwe startpagina. Laten we de homepage van de WKND-site verkennen en leren hoe we een activiteit kunnen maken met Adobe Target.

## Stappen om een A/B-test te creëren met behulp van Visual Experience Composer (VEC)

1. Aanmelden bij Adobe Target en navigeren naar het tabblad Activiteiten
1. Klik **creëren de knoop van de Activiteit** en kies dan **A/B de Activiteit van de Test**

   ![ Activiteit A/B ](assets/ab-target-activity.png)

1. Selecteer de **Visuele Composer van de Ervaring** optie, verstrek de Activiteit URL, en klik dan **daarna**

   ![ Activiteit URL ](assets/ab-test-url.png)

1. De visuele Composer van de Ervaring toont twee lusjes op de linkerkant nadat u een activiteit creeert: *Ervaring A* en *Ervaring B*. Selecteer een ervaring in de lijst. U kunt nieuwe ervaringen aan de lijst toevoegen door **te gebruiken voeg Ervaring** knoop toe.

   ![ de Opties van de Ervaring ](assets/experience-options.png)

1. De opties van de mening beschikbaar voor Ervaring A en selecteren dan de **Redirect aan URL** optie en verstrekken een URL voor de nieuwe WKND homepage van de Plaats.

   ![ Redirect URL ](assets/redirect-url.png)

1. Naam *Ervaring A* aan *Nieuwe WKND Homepage* en *Ervaring B* aan *WKND Homepage* anders

   ![ avonturen ](assets/new-experiences.png)

1. Klik **daarna** om zich aan het richten te bewegen en een Handmatige verkeerstoewijzing van 50-50 tussen de twee ervaringen te houden.

   ![ gericht ](assets/targeting.png)

1. Voor Doelstellingen en instellingen kiest u de rapportbron als Adobe Target en selecteert u de metrische waarde Doel als Omzetting met een paginaweergaveactie.

   ![ Doelen ](assets/goals.png)

1. Geef een naam op voor uw activiteiten en sla deze op.
1. Activeer je opgeslagen activiteit om je wijzigingen live te zetten.

   ![ Doelen ](assets/activate.png)

1. Open uw sitepagina (Activiteit URL van stap 3) op een nieuw tabblad en u moet een van de ervaringen (WKND-startpagina of Nieuwe WKND-startpagina) kunnen bekijken vanaf onze testactiviteit A/B. `us/en.html` wordt omgeleid naar `us/home.html` .

   ![ Doelen ](assets/redirect-test.png)

## Samenvatting

Als markeerteken kunt u een activiteit maken om uw sitepagina&#39;s die op AEM worden gehost, om te leiden naar een nieuwe pagina met Adobe Target.

## Ondersteunende koppelingen

* [ Foutopsporing van Adobe Experience Cloud - Chrome ](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
