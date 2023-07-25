---
title: E-mail verzenden met SendGrid
description: Een e-mail activeren met een koppeling naar het opgeslagen formulier
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 0%

---

# Integreren met SendGrid

Met AEM Forms Data Integration kunt u verschillende gegevensbronnen configureren en verbinden met AEM Forms. Het verstrekt een intuïtieve gebruikersinterface om een verenigd schema van de gegevensvertegenwoordiging van bedrijfsentiteiten en de diensten over verbonden gegevensbronnen tot stand te brengen.

We hebben de SendGrid-API gebruikt om e-mails te verzenden met behulp van een dynamische sjabloon. Aangenomen wordt dat u bekend bent met SendGrid API voor het verzenden van e-mails met behulp van dynamische sjablonen. In deze zelfstudie hebt u een kwikstbestand voor een beschrijving van de API ontvangen.

## De integratie maken

Voer de volgende stappen uit om de integratie tussen AEM Forms en SendGrid te maken

* Een RESTful-gegevensbron maken met de [wagenbestand](./assets/SendGridWithDynamicTemplate.yaml). [Volg deze video voor gedetailleerde instructies](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) over het maken van gegevensbronnen in AEM Forms
* Maak een formuliergegevensmodel op basis van de gegevensbron die u in de vorige stap hebt gemaakt.[Volg de gedetailleerde documentatie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) op het maken van een formuliergegevensmodel.

Het formuliergegevensmodel dat voor deze zelfstudie is gemaakt, maakt deel uit van de artikelelementen.

### Volgende stappen

[Azure Storage Integration maken](./create-fdm.md)


