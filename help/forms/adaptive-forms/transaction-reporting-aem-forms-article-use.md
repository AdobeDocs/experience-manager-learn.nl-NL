---
title: Transactierapportage in AEM Forms gebruiken
description: Met transactierapporten in AEM Forms kunt u een telling bijhouden van alle transacties die sinds een opgegeven datum op uw AEM Forms-implementatie zijn uitgevoerd.
feature: Adaptieve Forms
version: 6.4.1,6.5
topic: Ontwikkeling
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---


# Transactierapportage in AEM Forms gebruiken{#using-transaction-reporting-in-aem-forms}

Transactierapporten om het aantal verzonden formulieren, het weergeven van documenten met behulp van documentservices en het weergeven van interactieve communicatie (web- en afdrukkanalen) vast te leggen, zijn geïntroduceerd in AEM Forms 6.4.1. Deze mogelijkheid is vooral bedoeld voor klanten die een licentie voor de software willen verkrijgen op basis van het aantal verzonden formulieren en/of documenten. Deze mogelijkheid is momenteel alleen beschikbaar in AEM Forms OSGI-stapels.

## Transactierapportage inschakelen {#enabling-transaction-reporting}

Standaard is het opnemen van transacties uitgeschakeld. Volg onderstaande stappen om het opnemen van transacties in te schakelen:

* [De configMgr openen](http://localhost:4502/system/console/configMgr)
* Zoeken naar &#39;Forms Transaction Reporting&#39;
* Schakel het selectievakje Transacties opnemen in.
* Uw wijzigingen opslaan

Zodra transactierapportage is ingeschakeld, kunt u Adaptief Forms verzenden, documenten genereren met behulp van documentservices of Interactieve communicatiedocumenten genereren om transactierapportering in actie te zien.

## Transactierapport bekijken {#viewing-transaction-report}

Meld u aan bij AEM Forms als beheerder om het transactierapport weer te geven. Alleen leden van de groep fd-beheerder kunnen het transactierapport weergeven.

Gereedschappen selecteren | Forms | Transactierapport weergeven

of bekijk het transactierapport door [hier](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html) te klikken

![TransctionReporting](assets/transactionreporting.gif)

In de schermafbeelding boven Document verwerkt is het aantal documenten dat met documentservices is gegenereerd. Gerenderde documenten is het aantal Interactieve die Communicatie documenten (Web en Druk) worden teruggegeven. Forms verzonden is het aantal Adaptief verzonden formulieren.

Een transactie blijft in de buffer voor een gespecificeerde periode (de tijd van de Buffer van de Duw + omgekeerde replicatietijd). Door gebrek, vergt het ongeveer 90 seconden voor de transactietelling om in het transactierapport te weerspiegelen.

Handelingen als het verzenden van een PDF-formulier, het gebruik van de gebruikersinterface van de agent voor het weergeven van een voorvertoning van een interactieve communicatie of het gebruik van niet-standaardmethoden voor het verzenden van formulieren worden niet als transacties beschouwd. AEM Forms biedt een API om dergelijke transacties op te nemen. Roep API van uw douaneimplementaties aan om een transactie te registreren.

Als u het transactierapport over de auteursinstantie bekijkt, zorg ervoor de omgekeerde replicatie op alle publiceer instanties wordt gevormd.

Voor meer informatie over transactierapporteren [klik hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

