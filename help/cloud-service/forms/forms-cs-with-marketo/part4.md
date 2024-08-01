---
title: AEM Forms met Marketo (Deel 4)
description: Zelfstudie voor de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 68
source-git-commit: 835e76695824cc1f155720567ca104a50be4bab8
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Integratie testen

We testen de integratie door een eenvoudige formulieropname te maken en een hoofdobject van Market weer te geven.
>[!NOTE]
>
>Deze functionaliteit is getest op formulier op basis van basiscomponenten.

## Adaptief formulier maken

1. Maak een adaptief formulier en baseer dit op &quot;Lege formuliersjabloon&quot; en koppel het formulier aan het formuliergegevensmodel dat u in de vorige stap hebt gemaakt.
1. Open het formulier in de bewerkingsmodus.
1. Sleep een component TextField en een component Panel naar het adaptieve formulier. Stel de titel van de component TextField &quot;Enter Lead Id&quot; in en stel de naam ervan in op &quot;LeadId&quot;
1. Sleep 2 TextField-componenten naar de component Panel.
1. Plaats de Naam en de Titel van de 2 componenten TextField als FirstName en LastName
1. Vorm de component van het Comité om een herhaalbare component te zijn door Minimum aan 1 en Maximum aan -1 te plaatsen. Dit is vereist omdat de Marketo-service een array van hoofdobjecten retourneert en u een herhaalbare component nodig hebt om de resultaten weer te geven. In dit geval krijgen we echter maar één object Lead terug, omdat we op Lead-objecten zoeken aan de hand van de id.
1. Maak een regel in het veld LeadId, zoals in de onderstaande afbeelding wordt getoond
1. Geef een voorbeeld van het formulier weer en voer een geldige lead-id in het veld en de tab LeadID in. De gebieden Voornaam en Achternaam zouden met de resultaten van de de dienstvraag moeten worden bevolkt.

Het volgende schermschot verklaart de montages van de regelredacteur

![ ruleeditor ](assets/ruleeditor.png)


## Gefeliciteerd

U hebt AEM Forms met Marketo geïntegreerd via het AEM Forms-formuliergegevensmodel.