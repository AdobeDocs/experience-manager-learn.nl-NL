---
title: Formuliergegevens opslaan en ophalen met bijlagen uit MySQL-database
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het opslaan en ophalen van formuliergegevens met bijlagen
feature: Adaptieve Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 1%

---


# Adaptieve formuliergegevens opslaan en ophalen met 2FA

In deze zelfstudie worden de stappen beschreven die nodig zijn voor het opslaan en ophalen van adaptieve formuliergegevens met bijlagen in 2FA. Deze zelfstudie gebruikte MySQL-database voor het opslaan van adaptieve formuliergegevens. Het gegevensbestand van uw keus kan worden gebruikt om de gegevens op te slaan zolang u de gegevensbestand specifieke bestuurders in AEM hebt opgesteld. Op hoog niveau zijn de volgende stappen nodig om het gebruiksgeval te bereiken:

* Gebruik de GuideBridge-API om toegang te krijgen tot de Adaptief-formuliergegevens

* Maak een POST vraag aan een servlet. Deze servlet slaat de gegevens in het gegevensbestand en de vormgehechtheid in de bewaarplaats CRX op. De opgeslagen gegevens in het gegevensbestand worden geassocieerd met een GUID.

* Wanneer u de Aangepaste Vorm met de opgeslagen gegevens wilt bevolken, wint u de gegevens verbonden aan GUID terug en bevolkt de Aanpassende Vorm gebruikend **request.setAttribute** methode.

## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## Vereisten

Het publiek van deze inhoud heeft naar verwachting enige ervaring op de volgende gebieden:

* Adaptief formulier
* Formuliergegevensmodel
* OSGi-diensten/onderdelen
* Clientbibliotheken AEM
