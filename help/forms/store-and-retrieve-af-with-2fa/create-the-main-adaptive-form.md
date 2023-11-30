---
title: Het belangrijkste adaptieve formulier maken
description: De adaptieve formulieren maken om informatie van de aanvrager en het adaptieve formulier op te nemen om het opgeslagen adaptieve formulier op te halen
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Het belangrijkste adaptieve formulier maken

Het formulier **StoreAFWithAttachments** is het belangrijkste adaptieve formulier. Dit adaptieve formulier is het ingangspunt voor het gebruiksgeval. In dit formulier worden gebruikersgegevens, waaronder het mobiele nummer, vastgelegd. Dit formulier kan ook enkele bijlagen toevoegen. Wanneer op de knop Opslaan en afsluiten wordt geklikt, wordt de code aan de serverzijde uitgevoerd om de formuliergegevens in de database op te slaan en wordt een unieke toepassings-id gegenereerd en aan de gebruiker gepresenteerd voor een veilige bewaring. Deze toepassings-id wordt gebruikt om het mobiele nummer op te halen dat aan de toepassing is gekoppeld.

![hoofdaanvraagformulier](assets/6552.JPG)

Dit formulier is gekoppeld aan **bootboxjs540,storeAFWithAttachments** clientbibliotheken die eerder in de cursus zijn gemaakt en een AEM die tijdens het verzenden van het formulier worden geactiveerd.


* De voorbeeldformulieren zijn gebaseerd op [aangepaste adaptieve formuliersjabloon](assets/custom-template-with-page-component.zip) die in AEM moeten worden geïmporteerd om de voorbeeldformulieren correct te kunnen weergeven.

* De voltooide [FormStoreAfWithAttachments](assets/store-af-with-attachments-form.zip) kan worden gedownload en geïmporteerd in uw AEM.

* De [Aan dit formulier gekoppelde AEM](assets/workflow-model-store-af-with-attachments.zip) het formulier werkt alleen als het in uw AEM exemplaar is geïmporteerd.


## Volgende stappen

[Het formulier maken dat het opgeslagen formulier ophaalt](./retrieve-saved-form.md)