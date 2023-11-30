---
title: Het voorbeeld implementeren
description: Gebruiksscenario's uitvoeren op uw lokale AEM Forms-instantie
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 0%

---

# Het voorbeeld implementeren

Volg de onderstaande instructies om deze gebruiksaanwijzing op uw systeem te laten werken:

>[!NOTE]
>Aangenomen wordt dat u AEM Forms uitvoert op poort 4502.


## Database maken

In dit voorbeeld wordt de MySQL-database gebruikt om de adaptieve formuliergegevens op te slaan. U moet de [databaseschema door het schemabestand te importeren](assets/data-base-schema.sql) in MySQL Workbench.

## Gegevensbron maken

U moet een Apache Sling Connection Pooled DataSource maken, genaamd **StoreAndRetrieveAfData** verwijzen naar het databaseschema dat in de vorige stap is gemaakt. De code in de bundel OSGi gebruikt deze datasource naam.

## Formuliergegevensmodel maken

Formuliergegevensmodel moet worden gemaakt op basis van deze gegevensbron die wordt genoemd **StoreAndRetrieveAfData**. Dit formuliergegevensmodel wordt gebruikt om het mobiele-telefoonnummer op te halen dat aan de toepassings-id is gekoppeld. Het formuliergegevensmodel kan [hier gedownload.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Ontwikkelaarsaccount maken met nexmo

Een ontwikkelaarsaccount maken met [Nexmo](https://dashboard.nexmo.com/) voor het verzenden en verifiëren van OTP-codes. Noteer de API-sleutel en de API-beveiligingssleutel. Het gegevensbron- en formuliergegevensmodel zijn al voor u gemaakt op basis van deze service en worden opgenomen met de elementen die in de vorige stap zijn vermeld.

## De volgende OSGi-bundels implementeren

De bundel implementeren die de [code om gegevens uit de database op te slaan en op te halen](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
Download en decomprimeer de [developwithService user.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Implementeer het bestand DevelopingWithServiceUser.jar met behulp van de Felix-webconsole.

## De clientbibliotheek implementeren

In het voorbeeld worden twee clientbibliotheken gebruikt. Deze importeren [clientbibliotheken](assets/store-af-with-attachments-client-lib.zip) in AEM.

## De aangepaste adaptieve formuliersjabloon importeren

De voorbeeldformulieren die in deze demo worden gebruikt, zijn gebaseerd op een aangepaste sjabloon. Het dialoogvenster Importeren [aangepaste sjabloon in AEM](assets/custom-template-with-page-component.zip)

## De adaptieve voorbeeldformulieren importeren

De twee formulieren waaruit dit voorbeeld bestaat, moeten in AEM worden geïmporteerd. De voorbeeldformulieren kunnen [hier gedownload](assets/sample-forms.zip)

Open de [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) in bewerkingsmodus. Geef de waarden voor Vonage API Key en API Secret op in de juiste velden in het adaptieve formulier.

## De oplossing testen

Voorvertoning van de [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Voer uw mobiele nummer in, inclusief de landcode, vul uw gebruikersgegevens in en voeg enkele bijlagen toe. Klik op de knop &quot;Opslaan en afsluiten&quot; om het aangepaste formulier en de bijbehorende bijlagen op te slaan


## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
