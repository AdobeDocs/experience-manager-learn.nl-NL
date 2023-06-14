---
title: Voorwaarden installeren
description: Installeer de benodigde software om uw ontwikkelomgeving in te stellen
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# De vereiste software installeren

Deze zelfstudie begeleidt u door de stappen die nodig zijn om een AEM Forms-project te maken en synchroniseert het AEM Forms-project met uw lokale AEM-instantie met behulp van IntelliJ en repo. U leert ook hoe u uw project toevoegt aan de lokale opslagplaats voor it en de lokale opslagplaats voor it naar de opslagplaats voor cloudbeheer stuurt.




In deze zelfstudie wordt de volgende stap gezet in de mapstructuur.

* [JDK 11 installeren](https://www.oracle.com/java/technologies/downloads/#java11-windows). Ik heb jdk-11.0.6_windows-x64_bin.zip gedownload
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Als u bijvoorbeeld Maven hebt geïnstalleerd in c:\maven folder, moet u een omgevingsvariabele maken met de naam M2_HOME met de waarde C:\maven\apache-maven-3.6.0. Voeg vervolgens M2_HOME\bin toe aan het pad en sla uw instelling op.

## Maven-project maken met AEM projectarchetype

* Een map maken met de naam **cloudmanager**(u kunt het om het even welke naam geven) in uw aandrijving van c
* Open uw opdrachtpromptvenster en navigeer naar **c:\cloudmanager**
* Kopieer en plak de inhoud van de [tekstbestand](assets/creating-maven-project.txt) in uw opdrachtpromptvenster. Afhankelijk van het [nieuwste versie](https://github.com/adobe/aem-project-archetype/releases). De meest recente versie was 30 op het moment dat dit artikel werd geschreven.
* Voer het bevel uit door op Enter te drukken.Als alles correct gaat zou u het bericht van het bouwstijlsucces moeten zien.

## Volgende stappen

[IntelliJ installeren](./intellij-set-up.md)