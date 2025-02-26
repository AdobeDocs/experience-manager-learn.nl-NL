---
title: Een configuratie voor cloudservices maken
description: Gegevensbron maken voor verbinding met Salesforce met OAuth-referenties
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms, Integrations
jira: KT-7148
thumbnail: 331755.jpg
exl-id: e2d56e91-c13e-4787-a97f-255938b5d290
duration: 173
source-git-commit: ce22dd482417a54d222165deaf485ff69c2856b7
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 0%

---

# Source voor gegevens maken

Maak een gegevensbron met REST-back-up met het wagerbestand dat u in de vorige stap hebt gemaakt.

>[!VIDEO](https://video.tv.adobe.com/v/331755?quality=12&learn=on)

| Instelling | Waarde |
|---------------------|-----------------------------------------------------------------|
| OAuth URL | https://login.salesforce.com/services/oauth2/authorize |
| Autorisatiebereik | api chatter_api full id openid refresh_token visualforce web |
| Token-URL vernieuwen | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |
| Toegang tot token-URL | https://newfocus-dev-ed.my.salesforce.com/services/oauth2/token |


**Vernieuwen en de domeinnamen van de Symbolische URL van de Toegang zullen moeten veranderen om uw de rekeningsmontages van Salesforce aan te passen**