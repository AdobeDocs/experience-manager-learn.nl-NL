---
title: Adrescomponent maken
description: Nieuwe adreskern-component maken in AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Uw project implementeren

Voordat u het project implementeert op uw AEM Forms-Cloud Service, is het raadzaam het project te implementeren in uw lokale, voor de cloud geschikte instantie van AEM Forms.

## Wijzigingen synchroniseren met uw AEM project

Start IntelliJ en navigeer naar de map adaptiveForm onder de map ``ui.apps`` map zoals hieronder weergegeven
![intellij](assets/intellij.png)

Rechtsklik ingeschakeld ``adaptiveForm`` en selecteer Nieuw | Pakket controleren Controleer of u de naam toevoegt **adresblok** naar het pakket

Klik met de rechtermuisknop op het nieuwe pakket ``addressblock`` en selecteert u ``repo | Get Command`` zoals hieronder weergegeven
![repo-sync](assets/sync-repo.png)

Dit moet het project synchroniseren met uw lokale, voor de cloud geschikte AEM Forms-instantie. U kunt het .content.xml-bestand controleren om de eigenschappen te bevestigen
![after-sync](assets/after-sync.png)

## Project distribueren naar uw lokale instantie

Begin een nieuw bevel snel venster en navigeer aan de wortelomslag van het project en bouwt het project gebruikend het hieronder getoonde bevel
![inzetten](assets/build-project.png)

Zodra het project met succes wordt opgesteld, kan de component van het Adres nu in een Aangepaste Vorm worden gebruikt

## Het project implementeren in een cloud-omgeving

Als alles er goed uitziet in uw lokale ontwikkelomgeving, is de volgende stap implementeren in de [cloudinstantie met gebruik van cloudbeheer.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



