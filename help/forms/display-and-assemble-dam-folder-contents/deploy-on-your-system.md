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
duration: 27
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Implementeren op uw systeem

Volg de onderstaande stappen om deze kwestie te laten werken aan uw lokale AEM.

* [ stelt de Bundel DevelopingWithServiceUser ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) in het ZIP dossier op.

* Voeg de volgende ingang in de Dienst van het Mapper van de Gebruiker van de Dienst Apache Sling **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** toe gebruikend [ configMgr ](http://localhost:4502/system/console/configMgr).

* [ stel de nieuwsbrief bundel ](assets/Newsletters.core-1.0.0-SNAPSHOT.jar) op. Deze bundel bevat de code voor het weergeven van de inhoud van de map en het samenstellen van de geselecteerde nieuwsbrief(en).

* [ voer het pakket in gebruikend de Manager van het Pakket ](assets/newsletter.zip). Dit pakket bevat clientbibliotheek en voorbeeld-PDF-bestanden om de oplossing te testen.

* [ voer de steekproef aanpassende vorm ](assets/sample-adaptive-form.zip) in. In dit formulier worden de nieuwsbrieven weergegeven die kunnen worden geselecteerd.

* [ voorproef de vorm ](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selecteer een paar nieuwsbrieven om te downloaden. De geselecteerde nieuwsbrieven zullen in één pdf worden gecombineerd en aan u teruggegeven.
