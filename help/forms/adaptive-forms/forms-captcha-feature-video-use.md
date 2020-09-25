---
title: CAPTCHA's gebruiken met AEM adaptieve Forms
seo-title: CAPTCHA's gebruiken met AEM adaptieve Forms
description: Een CAPTCHA toevoegen en gebruiken met AEM Adaptive Forms.
seo-description: Een CAPTCHA toevoegen en gebruiken met AEM Adaptive Forms.
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---


# CAPTCHA&#39;s gebruiken met AEM adaptieve Forms{#using-captchas-with-aem-adaptive-forms}

Een CAPTCHA toevoegen en gebruiken met AEM Adaptive Forms.

Ga naar de voorbeeldpagina [van](https://forms.enablementadobe.com/content/samples/samples.html?query=0) AEM Forms voor een koppeling naar een live demo over deze mogelijkheid.

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
>open de felix [Webconsole](http://localhost:4502/system/console/bundles) op de auteursinstantie
>
>zoeken naar com.adobe.granite.crypto.file bundle
>
>Noteer de bundle-id. Op mijn exemplaar is het 20
>
>Navigeer naar de bundel-id op het bestandssysteem op de auteurinstantie
>
>* &lt;schrijver-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* Kopieer de HMAC- en master bestanden

Open de [felix-webconsole](http://localhost:4502/system/console/bundles) op uw publicatieexemplaar. Zoek naar com.adobe.granite.crypto.file bundle. De bundle-id noteren
Navigeer naar de bundel-id in het bestandssysteem van uw publicatie-instantie
* &lt;publish-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* Verwijder de bestaande HMAC- en master bestanden.
* plak HMAC en master die dossiers van de auteursinstantie worden gekopieerd

Start AEM publicatieserver opnieuw

## Ondersteunende materialen {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

