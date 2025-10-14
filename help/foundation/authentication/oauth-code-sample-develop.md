---
title: OAuth-oppervlakken ontwikkelen in AEM
description: Adobe Experience Manager die verlengbare OAuth Scopes toestaat voor toegangsbeheer voor middelen van een cliënttoepassing die door een eind - gebruiker wordt toegelaten. Het onderstaande diagram illustreert de aanvraagstroom in de context van AEM.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---

# OAuth-bereiken ontwikkelen

Het uitbreidbare OAuth werkingsgebied van Adobe Experience Manager staat voor toegangsbeheer voor middelen van een cliënttoepassing toe die door een eind - gebruiker wordt toegelaten. Het onderstaande diagram illustreert de aanvraagstroom in de context van AEM.

![&#x200B; Oauth Scopes Flow &#x200B;](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM biedt drie bereikregels:

* Profiel
* Offline toegang
* Repliceren

Door het uitbreidbare OAuth-bereik van AEM kunnen andere aangepaste bereiken worden gedefinieerd. Een aangepast bereik kan bijvoorbeeld worden ontwikkeld en geïmplementeerd in AEM, zodat een mobiele app die via OAuth is geautoriseerd, alleen kan worden gelezen, maar geen elementen kan schrijven.

OAuth is de voorkeursmethode voor het autoriseren van een clienttoepassing, aangezien deze een toegangstoken gebruikt in plaats van dat de referenties van een AEM-gebruiker naar die toepassing moeten worden verzonden.

* [&#x200B; Mening de code &#x200B;](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
