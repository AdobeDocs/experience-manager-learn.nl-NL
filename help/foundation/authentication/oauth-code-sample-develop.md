---
title: OAuth-bereiken ontwikkelen in AEM
description: Adobe Experience Manager die verlengbare OAuth Scopes toestaat voor toegangsbeheer voor middelen van een cliënttoepassing die door een eind - gebruiker wordt toegelaten. Het onderstaande diagram illustreert de aanvraagstroom in de context van AEM.
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---

# OAuth-bereiken ontwikkelen

Het uitbreidbare OAuth werkingsgebied van Adobe Experience Manager staat voor toegangsbeheer voor middelen van een cliënttoepassing toe die door een eind - gebruiker wordt toegelaten. Het onderstaande diagram illustreert de aanvraagstroom in de context van AEM.

![Oauth Scopes Flow](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM biedt drie bereik:

* Profiel
* Offline toegang
* Repliceren

AEM uitbreidbare OAuth-bereiken maken het mogelijk andere aangepaste bereikregels te definiëren. Een aangepast bereik kan bijvoorbeeld worden ontwikkeld en geïmplementeerd voor AEM waarmee een via OAuth geautoriseerde mobiele app kan worden beperkt tot lezen, maar geen elementen mag schrijven.

OAuth is de aangewezen methode om een cliënttoepassing toe te laten aangezien het een toegangstoken in plaats van het vereisen van de geloofsbrieven van een AEM wordt verstrekt aan die toepassing gebruikt.

* [De code weergeven](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
