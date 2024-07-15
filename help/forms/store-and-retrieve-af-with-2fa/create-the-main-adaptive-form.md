---
title: Het belangrijkste adaptieve formulier maken
description: De adaptieve formulieren maken om informatie van de aanvrager en het adaptieve formulier op te nemen om het opgeslagen adaptieve formulier op te halen
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 41
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Het belangrijkste adaptieve formulier maken

De vorm **StoreAFWithAttachments** is de belangrijkste adaptieve vorm. Dit adaptieve formulier is het ingangspunt voor het gebruiksgeval. In dit formulier worden gebruikersgegevens, waaronder het mobiele nummer, vastgelegd. Dit formulier kan ook enkele bijlagen toevoegen. Wanneer op de knop Opslaan en afsluiten wordt geklikt, wordt de code aan de serverzijde uitgevoerd om de formuliergegevens in de database op te slaan en wordt een unieke toepassings-id gegenereerd en aan de gebruiker gepresenteerd voor een veilige bewaring. Deze toepassings-id wordt gebruikt om het mobiele nummer op te halen dat aan de toepassing is gekoppeld.

![ belangrijkste toepassingsvorm ](assets/6552.JPG)

Deze vorm wordt geassocieerd met **bootboxjs540, storeAFWithAttachments** cliëntbibliotheken die vroeger in de cursus en een AEM worden gecreeerd die op vormvoorlegging teweeggebracht.


* De steekproefvormen zijn gebaseerd op [ douane adaptieve vormmalplaatje ](assets/custom-template-with-page-component.zip) dat in AEM voor de steekproefvormen moet worden ingevoerd om correct terug te geven.

* De voltooide [ Vorm StoreAfWithAttachments ](assets/store-af-with-attachments-form.zip) kan in uw AEM instantie worden gedownload en worden ingevoerd.

* Het [ AEM werkschema verbonden aan deze vorm ](assets/workflow-model-store-af-with-attachments.zip) moet in uw AEM instantie worden ingevoerd opdat de vorm werkt.


## Volgende stappen

[Het formulier maken dat het opgeslagen formulier ophaalt](./retrieve-saved-form.md)