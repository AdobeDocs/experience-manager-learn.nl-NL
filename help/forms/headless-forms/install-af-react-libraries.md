---
title: De vereiste adaptieve reactiebibliotheken voor formulieren installeren
description: Voeg de vereiste gebiedsdelen aan uw reactieproject toe
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
duration: 72
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# De vereiste afhankelijkheden installeren

Installeer de volgende afhankelijkheden in uw reactieproject om te beginnen met het gebruik van adaptieve formulieren zonder kop in uw reactieproject

* @aemforms/af-response-components
* @aemforms/af-response-renderer

Werk package.json bij om de volgende gebiedsdelen te omvatten. Op het moment van schrijven was 0.22.41 de huidige versie

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>De vervolgkeuzelijst en de kaartlay-out in deze zelfstudie zijn gemaakt met [Materiële UI-bibliotheek](https://mui.com/). U moet de juiste Materiële UI-pakketten downloaden om de code op uw systeem te laten werken.

## Proxy instellen

Cross-Origin Resource Sharing (CORS) is een beveiligingsmechanisme dat webbrowsers beperkt om aanvragen in te dienen op een ander domein dan het domein waarop de app wordt gehost. Er kunnen CORS-fouten optreden wanneer u gegevens probeert op te halen van een API die wordt gehost op een ander domein. Door een proxy in te stellen, kunt u de CORS-beperkingen omzeilen en aanvragen indienen bij de API vanuit de React-app. Ik heb de volgende code in een dossier genoemd setUpProxy.js in de src omslag gebruikt. **Zorg ervoor dat u het doel wijzigt zodat dit naar de publicatie-instantie verwijst.**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

U moet ook de **http-proxy-middleware** aan uw project.

## Volgende stappen

[Het in te sluiten formulier ophalen](./fetch-the-form.md)
