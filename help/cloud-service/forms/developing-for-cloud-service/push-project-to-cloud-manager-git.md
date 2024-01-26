---
title: Project AEM naar opslagplaats voor cloudbeheer
description: Push local git repository to the cloud manager repository
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
duration: 37
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# AEM project naar cloudmanager-git-repo verplaatsen

In de vorige stap hebben we ons AEM Project gesynchroniseerd met de Adaptive Forms en Thema&#39;s die in de AEM zijn gemaakt.
We moeten deze wijzigingen nu toevoegen aan onze lokale git-opslagplaats en vervolgens de lokale git-opslagplaats naar de git-opslagplaats voor cloudbeheer duwen.
Opdrachtprompt openen en naar c:\cloudmanager\aem-banking-app Voer de volgende opdrachten uit

```
git add .
```

Hiermee voegt u de nieuwe bestanden toe aan de werkgebiedvertakking van de lokale it-opslagruimte

```
git commit -m "My First AF"
```

Hierdoor worden de bestanden toegewezen aan de hoofdvertakking van onze lokale it-opslagruimte

```
git push -f bankingapp master:"MyFirstAF"
```

In het bovenstaande bevel duwen we onze hoofdtak van onze lokale git bewaarplaats in de afdeling MyFirstAF van de bewaarplaats van de wolkenmanager die door de bankingapp vriendelijke naam wordt geïdentificeerd.

## Volgende stappen

[Implementeer het project in de ontwikkelomgeving](./deploy-to-dev-environment.md)
