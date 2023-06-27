---
title: Distribueren naar ontwikkelomgeving
description: De code distribueren vanuit de vertakking van de gegevensopslagruimte van de cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Code Deployment
kt: 8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 0%

---

# Distribueren naar ontwikkelomgeving

In de vorige stap duwden we onze master vertakking van onze lokale git-opslagplaats naar de MyFirstAF-vertakking van de cloudbeheeropslagplaats.

De volgende stap is de code aan het ontwikkelmilieu op te stellen.
Meld u aan bij Cloud Manager en selecteer uw programma

Selecteer de Deplosie aan Dev zoals hieronder getoond


![eerste stap](assets/deploy-first-step1.png)


Distributiepijpleiding selecteren zoals wordt weergegeven
![eerste stap](assets/deploy1.png)

Selecteer de broncode en de juiste Git-vertakking
![eerste stap](assets/deploy2.png)
Controleer of uw wijzigingen zijn bijgewerkt

De pijplijn uitvoeren
![pijpleiding](assets/run-pipeline.png)

Als de code eenmaal is geïmplementeerd, worden de wijzigingen weergegeven in de cloudservice-instantie van AEM Forms.

## Volgende stappen

[Bezig met bijwerken van toegepast archetype-project](./updating-project-archetype.md)
