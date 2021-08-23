---
title: Triggerwerkstroom AEM HTML5-formulierverzending
seo-title: Trigger AEM workflow voor het verzenden van HTML5-formulieren
description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
seo-description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# Gedeeltelijk ingevuld mobiel formulier downloaden en verzenden naar AEM workflow

Een veel voorkomend geval is dat u de XDP als HTML-bestand kunt weergeven voor activiteiten voor het vastleggen van gegevens. Dit werkt goed als de formulieren eenvoudig zijn en online kunnen worden ingevuld en verzonden. Als het formulier echter complex is en gebruikers het formulier mogelijk niet online kunnen invullen, moeten we de gebruikers de mogelijkheid bieden om interactieve versies van het formulier te downloaden en vervolgens via Acrobat/Reader offline in te vullen. Nadat het formulier is ingevuld, kan de gebruiker het online verzenden.
Voor dit gebruiksgeval moeten de volgende stappen worden uitgevoerd:

* Mogelijkheid om een interactieve/invulbare PDF te genereren met de gegevens die zijn ingevoerd in het mobiele formulier
* PDF-verzending vanuit Acrobat/Reader verwerken
* De Adobe Experience Manager-workflow (AEM) activeren om de verzonden PDF te reviseren

Deze zelfstudie doorloopt de stappen die nodig zijn om het bovenstaande gebruiksgeval te voltooien. Voorbeeldcode en elementen die betrekking hebben op deze zelfstudie zijn [hier beschikbaar.](part-four.md)

In de volgende video ziet u een overzicht van het gebruik

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

