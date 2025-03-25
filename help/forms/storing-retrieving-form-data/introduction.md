---
title: Het opslaan van en het Ophalen van de Gegevens van de Vorm van de Inleiding van het Gegevensbestand MySQL
description: Zelfstudie met meerdere onderdelen om door de stappen te bladeren die nodig zijn voor het opslaan en ophalen van formuliergegevens
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 236
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# Adaptieve formuliergegevens opslaan en ophalen vanuit MySQL-database

Deze zelfstudie begeleidt u door de stappen die nodig zijn voor het opslaan en ophalen van adaptieve formuliergegevens uit de database. Deze zelfstudie gebruikte MySQL-database voor het opslaan van adaptieve formuliergegevens. De database van uw keuze kan worden gebruikt om de gegevens op te slaan zolang u de databasespecifieke stuurprogramma&#39;s in AEM hebt geïmplementeerd. Op hoog niveau zijn de volgende stappen nodig om het gebruiksgeval te bereiken:

* Gebruik de GuideBridge-API om toegang te krijgen tot de Adaptief-formuliergegevens

* Maak een POST vraag aan een servlet. Deze servlet slaat de gegevens in het gegevensbestand op. De opgeslagen gegevens worden geassocieerd met een GUID

* Wanneer u de Aangepaste Vorm met de opgeslagen gegevens wilt bevolken, wint u de gegevens verbonden aan GUID terug en bevolkt de Aanpassende Vorm gebruikend de {**methode 0} request.setAttribute.**

## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


