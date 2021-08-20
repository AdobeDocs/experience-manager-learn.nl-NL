---
title: Formuliergegevens opslaan en ophalen vanuit MySQL-database
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het opslaan en ophalen van formuliergegevens
feature: Adaptieve Forms
type: Tutorial
version: 6.3,6.4,6.5
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---


# Adaptieve formuliergegevens opslaan en ophalen vanuit MySQL-database

Deze zelfstudie begeleidt u door de stappen die nodig zijn voor het opslaan en ophalen van adaptieve formuliergegevens uit de database. Deze zelfstudie gebruikte MySQL-database voor het opslaan van adaptieve formuliergegevens. Het gegevensbestand van uw keus kan worden gebruikt om de gegevens op te slaan zolang u de gegevensbestand specifieke bestuurders in AEM hebt opgesteld. Op hoog niveau zijn de volgende stappen nodig om het gebruiksgeval te bereiken:

* Gebruik de GuideBridge-API om toegang te krijgen tot de Adaptief-formuliergegevens

* Maak een POST vraag aan een servlet. Deze servlet slaat de gegevens in het gegevensbestand op. De opgeslagen gegevens worden geassocieerd met een GUID

* Wanneer u de Aangepaste Vorm met de opgeslagen gegevens wilt bevolken, wint u de gegevens verbonden aan GUID terug en bevolkt de Aanpassende Vorm gebruikend **request.setAttribute** methode.

## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
