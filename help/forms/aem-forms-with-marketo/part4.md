---
title: AEM Forms met Marketo (Deel 4)
seo-title: AEM Forms met Marketo (Deel 4)
description: Zelfstudie over de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
seo-description: Zelfstudie over de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---


# Adaptief formulier maken met formuliergegevensmodel

De volgende stap bestaat uit het maken van een adaptief formulier en het baseren op het formuliergegevensmodel dat in de vorige stap is gemaakt.
De gebruiker zal identiteitskaart van de Leiding ingaan en bij het op de tabs van de dienst van de Marketo om de lood door identiteitskaart te krijgen zal worden aangehaald. De resultaten van de servicebewerking worden vervolgens toegewezen aan de desbetreffende velden van de Adaptive Forms.

1. Maak een adaptief formulier en baseer dit op &quot;Lege formuliersjabloon&quot; en koppel het formulier aan het formuliergegevensmodel dat u in de vorige stap hebt gemaakt.
1. Het formulier openen in de bewerkingsmodus
1. Sleep een component TextField en een component Panel naar het adaptieve formulier. Stel de titel van de component TextField &quot;Enter Lead Id&quot; in en stel de naam ervan in op &quot;LeadId&quot;
1. Sleep 2 TextField-componenten naar de component Panel.
1. Plaats de Naam en de Titel van de 2 componenten TextField als FirstName en LastName
1. Vorm de component van het Comité om een herhaalbare component te zijn door Minimum aan 1 en Maximum aan -1 te plaatsen. Dit wordt vereist aangezien de dienst van de Marketo een serie van LeidingsVoorwerpen terugkeert en u een herhaalbare component moet hebben om de resultaten te tonen. In dit geval krijgen we echter maar één object Lead terug, omdat we met de id op Lead-objecten zoeken.
1. Maak een regel in het veld LeadId, zoals in de onderstaande afbeelding wordt getoond
1. Geef een voorbeeld van het formulier weer en voer een geldige lead-id in het veld en de tab LeadID in. De gebieden Voornaam en Achternaam zouden met de resultaten van de de dienstvraag moeten worden bevolkt.

Het volgende schermschot verklaart de montages van de regelredacteur

![ruleeditor](assets/ruleeditor.jfif)
