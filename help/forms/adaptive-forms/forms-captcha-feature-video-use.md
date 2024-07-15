---
title: CAPTCHA's gebruiken met AEM adaptieve Forms
description: Een CAPTCHA toevoegen en gebruiken met AEM Adaptive Forms.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# CAPTCHA&#39;s gebruiken met AEM adaptieve Forms{#using-captchas-with-aem-adaptive-forms}

Een CAPTCHA toevoegen en gebruiken met AEM Adaptive Forms.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*Deze video loopt door het proces om een CAPTCHA aan een AEM Aangepaste Vorm toe te voegen gebruikend zowel de ingebouwde AEM dienst CAPTCHA evenals de dienst van Google reCAPTCHA.*

>[!NOTE]
>
>Deze functie is alleen beschikbaar vanaf AEM 6.3.

>[!NOTE]
>
>**om reCaptcha op te vormen publiceer instantie gelieve de stappen te volgen**
>
>Vastleggen op auteurinstantie configureren
>
>open de Felix [ Webconsole ](http://localhost:4502/system/console/bundles) op de auteursinstantie
>
>zoeken naar com.adobe.granite.crypto.file bundle
>
>Noteer de bundle-id. Op mijn exemplaar is het 20
>
>Navigeer naar de bundel-id op het bestandssysteem op de auteurinstantie
>
>* &lt;schrijver-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* De HMAC- en hoofdbestanden kopiëren
>
Open de [ felix Webconsole ](http://localhost:4502/system/console/bundles) op uw publiceer instantie. Zoek naar com.adobe.granite.crypto.file bundle. De bundle-id noteren
>
Navigeer naar de bundel-id in het bestandssysteem van uw publicatie-instantie
>
* &lt;publish-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* Verwijder de bestaande HMAC- en hoofdbestanden.
* plak HMAC en hoofddossiers die van de auteursinstantie worden gekopieerd
>
Start AEM publicatieserver opnieuw

## Ondersteunende materialen {#supporting-materials}

* [ Google reCAPTCHA ](https://www.google.com/recaptcha)
