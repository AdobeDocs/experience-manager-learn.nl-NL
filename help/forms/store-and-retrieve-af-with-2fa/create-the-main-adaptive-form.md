---
title: Het belangrijkste adaptieve formulier maken
description: De adaptieve formulieren maken om informatie van de aanvrager en het adaptieve formulier op te nemen om het opgeslagen adaptieve formulier op te halen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# Het belangrijkste adaptieve formulier maken

Het formulier **StoreAFWithAttachments** is het meest adaptieve formulier. Dit adaptieve formulier is het ingangspunt voor het gebruiksgeval. In dit formulier worden gebruikersgegevens, waaronder het mobiele nummer, vastgelegd. Dit formulier kan ook enkele bijlagen toevoegen. Wanneer op de knop Opslaan en afsluiten wordt geklikt, wordt de code aan de serverzijde uitgevoerd om de formuliergegevens in de database op te slaan en wordt een unieke toepassings-id gegenereerd en aan de gebruiker gepresenteerd voor een veilige bewaring. Deze toepassings-id wordt gebruikt om het mobiele nummer op te halen dat aan de toepassing is gekoppeld.

![hoofdaanvraagformulier](assets/6552.JPG)

Dit formulier is gekoppeld aan **bootboxjs540,storeAFWithAttachments** -clientbibliotheken die eerder in de cursus zijn gemaakt, en een AEM workflow die wordt geactiveerd bij het verzenden van het formulier.


* De voorbeeldformulieren zijn gebaseerd op een [aangepaste formuliersjabloon](assets/custom-template-with-page-component.zip) dat naar AEM moet worden geïmporteerd om de voorbeeldformulieren correct te kunnen weergeven.

* Het ingevulde [StoreAfWithAttachments-formulier](assets/store-af-with-attachments-form.zip) kan worden gedownload en geïmporteerd in uw AEM-exemplaar.

* Het formulier werkt alleen als de [AEM workflow die aan dit formulier](assets/workflow-model-store-af-with-attachments.zip) is gekoppeld, in uw AEM wordt geïmporteerd.



