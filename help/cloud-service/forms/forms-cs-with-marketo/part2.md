---
title: AEM Forms as a Cloud Service en Marketo integreren (deel 2)
description: Leer hoe u AEM Forms en Marketo kunt integreren met het AEM Forms-formuliergegevensmodel.
feature: Form Data Model,Integration
version: Experience Manager as a Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Source voor gegevens maken

Marketo REST API&#39;s worden geverifieerd met OAuth 2.0 met twee poten. We kunnen eenvoudig een gegevensbron maken met behulp van het wagerbestand dat u in de vorige stap hebt gedownload

## Configuratiecontainer maken

* Aanmelden bij AEM.
* Klik op het hulpmiddelenmenu en dan **Browser van de Configuratie** zoals hieronder getoond

* ![&#x200B; hulpmiddelmenu &#x200B;](assets/datasource3.png)

* Klik op **creeer** en verstrek een betekenisvolle naam zoals hieronder getoond. Controleer of de optie Cloud Configurations (Cloud Configurations) is geselecteerd, zoals hieronder wordt weergegeven

* ![&#x200B; configuratiecontainer &#x200B;](assets/datasource4.png)

## Cloudservices maken

* Navigeer naar het menu Gereedschappen en klik vervolgens op cloudservices -> Gegevensbronnen

* ![&#x200B; wolk-diensten &#x200B;](assets/datasource5.png)

* Selecteer de configuratiecontainer die in de vroegere stap wordt gecreeerd en klik op **creeer** om een nieuwe gegevensbron tot stand te brengen.Verstrek een betekenisvolle naam en de uitgezochte dienst RESTful van de drop-down lijst van het Type van de Dienst en klik **daarna**
* ![&#x200B; nieuw-gegeven-bron &#x200B;](assets/datasource6.png)

* Upload het wagerbestand en geef het giftype, de Client Id, de Client Secret en Access Token-URL op die specifiek zijn voor uw Marketo-instantie, zoals wordt weergegeven in de onderstaande schermafbeelding.

* Test de verbinding en als de verbinding succesvol is zorg ervoor u op de blauwe **creeert** knoop klikt om het proces te beëindigen om de gegevensbron tot stand te brengen.

* ![&#x200B; gegeven-bron-config &#x200B;](assets/datasource1.png)


## Volgende stappen

[Formuliergegevensmodel maken](./part3.md)
