---
title: Formulierverzending aanvragen
description: Multi-part zelfstudie om u door de stappen te leiden die betrokken zijn bij het opvragen van formulierverzendingen die zijn opgeslagen in Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: b814097c-0066-44da-bba5-6914dee0df75
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 0%

---

# Aangepaste verzending maken

Er is een aangepaste verzendhandler geschreven voor het verzenden van formulieren. Op een hoog niveau doet de aangepaste verzendhandler het volgende

* Extraheert de naam van het verzonden formulier.
* Extraheert de verzonden gegevens. De verzonden gegevens van een formulier op basis van een kerncomponent hebben altijd de JSON-indeling.
* Hiermee worden de formulierbijlagen opgehaald en opgeslagen in het Azure-portaal. Hiermee werkt u de ingediende JSON-gegevens bij met de URL van de bijlage.
* Hiermee maakt u blob-indexcodes. Hiermee zoekt u naar de lijst met doorzoekbare velden voor het formulier en de bijbehorende waarde van de verzonden gegevens.
* Koppelt de blob-indextags aan de verzonden gegevens en slaat deze op in de Azure-portal.

Het volgende schermschot toont u de blob indexmarkeringen in Azure portaal

![&#x200B; blob-index-markeringen &#x200B;](assets/blob-index-tags.png)

De douane legt code voor is in **_StoreFormDataWithBlobIndexTagsInAzure_** en de code voor het opslaan van en het terugwinnen van gegevens van Azure is in de component **_SaveAndFetchFromAzure_**

## Volgende stappen

[Query-interface samenstellen](./part3.md)
