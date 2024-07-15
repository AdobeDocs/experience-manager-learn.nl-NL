---
title: Voorwaarden installeren
description: Installeer de benodigde software om uw ontwikkelomgeving in te stellen
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
duration: 44
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# De vereiste software installeren

Deze zelfstudie begeleidt u door de stappen die nodig zijn om een AEM Forms-project te maken en synchroniseert het AEM Forms-project met uw lokale AEM-instantie met behulp van IntelliJ en repo. U leert ook hoe u uw project toevoegt aan de lokale opslagplaats voor it en de lokale opslagplaats voor it naar de opslagplaats voor cloudbeheer stuurt.





In deze zelfstudie wordt de volgende stap gezet in de mapstructuur.

* [ installeer JDK 11 ](https://www.oracle.com/java/technologies/downloads/#java11-windows). Ik heb jdk-11.0.6_windows-x64_bin.zip gedownload
* [ Gemaakt ](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Bijvoorbeeld als u Geweven in c:\maven omslag hebt geïnstalleerd, zult u een milieuvariabele geroepen M2_HOME met waarde C:\maven\apache-maven-3.6.0 moeten tot stand brengen. Voeg vervolgens M2_HOME\bin toe aan het pad en sla uw instelling op.

## Maven-project maken met AEM projectarchetype

* Creeer een omslag genoemd **cloudmanager** (u kunt het om het even welke naam) in uw aandrijving van c geven
* Open uw venster van de bevelherinnering en navigeer aan **c:\cloudmanager**
* Kopieer en kleef de inhoud van het [ tekstdossier ](assets/creating-maven-project.txt) in uw venster van de bevelherinnering. U kunt DarchetypeVersion=30 afhankelijk van de [ recentste versie ](https://github.com/adobe/aem-project-archetype/releases) moeten veranderen. De meest recente versie was 30 op het moment dat dit artikel werd geschreven.
* Voer het bevel uit door op Enter te drukken.Als alles correct gaat zou u het bericht van het bouwstijlsucces moeten zien.

## Volgende stappen

[IntelliJ installeren](./intellij-set-up.md)