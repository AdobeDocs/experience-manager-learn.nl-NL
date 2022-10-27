---
title: Triggerwerkstroom AEM HTM5 Form Submission Introduction
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Gedeeltelijk ingevuld mobiel formulier downloaden en verzenden naar AEM workflow

Een algemeen gebruiksgeval moet de capaciteit hebben om XDP als HTML voor de activiteiten van de gegevensvangst terug te geven. Dit werkt goed als de formulieren eenvoudig zijn en online kunnen worden ingevuld en verzonden. Als het formulier echter complex is en gebruikers het formulier mogelijk niet online kunnen invullen, moeten we de gebruikers de mogelijkheid bieden om interactieve versies van het formulier te downloaden en vervolgens via Acrobat/Reader offline in te vullen. Nadat het formulier is ingevuld, kan de gebruiker het online verzenden.
Voor dit gebruiksgeval moeten de volgende stappen worden uitgevoerd:

* Mogelijkheid om interactieve/invulbare PDF te genereren met de gegevens die zijn ingevoerd in het mobiele formulier
* De PDF-verzending van Acrobat/Reader afhandelen
* De Adobe Experience Manager-workflow (AEM) activeren om de verzonden PDF te bekijken

Deze zelfstudie doorloopt de stappen die nodig zijn om het bovenstaande gebruiksgeval te voltooien. Voorbeeldcode en elementen die aan deze zelfstudie zijn gerelateerd, zijn [hier beschikbaar.](part-four.md)

In de volgende video ziet u een overzicht van het gebruik

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)
