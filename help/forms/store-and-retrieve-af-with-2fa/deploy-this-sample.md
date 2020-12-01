---
title: Het voorbeeld implementeren
description: Gebruiksscenario's uitvoeren op uw lokale AEM Forms-instantie
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---



# Het voorbeeld implementeren

Volg de onderstaande instructies om deze gebruiksaanwijzing op uw systeem te laten werken:

>[!NOTE]
>Aangenomen wordt dat u AEM Forms uitvoert op poort 4502.


## Database maken

In dit voorbeeld wordt de MySQL-database gebruikt om de adaptieve formuliergegevens op te slaan. U zult het [gegevensbestandschema moeten tot stand brengen door het schemadossier](assets/data-base-schema.sql) in MySQL werkbank in te voeren.

## Gegevensbron maken

U moet een gegevensbron creëren genoemd **StoreAndRetrieveAfData**. De code in de bundel OSGi gebruikt deze datasource naam

## Formuliergegevensmodel maken

Het Model van de Gegevens van de vorm moet op deze gegevensbron worden gecreeerd genoemd **StoreAndRetrieveAfData**. Dit formuliergegevensmodel wordt gebruikt om het mobiele-telefoonnummer op te halen dat aan de toepassings-id is gekoppeld. Het formuliergegevensmodel kan [hier worden gedownload.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Ontwikkelaarsaccount maken met nexmo

Maak een ontwikkelaarsaccount met [Nexmo](https://dashboard.nexmo.com/) voor het verzenden en verifiëren van OTP-codes. Noteer de API-sleutel en de API-beveiligingssleutel. Het gegevensbron- en formuliergegevensmodel zijn al voor u gemaakt op basis van deze service en worden opgenomen met de elementen die in de vorige stap zijn vermeld.

## De volgende OSGi-bundels implementeren

Implementeer de bundel die de [code bevat om gegevens uit de database op te slaan en op te halen](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
Implementeer [DevelopingWithServiceUser Bundle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).

## De clientbibliotheek implementeren

In het voorbeeld worden twee clientbibliotheken gebruikt. Importeer deze [clientbibliotheken](assets/client-libraries.zip) in AEM.

## De aangepaste adaptieve formuliersjabloon importeren

De voorbeeldformulieren die in deze demo worden gebruikt, zijn gebaseerd op een aangepaste sjabloon. Importeer de [aangepaste sjabloon in AEM](assets/custom-template-with-page-component.zip)

## De adaptieve voorbeeldformulieren importeren

De twee formulieren waaruit dit voorbeeld bestaat, moeten in AEM worden geïmporteerd. De voorbeeldformulieren kunnen [hier worden gedownload](assets/sample-forms.zip)

Open [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) in bewerkingsmodus. Geef de waarden voor de API-sleutel en het API-geheim op in de desbetreffende velden in het adaptieve formulier.

## De oplossing testen

Voorvertoning van de [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Voer uw mobiele nummer in, inclusief de landcode, vul uw gebruikersgegevens in en voeg enkele bijlagen toe. Klik op de knop &quot;Opslaan en afsluiten&quot; om het aangepaste formulier en de bijbehorende bijlagen op te slaan


## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
