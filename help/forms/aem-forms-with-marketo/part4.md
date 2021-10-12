---
title: AEM Forms with Marketo(Part 4)
description: Zelfstudie voor de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptive Forms, Form Data Model
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: 020852f16de0cdb1e17e19ad989dabf37b7f61f5
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Adaptief formulier maken met formuliergegevensmodel

De volgende stap bestaat uit het maken van een adaptief formulier en het baseren op het formuliergegevensmodel dat in de vorige stap is gemaakt.
The user will enter the Lead Id  and on tabbing out the Marketo service to get the leads by id will be invoked. The results of the service operation are then mapped to the appropriate fields of the Adaptive Forms.

1. Create an Adaptive Form and base it on &quot;Blank Form Template&quot;, associate it with the Form Data Model created in the earlier step.
1. Het formulier openen in de bewerkingsmodus
1. Sleep een component TextField en een component Panel naar het adaptieve formulier. Stel de titel van de component TextField &quot;Enter Lead Id&quot; in en stel de naam ervan in op &quot;LeadId&quot;
1. Sleep 2 TextField-componenten naar de component Panel.
1. Plaats de Naam en de Titel van de 2 componenten TextField als FirstName en LastName
1. Vorm de component van het Comité om een herhaalbare component te zijn door Minimum aan 1 en Maximum aan -1 te plaatsen. Dit is vereist omdat de Marketo-service een array van hoofdobjecten retourneert en u een herhaalbare component nodig hebt om de resultaten weer te geven. In dit geval krijgen we echter maar één object Lead terug, omdat we met de id op Lead-objecten zoeken.
1. Maak een regel in het veld LeadId, zoals in de onderstaande afbeelding wordt getoond
1. Geef een voorbeeld van het formulier weer en voer een geldige lead-id in het veld en de tab LeadID in. De gebieden Voornaam en Achternaam zouden met de resultaten van de de dienstvraag moeten worden bevolkt.

The following screenshot explains the rule editor settings

![ruleeditor](assets/ruleeditor.jfif)

## Foutopsporing

Als u de bundels gebruikt die bij dit artikel worden geleverd, kunt u [debug logboeken](http://localhost:4502/system/console/slinglog) voor de volgende klassen willen toelaten:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
