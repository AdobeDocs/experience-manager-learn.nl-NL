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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# CAPTCHA&#39;s gebruiken met AEM adaptieve Forms{#using-captchas-with-aem-adaptive-forms}

Een CAPTCHA toevoegen en gebruiken met AEM Adaptive Forms.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*Deze video doorloopt het proces om een CAPTCHA aan een AEM Aangepast Vorm toe te voegen gebruikend zowel de ingebouwde AEM dienst CAPTCHA als de dienst van Google reCAPTCHA.*

>[!NOTE]
>
>Deze functie is alleen beschikbaar vanaf AEM 6.3.

>[!NOTE]
>
>**Volg de stappen om reCaptcha op een publicatieexemplaar te configureren**
>
>Vastleggen op auteurinstantie configureren
>
>De Felix openen [webconsole](http://localhost:4502/system/console/bundles) op de instantie van de auteur
>
>zoeken naar com.adobe.granite.crypto.file bundle
>
>Noteer de bundle-id. Op mijn exemplaar is het 20
>
>Navigeer naar de bundel-id op het bestandssysteem op de auteurinstantie
>
>* &lt;author-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* Kopieer de HMAC- en master bestanden
>
Open de [felix-webconsole](http://localhost:4502/system/console/bundles) op uw publicatieexemplaar. Zoek naar com.adobe.granite.crypto.file bundle. De bundle-id noteren
Navigeer naar de bundel-id in het bestandssysteem van uw publicatie-instantie
* &lt;publish-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* Verwijder de bestaande HMAC- en master bestanden.
* plak HMAC en master die dossiers van de auteursinstantie worden gekopieerd
>
Start AEM publicatieserver opnieuw

## Ondersteunende materialen {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
