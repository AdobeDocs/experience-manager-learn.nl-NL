---
title: AEM Forms met Marketo (Deel 4)
description: Zelfstudie voor de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptief Forms, formuliergegevensmodel
version: 6.3,6.4,6.5
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---


# Adaptief formulier maken met formuliergegevensmodel

De volgende stap bestaat uit het maken van een adaptief formulier en het baseren op het formuliergegevensmodel dat in de vorige stap is gemaakt.
De gebruiker voert de lead-id in en wanneer de Marketo-service op de Tab-toets wordt gedrukt om de leads te ontvangen met de id, wordt een aanroep gedaan. De resultaten van de servicebewerking worden vervolgens toegewezen aan de desbetreffende velden van de Adaptive Forms.

1. Maak een adaptief formulier en baseer dit op &quot;Lege formuliersjabloon&quot; en koppel het formulier aan het formuliergegevensmodel dat u in de vorige stap hebt gemaakt.
1. Het formulier openen in de bewerkingsmodus
1. Sleep een component TextField en een component Panel naar het adaptieve formulier. Stel de titel van de component TextField &quot;Enter Lead Id&quot; in en stel de naam ervan in op &quot;LeadId&quot;
1. Sleep 2 TextField-componenten naar de component Panel.
1. Plaats de Naam en de Titel van de 2 componenten TextField als FirstName en LastName
1. Vorm de component van het Comité om een herhaalbare component te zijn door Minimum aan 1 en Maximum aan -1 te plaatsen. Dit is vereist omdat de Marketo-service een array van hoofdobjecten retourneert en u een herhaalbare component nodig hebt om de resultaten weer te geven. In dit geval krijgen we echter maar één object Lead terug, omdat we met de id op Lead-objecten zoeken.
1. Maak een regel in het veld LeadId, zoals in de onderstaande afbeelding wordt getoond
1. Geef een voorbeeld van het formulier weer en voer een geldige lead-id in het veld en de tab LeadID in. De gebieden Voornaam en Achternaam zouden met de resultaten van de de dienstvraag moeten worden bevolkt.

Het volgende schermschot verklaart de montages van de regelredacteur

![ruleeditor](assets/ruleeditor.jfif)
