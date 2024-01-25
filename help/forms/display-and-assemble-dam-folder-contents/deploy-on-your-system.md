---
title: De elementen lokaal implementeren
description: De zelfstudie-elementen implementeren op uw lokale AEM
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 32
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Implementeren op uw systeem

Volg de onderstaande stappen om deze kwestie te laten werken aan uw lokale AEM.

* [Implementeer de DevelopingWithServiceUser Bundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) in het ZIP-bestand.

* Voeg het volgende item toe aan de Apache Sling Service User Mapper Service **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** met de [configMgr](http://localhost:4502/system/console/configMgr).

* [De bundel met nieuwsbrieven implementeren](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Deze bundel bevat de code voor het weergeven van de inhoud van de map en het samenstellen van de geselecteerde nieuwsbrief(en).

* [Pakket importeren met Package Manager](assets/newsletter.zip). Dit pakket bevat clientbibliotheek en voorbeeld-PDF-bestanden om de oplossing te testen.

* [Het adaptieve voorbeeldformulier importeren](assets/sample-adaptive-form.zip). In dit formulier worden de nieuwsbrieven weergegeven die kunnen worden geselecteerd.

* [Een voorbeeld van het formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selecteer een paar nieuwsbrieven om te downloaden. De geselecteerde nieuwsbrieven zullen in één pdf worden gecombineerd en aan u teruggegeven.
