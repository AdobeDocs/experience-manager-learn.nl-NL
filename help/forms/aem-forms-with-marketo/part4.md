---
title: AEM Forms met Marketo (Deel 4)
description: Zelfstudie voor de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '317'
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
1. Geef een voorbeeld van het formulier weer en voer een geldige lead-id in het veld en de tab LeadID in. De velden Voornaam en Achternaam moeten worden gevuld met de resultaten van de serviceaanroep.

Het volgende schermschot verklaart de montages van de regelredacteur

![ruleeditor](assets/ruleeditor.jfif)

## Foutopsporing

Als u de bundels gebruikt die bij dit artikel worden geleverd, kunt u [foutopsporingslogboeken](http://localhost:4502/system/console/slinglog) voor de volgende klassen:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
