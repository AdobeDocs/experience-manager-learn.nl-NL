---
title: Het belangrijkste adaptieve formulier maken
description: De adaptieve formulieren maken om informatie van de aanvrager en het adaptieve formulier op te nemen om het opgeslagen adaptieve formulier op te halen
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---


# Het belangrijkste adaptieve formulier maken

Het formulier **StoreAFWithAttachments** is het belangrijkste adaptieve formulier. Dit adaptieve formulier is het ingangspunt voor het gebruiksgeval. In dit formulier worden gebruikersgegevens, waaronder het mobiele nummer, vastgelegd. Dit formulier kan ook enkele bijlagen toevoegen. Wanneer op de knop Opslaan en afsluiten wordt geklikt, wordt de code aan de serverzijde uitgevoerd om de formuliergegevens in de database op te slaan en wordt een unieke toepassings-id gegenereerd en aan de gebruiker gepresenteerd voor een veilige bewaring. Deze toepassings-id wordt gebruikt om het mobiele nummer op te halen dat aan de toepassing is gekoppeld.

![hoofdaanvraagformulier](assets/6552.JPG)

Dit formulier is gekoppeld aan **bootboxjs540,storeAFWithAttachments** clientbibliotheken die eerder in de cursus zijn gemaakt en een AEM workflow die wordt geactiveerd bij het verzenden van formulieren.


* De voorbeeldformulieren zijn gebaseerd op aangepaste formuliersjablonen [die in AEM moeten worden geïmporteerd om de voorbeeldformulieren correct te kunnen weergeven.](assets/custom-template-with-page-component.zip)

* Het voltooide [FormAfWithAttachments Form](assets/store-af-with-attachments-form.zip) kan worden gedownload en in uw AEM-instantie worden geïmporteerd.

* De [AEM workflow die aan dit formulier is gekoppeld, moet worden geïmporteerd in uw AEM exemplaar, anders werkt het formulier niet.](assets/workflow-model-store-af-with-attachments.zip)



