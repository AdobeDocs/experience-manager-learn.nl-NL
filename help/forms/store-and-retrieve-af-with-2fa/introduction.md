---
title: Formuliergegevens opslaan en ophalen met bijlagen uit MySQL-database
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het opslaan en ophalen van formuliergegevens met bijlagen
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 148
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---

# Adaptieve formuliergegevens opslaan en ophalen met 2FA

In deze zelfstudie worden de stappen beschreven die nodig zijn voor het opslaan en ophalen van adaptieve formuliergegevens met bijlagen in 2FA. Deze zelfstudie gebruikte MySQL-database voor het opslaan van adaptieve formuliergegevens. Het gegevensbestand van uw keus kan worden gebruikt om de gegevens op te slaan zolang u de gegevensbestand specifieke bestuurders in AEM hebt opgesteld. Op hoog niveau zijn de volgende stappen nodig om het gebruiksgeval te bereiken:

* Gebruik de GuideBridge-API om toegang te krijgen tot de Adaptief-formuliergegevens

* Maak een POST vraag aan een servlet. Deze server slaat de gegevens op in de database en de formulierbijlagen in de CRX-opslagplaats. De opgeslagen gegevens in het gegevensbestand worden geassocieerd met een GUID.

* Wanneer u de Aangepaste Vorm met de opgeslagen gegevens wilt bevolken, wint u de gegevens verbonden aan GUID terug en bevolkt de Aanpassende Vorm gebruikend de {**methode 0} request.setAttribute.**

## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## Vereisten

Het publiek van deze inhoud heeft naar verwachting enige ervaring op de volgende gebieden:

* Adaptief formulier
* Formuliergegevensmodel
* OSGi-diensten/onderdelen
* Clientbibliotheken AEM


## Volgende stappen

[Source voor gegevens configureren](./configure-data-source.md)