---
title: Fouten opsporen in AEM SDK met de OSGi-webconsole
description: De lokale QuickStart van AEM SDK heeft een OSGi-webconsole die verschillende informatie en introspecties in de lokale AEM-runtime biedt die handig zijn om te begrijpen hoe uw toepassing wordt herkend door en werkt binnen AEM.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 486
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Fouten opsporen in AEM SDK met de OSGi-webconsole

De lokale QuickStart van AEM SDK heeft een OSGi-webconsole die verschillende informatie en introspecties in de lokale AEM-runtime biedt die handig zijn om te begrijpen hoe uw toepassing wordt herkend door en werkt binnen AEM.

AEM verstrekt vele consoles OSGi, elk die zeer belangrijke inzichten in verschillende aspecten van AEM verstrekken, nochtans zijn het volgende typisch het nuttigst in het zuiveren van uw toepassing.

## Bundels

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

De bundelconsole is een catalogus van de OSGi-bundels en hun gegevens, geïmplementeerd op AEM, samen met de ad-hocmogelijkheid om ze te starten en te stoppen.

De bundelconsole bevindt zich in:

+ Gereedschappen > Bewerkingen > Webconsole > OSGi > Bundels
+ Of direct bij: [ http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Klik in elke bundel, verstrekt details die met het zuiveren uw toepassing helpen.

+ De OSGi-bundel valideren is aanwezig
+ Valideren als een OSGi-bundel actief is
+ Bepalen of een OSGi-bundel niet-bevredigde invoer bevat waardoor het niet kan beginnen

## Onderdelen

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

De console van Componenten is een catalogus van alle componenten OSGi die aan AEM worden opgesteld, en verstrekt alle informatie over hen, van hun bepaalde cyclus van het de componentenleven OSGi, aan welke diensten OSGi zij kunnen verwijzen naar

De console van Componenten wordt gevestigd bij:

+ Gereedschappen > Bewerkingen > Webconsole > OSGi > Componenten
+ Of direct bij: [ http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Belangrijke aspecten die u helpen bij de foutopsporingsactiviteiten:

+ De OSGi-bundel valideren is aanwezig
+ Valideren als een OSGi-bundel actief is
+ Bepalen of een OSGi-bundel niet-bevredigde invoer bevat waardoor het niet kan beginnen
+ Het verkrijgen van PID van de component, om configuraties OSGi voor hen in Git te creëren
+ Het identificeren van OSGi bezitswaarden verbindend aan de actieve configuratie OSGi

## Verkoopmodellen

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

De Sling Models-console bevindt zich in:

+ Gereedschappen > Bewerkingen > Webconsole > Status > Modellen verkopen
+ Of direct bij: [ http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Belangrijke aspecten die u helpen bij de foutopsporingsactiviteiten:

+ Validatie van modellen van het Verdelen wordt geregistreerd aan het juiste middeltype
+ Validatiemodellen voor splitsingen zijn aanpasbaar vanuit de juiste objecten (Resource of SlingHttpRequestServlet)
+ Exporteurs van verkoopmodellen valideren is correct geregistreerd
