---
title: Aangepaste formuliertabel vullen
description: De tabel Adaptief formulier vullen met de resultaten van de serviceaanroepen van het formuliergegevensmodel
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 57
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Aangepaste formuliertabel vullen met de resultaten van Inroeping service formuliergegevensmodel

[Live formulier wordt hier gehost](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
In dit artikel bekijken we hoe de tabel met adaptieve formulieren wordt gevuld door gegevens op te halen uit het oproepen van de service van het formuliergegevensmodel. We gaan een aflossingsschema maken in een tabel waarin elke regelmatige betaling op een hypotheek in de loop van de tijd wordt vermeld. De afschrijvingsresultaten worden geretourneerd door ons formuliergegevensmodel. De dienst van het Model van Gegevens van de Vorm wordt aangehaald op de klikgebeurtenis van berekeningsknoop zoals aangetoond in het schermschot. De invoer- en uitvoerparameters van de serviceaanroep worden op de juiste wijze toegewezen, zoals in de schermafbeelding wordt getoond. De uitvoer wordt toegewezen aan de kolommen van Rij1
![clickEvent](assets/amortization.PNG)

Row1 wordt gevormd om afhankelijk van de gegevens te kweken die door de de dienstvraag zijn teruggekeerd. Let op de herhalingsinstellingen die hier zijn opgegeven. De waarde -1 geeft een onbeperkt aantal rijen in de tabel aan
![Rij1](assets/rowconfiguration.PNG)

## Deze op uw server implementeren

[Tomcat installeren zoals hier opgegeven](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Het bestand SampleRest.war uit dit ZIP-bestand in uw Tomcat-bestand implementeren](assets/sample-rest.zip)
[De middelen installeren](assets/amortizationschedule.zip) met AEM pakketbeheer
[Het formulier voor het versieschema openen](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Voer de juiste waarde in en klik op Wijzigingsschema berekenen als het formulier moet worden ingevuld
