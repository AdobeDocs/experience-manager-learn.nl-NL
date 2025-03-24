---
title: E-mail verzenden met SendGrid
description: Een e-mail activeren met een koppeling naar het opgeslagen formulier
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
duration: 37
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Integreren met SendGrid

Met AEM Forms Data Integration kunt u verschillende gegevensbronnen configureren en verbinden met AEM Forms. Het verstrekt een intuïtieve gebruikersinterface om een verenigd schema van de gegevensvertegenwoordiging van bedrijfsentiteiten en de diensten over verbonden gegevensbronnen tot stand te brengen.

We hebben de SendGrid-API gebruikt om e-mails te verzenden met behulp van een dynamische sjabloon. Men veronderstelt dat u met SendGrid API voor het verzenden van e-mail gebruikend dynamische malplaatjes vertrouwd bent. In deze zelfstudie hebt u een kwikstbestand voor een beschrijving van de API ontvangen.

## De integratie maken

Voer de volgende stappen uit om de integratie tussen AEM Forms en SendGrid te maken

* Creeer een RESTful gegevensbron gebruikend het [ wagerdossier ](./assets/SendGridWithDynamicTemplate.yaml). [ volg deze video voor gedetailleerde instructies ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) bij het creëren van gegevensbronnen in AEM Forms
* Maak een formuliergegevensmodel op basis van de gegevensbron die u in de vorige stap hebt gemaakt.[ volg de gedetailleerde documentatie ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) bij het creëren van het Model van de Gegevens van de Vorm.

Het formuliergegevensmodel dat voor deze zelfstudie is gemaakt, maakt deel uit van de artikelelementen.

### Volgende stappen

[Azure Storage Integration maken](./create-fdm.md)
