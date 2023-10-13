---
title: Adaptieve formuliergegevens opslaan in Azure Storage
description: Adaptief formulier maken en configureren om gegevens op te slaan in Azure Storage
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
exl-id: 0b543c6b-9cfd-4fac-b8d0-33153c036f4b
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Alles samenvoegen

We beschikken nu over alle vereiste configuraties/integraties die vereist zijn voor het gebruiksscenario. De laatste stap bestaat uit het maken van een adaptief formulier op basis van het formuliergegevensmodel dat wordt ondersteund door Azure Storage.

Maak en adaptief formulier en zorg ervoor dat dit is gebaseerd op het juiste adaptieve formuliersjabloon. Zo zorgt u ervoor dat de aangepaste code die aan de sjabloon is gekoppeld, altijd wordt uitgevoerd wanneer een adaptief formulier wordt gegenereerd.

## Voorbeeld van adaptief formulier

In het formulier hebben we 2 verborgen velden toegevoegd

* Klodderidentiteitskaart - Dit gebied wordt bevolkt met een GUID wanneer het gebied wordt geïnitialiseerd. De waarde van dit veld wordt gebruikt als bindmiddel om blob-opslag van de formuliergegevens op unieke wijze te identificeren. Dit bindmiddel wordt gebruikt om de formuliergegevens te identificeren.
* Klodder-id geretourneerd - Dit veld wordt gevuld met het resultaat van de serviceaanroep om gegevens op te slaan in Azure Storage. Deze waarde komt overeen met de Blob-id van de vorige stap.

Het formulier heeft de volgende bedrijfsregels

* De knop Formulier opslaan wordt weergegeven wanneer de gebruiker een e-mailadres invoert. Klik op de knop Formulier opslaan om de formuliergegevens in Azure Storage op te slaan. Hierbij wordt gebruikgemaakt van de oproepservice van het formuliergegevensmodel.
* De BlobID die door de oproepservice wordt geretourneerd, wordt opgeslagen in het veld Blob-id. Wanneer deze waarde verandert, wordt een e-mail verzonden naar de aanvrager gebruikend SendGrid. Het e-mailbericht bevat de koppeling voor het openen van het gedeeltelijk ingevulde formulier dat met de blob-id is geïdentificeerd.
* De gebruiker krijgt een bevestigingstekst te zien wanneer de gegevens zijn opgeslagen in Azure Storage

### Volgende stappen

[De voorbeeldelementen implementeren](./deploy-sample-assets.md)
