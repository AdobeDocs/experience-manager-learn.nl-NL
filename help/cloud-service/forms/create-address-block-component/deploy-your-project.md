---
title: Adrescomponent maken
description: Nieuwe adreskern-component maken in AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Uw project implementeren

Voordat u het project implementeert op uw AEM Forms as a Cloud Service, wordt u aangeraden het project te implementeren in uw lokale, voor de cloud geschikte versie van AEM Forms.

## Wijzigingen synchroniseren met uw AEM-project

Start IntelliJ en navigeer naar de map adaptiveForm onder de map ``ui.apps`` (zie hieronder)
![&#x200B; intellij &#x200B;](assets/intellij.png)

Klik met de rechtermuisknop op het knooppunt ``adaptiveForm`` en selecteer Nieuw | Pakket
Zorg ervoor u de naam **adressblock** aan het pakket toevoegt

Klik met de rechtermuisknop op het nieuwe pakket ``addressblock`` en selecteer ``repo | Get Command`` zoals hieronder weergegeven
![&#x200B; repo-sync &#x200B;](assets/sync-repo.png)

Dit moet het project synchroniseren met uw lokale, voor de cloud geschikte AEM Forms-instantie. U kunt het .content.xml-bestand controleren om de eigenschappen te bevestigen
![&#x200B; na-synchronisatie &#x200B;](assets/after-sync.png)

## Project distribueren naar uw lokale instantie

Begin een nieuw bevel snel venster en navigeer aan de wortelomslag van het project en bouwt het project gebruikend het hieronder getoonde bevel
![&#x200B; opstellen &#x200B;](assets/build-project.png)

Zodra het project met succes wordt opgesteld,
Adrescomponent kan nu worden gebruikt in een adaptief formulier

## Het project implementeren in een cloud-omgeving

Als alles goed op uw lokale ontwikkelomgeving kijkt, is de volgende stap aan de [&#x200B; wolkeninstantie op te stellen gebruikend wolkenmanager.](https://experienceleague.adobe.com/nl/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
