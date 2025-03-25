---
title: AEM-workflow activeren op verzending PDF-formulier
description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om de AEM-workflow te activeren
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 342
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# Gedeeltelijk ingevuld mobiel formulier downloaden en verzenden om een AEM-workflow te activeren

Een veel voorkomend geval is dat u de XDP als HTML voor gegevensvastleggingsactiviteiten kunt weergeven. Dit werkt goed als de formulieren eenvoudig zijn en online kunnen worden ingevuld en verzonden. Als het formulier echter complex is en gebruikers het formulier mogelijk niet online kunnen invullen, moeten we de gebruikers de mogelijkheid bieden om interactieve versies van het formulier te downloaden en vervolgens offline in te vullen met Acrobat/Reader. Nadat het formulier is ingevuld, kan de gebruiker het online verzenden.
Voor dit gebruiksgeval moeten de volgende stappen worden uitgevoerd:

* Mogelijkheid om interactieve/invulbare PDF te genereren met de gegevens die zijn ingevoerd in het mobiele formulier
* De PDF-verzending van Acrobat/Reader afhandelen
* Trigger Adobe Experience Manager (AEM)-workflow om de verzonden PDF te bekijken

Deze zelfstudie doorloopt de stappen die nodig zijn voor het uitvoeren van het bovenstaande gebruiksgeval. De code van de steekproef en de activa met betrekking tot dit leerprogramma zijn [ hier beschikbaar.](./deploy-assets.md)

In de volgende video ziet u een overzicht van het gebruik

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)

## Volgende stappen

[Aangepast profiel maken](./custom-profile.md)