---
title: 'Aangepaste formuliertabel vullen '
description: De tabel Adaptief formulier vullen met de resultaten van de serviceaanroepen van het formuliergegevensmodel
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
source-git-commit: 2b7f0f6c34803672cc57425811db89146b38a70a
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---


# Aangepaste formuliertabel vullen met de resultaten van Inroeping service formuliergegevensmodel

[Live formulier wordt ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
hier gehostIn dit artikel bekijken we het vullen van een tabel met adaptieve formulieren door gegevens op te halen uit het oproepen van de service van het formuliergegevensmodel. We gaan een aflossingsschema maken in een tabel waarin elke regelmatige betaling op een hypotheek in de loop van de tijd wordt vermeld. De afschrijvingsresultaten worden geretourneerd door ons formuliergegevensmodel. De dienst van het Model van Gegevens van de Vorm wordt aangehaald op de klikgebeurtenis van berekeningsknoop zoals aangetoond in het schermschot. De invoer- en uitvoerparameters van de serviceaanroep worden op de juiste wijze toegewezen, zoals in de schermafbeelding wordt getoond. De uitvoer wordt toegewezen aan de kolommen van Rij1
![clickEvent](assets/amortization.PNG)

Row1 wordt gevormd om afhankelijk van de gegevens te kweken die door de de dienstvraag zijn teruggekeerd. Let op de herhalingsinstellingen die hier zijn opgegeven. De waarde -1 geeft een onbeperkt aantal rijen in de tabel aan
![Rij1](assets/rowconfiguration.PNG)

## Deze op uw server implementeren

[Installeer Tomcat zoals ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[hier is opgegevenImplementeer het bestand SampleRest.war in dit ZIP-bestand in uw ](assets/sample-rest.zip)
[TomcatInstalleer de middelen  ](assets/amortizationschedule.zip) met AEM pakketbeheer 
[Open het ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
formulier Amortization ScheduleVoer de juiste waarde in en klik op het schema voor Amortization berekenen als het formulier moet worden ingevuld

