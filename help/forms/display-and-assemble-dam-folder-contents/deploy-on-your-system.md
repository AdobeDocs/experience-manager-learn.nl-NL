---
title: De elementen lokaal implementeren
description: De zelfstudie-elementen implementeren op uw lokale AEM
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---

# Implementeren op uw systeem

Volg de onderstaande stappen om deze kwestie te laten werken aan uw lokale AEM.

* [Implementeer de DevelopingWithServiceUser Bundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) in het ZIP-bestand.

* Voeg het volgende item toe aan de Apache Sling Service User Mapper Service **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** met de [configMgr](http://localhost:4502/system/console/configMgr).

* [De bundel met nieuwsbrieven implementeren](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Deze bundel bevat de code voor het weergeven van de inhoud van de map en het samenstellen van de geselecteerde nieuwsbrief(en).

* [Pakket importeren met Package Manager](assets/newsletter.zip). Dit pakket bevat clientbibliotheek en voorbeeld-PDF-bestanden om de oplossing te testen.

* [Het adaptieve voorbeeldformulier importeren](assets/sample-adaptive-form.zip). In dit formulier worden de nieuwsbrieven weergegeven die kunnen worden geselecteerd.

* [Geef een voorbeeld van het formulier weer](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selecteer een paar nieuwsbrieven om te downloaden. De geselecteerde nieuwsbrieven zullen in één pdf worden gecombineerd en aan u teruggegeven.




