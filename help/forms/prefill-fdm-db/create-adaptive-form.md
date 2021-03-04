---
title: adaptief formulier maken
description: Aangepast formulier maken en configureren voor gebruik van de vooraf ingevulde service van het formuliergegevensmodel
feature: adaptieve vormen
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---


# Adaptief formulier maken

Tot nu toe hebben we het volgende gecreëerd:

* Database met 2 tabellen - `newhire` en `beneficiaries`
* Gevormde Apache Sling Connection Pooled DataSource
* Op RDBMS gebaseerd formuliergegevensmodel

De volgende stap bestaat uit het maken en configureren van een adaptief formulier voor het gebruik van het formuliergegevensmodel.  Als u het begin van de kop wilt ophalen, kunt u [voorbeeldformulier downloaden en importeren](assets/fdm-demo-af.zip). Het voorbeeldformulier bevat een sectie waarin de werknemersgegevens worden weergegeven en een ander gedeelte waarin de begunstigden van de werknemers worden vermeld.

## Formulier koppelen aan formuliergegevensmodel

Het voorbeeldformulier dat bij deze cursus wordt geleverd, is niet gekoppeld aan een formuliergegevensmodel. Als u het formulier wilt configureren voor het gebruik van het gegevensmodel van het formulier, moet u het volgende doen:

* Het FDMDemo-formulier selecteren
* Klik op _Eigenschappen_->_Formuliermodel_
* Selecteer Formuliergegevensmodel in de vervolgkeuzelijst
* Zoek en selecteer het formuliergegevensmodel dat u in de vorige les hebt gemaakt.
* Klik op _Opslaan en sluiten_

## Prefill-service configureren

De eerste stap bestaat uit het koppelen van de vooraf ingevulde service voor het formulier. Volg de onderstaande stappen om vooraf ingevulde services te koppelen

* Selecteer het formulier `FDMDemo`
* Klik op _Bewerken_ om het formulier te openen in de bewerkingsmodus
* Selecteer Formuliercontainer in de inhoudshiërarchie en klik op het moersleutelpictogram om het eigenschappenblad te openen
* Selecteer _Vooraf ingevulde service Formuliergegevensmodel_ in de vervolgkeuzelijst Prefill-service
* Klik op blauwe ☑ om uw wijzigingen op te slaan

* ![Prefill-service](assets/fdm-prefill.png)

## Werknemersgegevens configureren

De volgende stap is de tekstvelden van het Adaptief formulier binden aan de elementen van het formuliergegevensmodel. U moet het eigenschappenblad van de volgende velden openen en de bindRef instellen zoals hieronder wordt weergegeven


| Veldnaam | Bind Ref |
|------------|--------------------|
| FirstName | /newhire/FirstName |
| LastName | /newhire/lastName |

>[!NOTE]
>
>Voel u vrij extra tekstvelden toe te voegen en deze te binden aan de juiste formuliergegevensmodelelementen

## Lijst met begunstigden configureren

De volgende stap is de begunstigden van de werknemer in tabelvorm te tonen. Het voorbeeldformulier bevat een tabel met vier kolommen en één rij. We moeten de tabel zo configureren dat deze groter wordt afhankelijk van het aantal begunstigden.

* Open het formulier in de bewerkingsmodus.
* Hoofdvenster uitvouwen->Uw begunstigden->Tabel
* Selecteer Rij1 en klik op het moersleutelpictogram om het eigenschappenblad te openen.
* Stel de bindingsverwijzing in op **/newhire/GetEmployeeBeneficiaries**
* Stel Herhalingsinstellingen - Minimum aantal in op 1 en Maximum aantal op 5.
* Uw configuratie Row1 zou als het scherm moeten kijken dat hieronder wordt ontsproten
   ![row-configure](assets/configure-row.PNG)
* Klik op de blauwe ☑ om uw wijzigingen op te slaan

## Rijcellen binden

Tot slot moeten wij de cellen van de Rij aan de Modelelementen van de Gegevens van de Vorm binden.

* Hoofdvenster uitvouwen->Uw begunstigden->Tabel->Rij1
* De bindingsverwijzing voor elke rijcel instellen volgens de onderstaande tabel

| Rijcel | Bindverwijzing |
|------------|----------------------------------------------|
| Voornaam | /newhire/GetEmployeeBeneficiaries/firstname |
| Achternaam | /newhire/GetEmployeeBeneficiaries/lastname |
| Relatie | /newhire/GetEmployeeBeneficiaries/relation |
| Percentage | /newhire/GetEmployeeBeneficiaries/percentage |

* Klik op de blauwe ☑ om uw wijzigingen op te slaan

## Uw formulier testen

We moeten het formulier nu openen met de juiste empID in de URL. De volgende twee koppelingen vullen formulieren met informatie uit de database
[Formulier met empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formulier met empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Problemen oplossen

Mijn formulier is leeg en bevat geen gegevens

* Zorg ervoor dat het formuliergegevensmodel de juiste resultaten retourneert.
* Het formulier is gekoppeld aan het juiste formuliergegevensmodel
* Veldbindingen controleren
* Stdout logbestand controleren. U ziet dat de empID naar het bestand wordt geschreven. Als deze waarde niet wordt weergegeven, gebruikt het formulier mogelijk niet de aangepaste sjabloon die is opgegeven.

Tabel is niet gevuld

* De binding Row1 controleren
* Controleer of de herhalingsinstellingen voor Rij1 correct zijn ingesteld (Min =1 en Max = 5 of meer)

