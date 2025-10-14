---
title: Het belangrijkste adaptieve formulier maken
description: De adaptieve formulieren maken om informatie van de aanvrager en het adaptieve formulier op te nemen om het opgeslagen adaptieve formulier op te halen
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 41
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Het belangrijkste adaptieve formulier maken

De vorm **StoreAFWithAttachments** is de belangrijkste adaptieve vorm. Dit adaptieve formulier is het ingangspunt voor het gebruiksgeval. In dit formulier worden gebruikersgegevens, waaronder het mobiele nummer, vastgelegd. Dit formulier kan ook enkele bijlagen toevoegen. Wanneer op de knop Opslaan en afsluiten wordt geklikt, wordt de code aan de serverzijde uitgevoerd om de formuliergegevens in de database op te slaan en wordt een unieke toepassings-id gegenereerd en aan de gebruiker gepresenteerd voor een veilige bewaring. Deze toepassings-id wordt gebruikt om het mobiele nummer op te halen dat aan de toepassing is gekoppeld.

![&#x200B; belangrijkste toepassingsvorm &#x200B;](assets/6552.JPG)

Deze vorm wordt geassocieerd met **bootboxjs540, storeAFWithAttachments** cliëntbibliotheken die vroeger in de cursus en een werkschema van AEM worden gecreeerd die op vormvoorlegging teweeggebracht.


* De steekproefvormen zijn gebaseerd op [&#x200B; douane adaptieve vormmalplaatje &#x200B;](assets/custom-template-with-page-component.zip) dat in AEM voor de steekproefvormen moet worden ingevoerd om correct terug te geven.

* De voltooide [&#x200B; Vorm StoreAfWithAttachments &#x200B;](assets/store-af-with-attachments-form.zip) kan in uw instantie van AEM worden gedownload en worden ingevoerd.

* Het [&#x200B; werkschema van AEM verbonden aan deze vorm &#x200B;](assets/workflow-model-store-af-with-attachments.zip) moet in uw instantie van AEM voor de vorm worden ingevoerd om te werken.


## Volgende stappen

[Het formulier maken dat het opgeslagen formulier ophaalt](./retrieve-saved-form.md)