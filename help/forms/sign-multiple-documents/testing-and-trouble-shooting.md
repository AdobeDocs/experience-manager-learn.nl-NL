---
title: Problemen oplossen met het ondertekenen van meerdere documenten
description: De oplossing testen en problemen oplossen
feature: Adaptive Forms
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# Testen en problemen oplossen


## Een voorbeeld van het herfinancieringsformulier bekijken

Het gebruiksgeval wordt teweeggebracht wanneer de agent van de klantendienst vult en voorlegt [herfinancieringsformulier](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

De workflow Meerdere Forms ondertekenen leidt tot het verzenden van dit formulier en de klant ontvangt een e-mailmelding met een koppeling om het invullen en ondertekenen van het formulier te starten.

## Formulieren invullen in het pakket

De klant wordt voorgesteld om het eerste formulier in het pakket in te vullen en te ondertekenen. Nadat het formulier is ondertekend, kan de klant naar het volgende formulier in het pakket navigeren. Nadat alle formulieren zijn ingevuld en ondertekend, wordt de klant de &quot;**AllDone**&quot;formulier.

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

Wanneer het pakket geen formulieren meer bevat om te ondertekenen, wordt het AllDone-formulier aan de gebruiker gepresenteerd. Als u het AllDone-formulier niet ziet, controleert u de URL die wordt gebruikt in regel 33 van het GetNextFormToSign.js-bestand dat deel uitmaakt van het **getnextform** client lib.
