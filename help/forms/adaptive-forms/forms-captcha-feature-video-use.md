---
title: CAPTCHA's gebruiken met AEM adaptieve Forms
description: Een CAPTCHA toevoegen en gebruiken met AEM Adaptive Forms.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# CAPTCHA&#39;s gebruiken met AEM adaptieve Forms{#using-captchas-with-aem-adaptive-forms}

Een CAPTCHA toevoegen en gebruiken met AEM Adaptive Forms.

Ga naar de pagina [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse1) voor een koppeling naar een live demo van deze mogelijkheid.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Deze video doorloopt het proces om een CAPTCHA aan een AEM Aangepast Vorm toe te voegen gebruikend zowel de ingebouwde AEM dienst CAPTCHA als de dienst reCAPTCHA van Google.*

>[!NOTE]
>
>Deze functie is alleen beschikbaar vanaf AEM 6.3.

>[!NOTE]
>
>**Volg de stappen om reCaptcha op een publicatieexemplaar te configureren**
>
>Vastleggen op auteurinstantie configureren
>
>Open de Felix [webconsole](http://localhost:4502/system/console/bundles) op de auteurinstantie
>
>zoeken naar com.adobe.granite.crypto.file bundle
>
>Noteer de bundle-id. Op mijn exemplaar is het 20
>
>Navigeer naar de bundel-id op het bestandssysteem op de auteurinstantie
>
>* &lt;author-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* Kopieer de HMAC- en master bestanden

Open de [felix webconsole](http://localhost:4502/system/console/bundles) op uw publicatieexemplaar. Zoek naar com.adobe.granite.crypto.file bundle. De bundle-id noteren
Navigeer naar de bundel-id in het bestandssysteem van uw publicatie-instantie
* &lt;publish-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* Verwijder de bestaande HMAC- en master bestanden.
* plakken de HMAC en de master dossiers die van de auteursinstantie worden gekopieerd

Start AEM publicatieserver opnieuw

## Ondersteunende materialen {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

