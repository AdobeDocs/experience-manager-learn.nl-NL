---
title: AEM-project naar cloudbeheeropslagplaats sturen
description: Push local git repository to the cloud manager repository
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# AEM-project naar cloudmanager verschuiven met statusrepo

In de vorige stap hebben we ons AEM-project gesynchroniseerd met de Adaptive Forms en Thema&#39;s die in de AEM-instantie zijn gemaakt.
We moeten deze wijzigingen nu toevoegen aan onze lokale git-opslagplaats en vervolgens de lokale git-opslagplaats naar de git-opslagplaats voor cloudbeheer duwen.
Opdrachtprompt openen en naar c:\cloudmanager\aem-banking-app navigeren
De volgende opdrachten uitvoeren

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
