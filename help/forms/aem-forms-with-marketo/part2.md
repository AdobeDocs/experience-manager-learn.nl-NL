---
title: AEM Forms met Marketo (Deel 2)
description: Zelfstudie voor de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
duration: 137
source-git-commit: 8bde459ae9a6e261cfc3aff308babe9de6e56059
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---

# Source voor gegevens maken

Marketo REST API&#39;s worden geverifieerd met OAuth 2.0 met twee poten. We kunnen eenvoudig een gegevensbron maken met behulp van het wagerbestand dat u in de vorige stap hebt gedownload

## Configuratiecontainer maken

* Meld u aan bij AEM.
* Klik op het hulpmiddelenmenu en dan **Browser van de Configuratie** zoals hieronder getoond

* ![ hulpmiddelmenu ](assets/datasource3.png)

* Klik op **creeer** en verstrek een betekenisvolle naam zoals hieronder getoond. Controleer of de optie Cloud Configurations (Cloud Configurations) is geselecteerd, zoals hieronder wordt weergegeven

* ![ configuratiecontainer ](assets/datasource4.png)

## Cloudservices maken

* Navigeer naar het menu Gereedschappen en klik vervolgens op cloudservices -> Gegevensbronnen

* ![ wolk-diensten ](assets/datasource5.png)

* Selecteer de configuratiecontainer die in de vroegere stap wordt gecreeerd en klik op **creeer** om een nieuwe gegevensbron tot stand te brengen.Verstrek een betekenisvolle naam en de uitgezochte dienst RESTful van de drop-down lijst van het Type van de Dienst en klik **daarna**
* ![ nieuw-gegeven-bron ](assets/datasource6.png)

* Upload het wagerbestand en geef het giftype, de Client Id, de Client Secret en Access Token-URL op die specifiek zijn voor uw Marketo-instantie, zoals wordt weergegeven in de onderstaande schermafbeelding.

* Test de verbinding en als de verbinding succesvol is zorg ervoor u op de blauwe **creeert** knoop klikt om het proces te beëindigen om de gegevensbron tot stand te brengen.

* ![ gegeven-bron-config ](assets/datasource1.png)


## Volgende stappen

[RESTful de dienstgebaseerde gegevensbron tot stand brengen](./part3.md)
