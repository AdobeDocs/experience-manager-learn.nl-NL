---
title: Aangepaste formuliertabel vullen
description: De tabel Adaptief formulier vullen met de resultaten van de serviceaanroepen van het formuliergegevensmodel
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 45
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Aangepaste formuliertabel vullen met de resultaten van Inroeping service formuliergegevensmodel

[ Levende vorm wordt hier ontvangen ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
In dit artikel bekijken we hoe de tabel met adaptieve formulieren wordt gevuld door gegevens op te halen uit het oproepen van de service van het formuliergegevensmodel. We gaan een aflossingsschema maken in een tabel waarin elke regelmatige betaling op een hypotheek in de loop van de tijd wordt vermeld. De afschrijvingsresultaten worden geretourneerd door ons formuliergegevensmodel. De dienst van het Model van Gegevens van de Vorm wordt aangehaald op de klikgebeurtenis van berekeningsknoop zoals aangetoond in het schermschot. De invoer- en uitvoerparameters van de serviceaanroep worden op de juiste wijze toegewezen, zoals in de schermafbeelding wordt getoond. De uitvoer wordt toegewezen aan de kolommen van Rij1
![ clickEvent ](assets/amortization.PNG)

Row1 wordt gevormd om afhankelijk van de gegevens te kweken die door de de dienstvraag zijn teruggekeerd. Let op de herhalingsinstellingen die hier zijn opgegeven. De waarde -1 geeft een onbeperkt aantal rijen in de tabel aan
![ Row1 ](assets/rowconfiguration.PNG)

## Deze op uw server implementeren

[ installeer Tomcat zoals hier gespecificeerd ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[ stel het dossier SampleRest.war in dit zip dossier in uw Tomcat ](assets/sample-rest.zip) op
[ installeer de activa ](assets/amortizationschedule.zip) gebruikend AEM pakketmanager
[ open de Vorm van het Programma van de Amortization ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
Voer de juiste waarde in en klik op Berekenen
Amortization-schema moet in het formulier worden ingevuld
