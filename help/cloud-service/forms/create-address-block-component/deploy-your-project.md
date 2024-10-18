---
title: Adrescomponent maken
description: Nieuwe adreskern-component maken in AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Uw project implementeren

Voordat u het project gaat implementeren op uw AEM Forms-as a Cloud Service, wordt u aangeraden het project te implementeren in uw lokale cloudinstantie van AEM Forms.

## Wijzigingen synchroniseren met uw AEM project

Start IntelliJ en navigeer naar de map adaptiveForm onder de map ``ui.apps`` (zie hieronder)
![ intellij ](assets/intellij.png)

Klik met de rechtermuisknop op het knooppunt ``adaptiveForm`` en selecteer Nieuw | Pakket
Zorg ervoor u de naam **adressblock** aan het pakket toevoegt

Klik met de rechtermuisknop op het nieuwe pakket ``addressblock`` en selecteer ``repo | Get Command`` zoals hieronder weergegeven
![ repo-sync ](assets/sync-repo.png)

Dit moet het project synchroniseren met uw lokale, voor de cloud geschikte AEM Forms-instantie. U kunt het .content.xml-bestand controleren om de eigenschappen te bevestigen
![ na-synchronisatie ](assets/after-sync.png)

## Project distribueren naar uw lokale instantie

Begin een nieuw bevel snel venster en navigeer aan de wortelomslag van het project en bouwt het project gebruikend het hieronder getoonde bevel
![ opstellen ](assets/build-project.png)

Zodra het project met succes wordt opgesteld,
Adrescomponent kan nu worden gebruikt in een adaptief formulier

## Het project implementeren in een cloud-omgeving

Als alles goed op uw lokale ontwikkelomgeving kijkt, is de volgende stap aan de [ wolkeninstantie op te stellen gebruikend wolkenmanager.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
