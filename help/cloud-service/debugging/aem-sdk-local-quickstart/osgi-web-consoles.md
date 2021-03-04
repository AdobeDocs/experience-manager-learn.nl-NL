---
title: Foutopsporing AEM SDK met de OSGi-webconsole
description: De lokale QuickStart van de AEM SDK heeft een OSGi-webconsole die een verscheidenheid aan informatie en introspecties in de lokale AEM runtime biedt die nuttig zijn om te begrijpen hoe uw toepassing door, en functies binnen AEM wordt erkend.
feature: Gereedschappen voor ontwikkelaars
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Ontwikkeling
role: Developer
level: Begin, tussenliggend
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---


# Foutopsporing AEM SDK met de OSGi-webconsole

De lokale QuickStart van de AEM SDK heeft een OSGi-webconsole die een verscheidenheid aan informatie en introspecties in de lokale AEM runtime biedt die nuttig zijn om te begrijpen hoe uw toepassing door, en functies binnen AEM wordt erkend.

AEM verstrekt vele consoles OSGi, elk die zeer belangrijke inzichten in verschillende aspecten van AEM verstrekken, nochtans zijn het volgende typisch het nuttigst in het zuiveren van uw toepassing.

## Bundels

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

De bundelconsole is een catalogus van de bundels OSGi, en hun details, opgesteld aan AEM, samen met de ad hoc capaciteit om hen te beginnen en tegen te houden.

De bundelconsole bevindt zich in:

+ Gereedschappen > Bewerkingen > Webconsole > OSGi > Bundels
+ Of rechtstreeks bij: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Klik in elke bundel, verstrekt details die met het zuiveren uw toepassing helpen.

+ De OSGi-bundel valideren is aanwezig
+ Valideren als een OSGi-bundel actief is
+ Bepalen of een OSGi-bundel niet-bevredigde invoer bevat waardoor het niet kan beginnen

## Onderdelen

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

De console van Componenten is een catalogus van alle componenten OSGi die aan AEM worden opgesteld, en verstrekt alle informatie over hen, van hun bepaalde cyclus van het componentenleven OSGi, aan welke diensten OSGi zij kunnen verwijzen naar

De console van Componenten wordt gevestigd bij:

+ Gereedschappen > Bewerkingen > Webconsole > OSGi > Componenten
+ Of rechtstreeks bij: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Belangrijke aspecten die u helpen bij de foutopsporingsactiviteiten:

+ De OSGi-bundel valideren is aanwezig
+ Valideren als een OSGi-bundel actief is
+ Bepalen of een OSGi-bundel niet-bevredigde invoer bevat waardoor het niet kan beginnen
+ Het verkrijgen van PID van de component, om configuraties OSGi voor hen in Git tot stand te brengen
+ Het identificeren van OSGi bezitswaarden verbindend aan de actieve configuratie OSGi

## Verkoopmodellen

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

De Sling Models-console bevindt zich op de volgende locatie:

+ Gereedschappen > Bewerkingen > Webconsole > Status > Modellen verkopen
+ Of rechtstreeks bij: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Belangrijke aspecten die u helpen bij de foutopsporingsactiviteiten:

+ Validatie van modellen van het Verdelen wordt geregistreerd aan het juiste middeltype
+ Validatiemodellen voor splitsingen kunnen worden aangepast aan de hand van de juiste objecten (Resource of SlingHttpRequestServlet)
+ Exporteurs van verkoopmodellen valideren is correct geregistreerd
