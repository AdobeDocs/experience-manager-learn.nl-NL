---
title: Distribueren naar ontwikkelomgeving
description: De code distribueren vanuit de vertakking van de gegevensopslagruimte van de cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Code Deployment
jira: KT-8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
duration: 23
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 0%

---

# Distribueren naar ontwikkelomgeving

In de vorige stap duwden we onze hoofdvertakking van onze lokale git-opslagplaats naar de MyFirstAF-vertakking van de cloudbeheeropslagplaats.

De volgende stap is de code aan het ontwikkelmilieu op te stellen.
Meld u aan bij Cloud Manager en selecteer uw programma

Selecteer de Deplosie aan Dev zoals hieronder getoond


![ eerste-stap ](assets/deploy-first-step1.png)


Distributiepijpleiding selecteren zoals wordt weergegeven
![ eerste-stap ](assets/deploy1.png)

Selecteer de broncode en de juiste Git-vertakking
![ eerste-stap ](assets/deploy2.png)
Controleer of uw wijzigingen zijn bijgewerkt

De pijplijn uitvoeren
![ looppas-pijpleiding ](assets/run-pipeline.png)

Als de code eenmaal is geïmplementeerd, worden de wijzigingen weergegeven in de cloudservice-instantie van AEM Forms.

## Volgende stappen

[Geweven archetype-project bijwerken](./updating-project-archetype.md)
