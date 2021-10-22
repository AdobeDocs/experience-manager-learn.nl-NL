---
title: Distribueren naar ontwikkelomgeving
description: De code distribueren vanuit de vertakking van de gegevensopslagruimte van de cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '112'
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
