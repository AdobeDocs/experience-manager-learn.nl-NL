---
title: Problemen oplossen met het ondertekenen van meerdere documenten
description: De oplossing testen en problemen oplossen
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 1%

---


# Testen en problemen oplossen


## Een voorbeeld van het herfinancieringsformulier bekijken

De gebruikscase wordt geactiveerd wanneer de medewerker van de klantenservice het formulier [herfinanciert en invult.](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled)

De workflow Meerdere Forms ondertekenen leidt tot het verzenden van dit formulier en de klant ontvangt een e-mailmelding met een koppeling om het invullen en ondertekenen van het formulier te starten.

## Formulieren invullen in het pakket

De klant wordt voorgesteld om het eerste formulier in het pakket in te vullen en te ondertekenen. Nadat het formulier is ondertekend, kan de klant naar het volgende formulier in het pakket navigeren. Nadat alle formulieren zijn ingevuld en ondertekend, wordt de klant het formulier &quot;**AllDone**&quot; aangeboden.

## Problemen oplossen

### E-mailmelding wordt niet gegenereerd

De e-mailmelding wordt verzonden door de component E-mail verzenden in de workflow Meerdere formulieren ondertekenen. Als een van de stappen in deze workflow mislukt, wordt het e-mailbericht verzonden. Zorg ervoor dat de stap van het aangepaste proces in de workflow rijen maakt in uw MySQL-database. Als de rijen worden gecreeerd dan controleer uw de configuratiemontages van de Dienst van de Post van de Dag CQ

### De koppeling in de e-mailmelding werkt niet

De koppelingen in de e-mailmeldingen worden dynamisch gegenereerd. Als uw AEM server niet op localhost wordt uitgevoerd:4502, geef dan de juiste servernaam en poort op in de argumenten van de stap Ondertekenen door Forms van winkel van de workflow Meerdere Forms ondertekenen

### Kan het formulier niet ondertekenen

Dit kan gebeuren als het formulier niet correct is ingevuld door de gegevens van de gegevensbron op te halen. Controleer de stdout-logboeken van uw server. De fetchformdata.jsp schrijft wat nuttige berichten aan de stdout.

### Kan niet naar het volgende formulier in het pakket navigeren

Wanneer een formulier met succes is ondertekend in het pakket, wordt de workflow Handtekeningstatus bijwerken geactiveerd. De eerste stap in de workflow werkt de status van de handtekening van het formulier in de database bij. Controleer of de status voor het formulier is bijgewerkt van 0 tot en met 1.

### Het AllDone-formulier niet zien

Wanneer het pakket geen formulieren meer bevat om te ondertekenen, wordt het AllDone-formulier aan de gebruiker gepresenteerd. Als u het AllDone-formulier niet ziet, controleert u de URL die wordt gebruikt in regel 33 van het GetNextFormToSign.js-bestand dat deel uitmaakt van de **getnextform** client lib.











