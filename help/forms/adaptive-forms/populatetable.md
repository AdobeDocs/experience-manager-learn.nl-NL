---
title: 'Aangepaste formuliertabel vullen '
seo-title: Aangepaste formuliertabel vullen
description: De tabel Adaptief formulier vullen met de resultaten van de serviceaanroepen van het formuliergegevensmodel
seo-description: De tabel Adaptief formulier vullen met de resultaten van de serviceaanroepen van het formuliergegevensmodel
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 4f51f7bf00827210d2631b9335768a9980f6655c
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# Aangepaste formuliertabel vullen met de resultaten van Inroeping service formuliergegevensmodel

[Live formulier wordt hier](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)gehost In dit artikel bekijken we het vullen van een tabel met adaptieve formulieren door gegevens op te halen uit het oproepen van de service van het formuliergegevensmodel. We gaan een aflossingsschema maken in een tabel waarin elke regelmatige betaling op een hypotheek in de loop van de tijd wordt vermeld. De afschrijvingsresultaten worden geretourneerd door ons formuliergegevensmodel. De dienst van het Model van Gegevens van de Vorm wordt aangehaald op de klikgebeurtenis van berekeningsknoop zoals aangetoond in het schermschot. De invoer- en uitvoerparameters van de serviceaanroep worden op de juiste wijze toegewezen, zoals in de schermafbeelding wordt getoond. De uitvoer wordt toegewezen aan de kolommen van Row1![clickEvent](assets/amortization.PNG)

Row1 wordt gevormd om afhankelijk van de gegevens te kweken die door de de dienstvraag zijn teruggekeerd. Let op de herhalingsinstellingen die hier zijn opgegeven. De waarde -1 geeft een onbeperkt aantal rijen in de tabel![Rij1 aan](assets/rowconfiguration.PNG)

## Deze op uw server implementeren

[Installeer Tomcat zoals u hier](/help/forms/ic-print-channel-tutorial/partone.md)[opgeeft Het bestand](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)[SampleRest.war installeren De middelen ](assets/amortizationschedule.zip) met behulp van AEM pakketbeheer[Open het formulier](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)Amortization Schedule Voer de juiste waarde in en klik op calculateAmortization Schedule als het formulier moet worden ingevuld

