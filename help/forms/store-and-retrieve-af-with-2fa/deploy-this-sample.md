---
title: Het voorbeeld implementeren
description: Gebruiksscenario's uitvoeren op uw lokale AEM Forms-instantie
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 186
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 0%

---

# Het voorbeeld implementeren

Volg de onderstaande instructies om deze gebruiksaanwijzing op uw systeem te laten werken:

>[!NOTE]
>Aangenomen wordt dat u AEM Forms uitvoert op poort 4502.


## Database maken

In dit voorbeeld wordt de MySQL-database gebruikt om de adaptieve formuliergegevens op te slaan. U zult het [ gegevensbestandschema moeten tot stand brengen door het schemadossier ](assets/data-base-schema.sql) in MySQL werkbank in te voeren.

## Gegevensbron maken

U moet een Apache het Schuiven Verbinding Pooled DataSource tot stand brengen riep **StoreAndRetrieveAfData** richtend aan het gegevensbestandschema dat in de vroegere stap wordt gecreeerd. De code in de bundel OSGi gebruikt deze datasource naam.

## Formuliergegevensmodel maken

Het Model van de Gegevens van de vorm moet worden gecreeerd gebaseerd op deze gegevensbron genoemd **StoreAndRetrieveAfData**. Dit formuliergegevensmodel wordt gebruikt om het mobiele-telefoonnummer op te halen dat aan de toepassings-id is gekoppeld. Het model van vormgegevens kan [ van hier worden gedownload.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Ontwikkelaarsaccount maken met nexmo

Creeer een ontwikkelaarrekening met [ Nexmo ](https://dashboard.nexmo.com/) voor het verzenden van en het verifiëren van codes OTP. Noteer de API-sleutel en de API-beveiligingssleutel. Het gegevensbron- en formuliergegevensmodel zijn al voor u gemaakt op basis van deze service en worden opgenomen met de elementen die in de vorige stap zijn vermeld.

## De volgende OSGi-bundels implementeren

Stel de bundel op die de [ code heeft om gegevens van gegevensbestand op te slaan en te halen ](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
Download en decomprimeer [ het ontwikkelen withServiceUser.zip ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=nl-NL).
Implementeer het bestand DevelopingWithServiceUser.jar met behulp van de Felix-webconsole.

## De clientbibliotheek implementeren

In het voorbeeld worden twee clientbibliotheken gebruikt. Importeer deze [ cliëntbibliotheken ](assets/store-af-with-attachments-client-lib.zip) in AEM.

## De aangepaste adaptieve formuliersjabloon importeren

De voorbeeldformulieren die in deze demo worden gebruikt, zijn gebaseerd op een aangepaste sjabloon. Invoer het [ douanemalplaatje in AEM ](assets/custom-template-with-page-component.zip)

## De adaptieve voorbeeldformulieren importeren

De twee formulieren waaruit dit voorbeeld bestaat, moeten in AEM worden geïmporteerd. De steekproefvormen kunnen [ van hier worden gedownload ](assets/sample-forms.zip)

Open [ MyAccountForm ](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) op geef wijze uit. Geef de waarden voor Vonage API Key en API Secret op in de juiste velden in het adaptieve formulier.

## De oplossing testen

Voorproef [ StoreAFWithAttachments ](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Voer uw mobiele nummer in, inclusief de landcode, vul uw gebruikersgegevens in en voeg enkele bijlagen toe. Klik op de knop &quot;Opslaan en afsluiten&quot; om het aangepaste formulier en de bijbehorende bijlagen op te slaan


## Bewijs van de gebruikszaak

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
