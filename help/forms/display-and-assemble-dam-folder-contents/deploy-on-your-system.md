---
title: De elementen lokaal implementeren
description: De lesbestanden op uw lokale AEM-instantie implementeren
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Implementeren op uw systeem

Volg de onderstaande stappen om deze kwestie te laten werken aan uw lokale AEM-exemplaar.

* [&#x200B; stelt de Bundel DevelopingWithServiceUser &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=nl-NL) in het ZIP dossier op.

* Voeg de volgende ingang in de Dienst van het Mapper van de Gebruiker van de Dienst Apache Sling **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** toe gebruikend [&#x200B; configMgr &#x200B;](http://localhost:4502/system/console/configMgr).

* [&#x200B; stel de nieuwsbrief bundel &#x200B;](assets/Newsletters.core-1.0.0-SNAPSHOT.jar) op. Deze bundel bevat de code voor het weergeven van de inhoud van de map en het samenstellen van de geselecteerde nieuwsbrief(en).

* [&#x200B; voer het pakket in gebruikend de Manager van het Pakket &#x200B;](assets/newsletter.zip). Dit pakket bevat clientbibliotheek en voorbeeld-PDF-bestanden om de oplossing te testen.

* [&#x200B; voer de steekproef aanpassende vorm &#x200B;](assets/sample-adaptive-form.zip) in. In dit formulier worden de nieuwsbrieven weergegeven die kunnen worden geselecteerd.

* [&#x200B; voorproef de vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selecteer een paar nieuwsbrieven om te downloaden. De geselecteerde nieuwsbrieven zullen in één pdf worden gecombineerd en aan u teruggegeven.
