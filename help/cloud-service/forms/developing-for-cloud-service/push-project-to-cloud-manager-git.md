---
title: Project AEM naar opslagplaats voor cloudbeheer
description: Push local git repository to the cloud manager repository
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# AEM project naar cloudmanager-git-repo verplaatsen

In de vorige stap hebben we ons AEM Project gesynchroniseerd met de Adaptive Forms en Thema&#39;s die in de AEM zijn gemaakt.
We moeten deze wijzigingen nu toevoegen aan onze lokale git-opslagplaats en vervolgens de lokale git-opslagplaats naar de git-opslagplaats voor cloudbeheer duwen.
Opdrachtprompt openen en naar c:\cloudmanager\aem-banking-app Execute the following commands navigeren

```
git add .**
```

Hiermee voegt u de nieuwe bestanden toe aan de werkgebiedvertakking van de lokale it-opslagruimte

```
git commit -m "My First AF"
```

Hierdoor worden de bestanden toegewezen aan de master vertakking van onze lokale it-opslagruimte

```
git push -f bankingapp master:"My First AF"
```

In het bovenstaande bevel duwen we onze master tak van onze lokale git opslagplaats naar de My First AF tak van de gegevensopslagplaats van de wolkenmanager die door de bankingapp vriendelijke naam wordt geïdentificeerd



