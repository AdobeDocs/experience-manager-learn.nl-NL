---
title: Query-interface samenstellen
description: Multi-part zelfstudie om u door de stappen te leiden die betrokken zijn bij het opvragen van formulierverzendingen die zijn opgeslagen in Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Query-interface samenstellen

Er is een eenvoudige query-interface gemaakt waarmee de beheerder zoekcriteria kan invoeren om bepaalde formulierverzendingen op te halen. De resultaten worden vervolgens weergegeven in een eenvoudige tabelindeling met de optie om een bepaald formulier te verzenden.

![vragen](assets/query-submissions.png)

De dropdowns in deze interface zijn trapsgewijs dropdown. De opties die beschikbaar zijn in het vervolgkeuzemenu worden gewijzigd op basis van de selecties die zijn gemaakt in het vorige vervolgkeuzemenu.

De dropdowns zijn bevolkt gebruikend RESTful gegevensbronnen.

De onderzoeksresultaten worden getoond in een douanecomponent genoemd &quot;SearchResults&quot;. Wanneer de gebruiker op de weergaveknop klikt, wordt het formulier voorgevuld met de verzonden gegevens en bijlagen.

## Volgende stappen

[De Prefill-service schrijven](./part4.md)
