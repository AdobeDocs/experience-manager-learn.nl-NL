---
title: adaptief formulier maken
description: Aangepast formulier maken en configureren voor gebruik van de vooraf ingevulde service van het formuliergegevensmodel
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5813
thumbnail: kt-5813.jpg
topic: Development
role: User
level: Beginner
exl-id: c8d4eed8-9e2b-458c-90d8-832fc9e0ad3f
duration: 124
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 0%

---

# Adaptief formulier maken

Tot nu toe hebben we het volgende gecreëerd:

* Database met 2 tabellen - `newhire` en `beneficiaries`
* Gevormde Apache Sling Connection Pooled DataSource
* Op RDBMS gebaseerd formuliergegevensmodel

De volgende stap bestaat uit het maken en configureren van een adaptief formulier voor het gebruik van het formuliergegevensmodel.  Om hoofdbegin te krijgen kunt u [&#128279;](assets/fdm-demo-af.zip) steekproefvorm downloaden en invoeren . Het voorbeeldformulier bevat een sectie waarin de werknemersgegevens worden weergegeven en een ander gedeelte waarin de begunstigden van de werknemers worden vermeld.

## Formulier koppelen aan formuliergegevensmodel

Het voorbeeldformulier dat bij deze cursus wordt geleverd, is niet gekoppeld aan een formuliergegevensmodel. Als u het formulier wilt configureren voor het gebruik van het gegevensmodel van het formulier, moet u het volgende doen:

* Het FDMDemo-formulier selecteren
* Klik op _Eigenschappen_ -> _Model van de Vorm_
* Selecteer een formuliergegevensmodel in de vervolgkeuzelijst
* Zoek en selecteer het formuliergegevensmodel dat u in de vorige les hebt gemaakt.
* Klik op _sparen &amp; Sluiten_

## Prefill-service configureren

De eerste stap bestaat uit het koppelen van de vooraf ingevulde service voor het formulier. Volg de onderstaande stappen om vooraf ingevulde services te koppelen

* Selecteer het `FDMDemo` -formulier
* Klik _uitgeven_ om de vorm op uit te geven wijze te openen
* Selecteer Formuliercontainer in de inhoudshiërarchie en klik op het moersleutelpictogram om het eigenschappenblad te openen
* Selecteer _de ModelVooraf ingevulde dienst van de Gegevens van de Vorm_ van de Prefill drop-down lijst van de Dienst
* Klik op blauwe ☑ om uw wijzigingen op te slaan

* ![ prefill-service ](assets/fdm-prefill.png)

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
* Plaats de Bindverwijzing aan **/newhire/GetEmployeeBeneficiaries**
* Stel Herhalingsinstellingen - Minimum aantal in op 1 en Maximum aantal op 5.
* Uw configuratie Row1 zou als het scherm moeten kijken dat hieronder wordt ontsproten
  ![ rij-vormt ](assets/configure-row.PNG)
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
[ Vorm met empID=207 ](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[ Vorm met empID=208 ](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Problemen oplossen

Mijn formulier is leeg en bevat geen gegevens

* Zorg ervoor dat het formuliergegevensmodel de juiste resultaten retourneert.
* Het formulier is gekoppeld aan het juiste formuliergegevensmodel
* Veldbindingen controleren
* Stdout logbestand controleren. U ziet dat de empID naar het bestand wordt geschreven. Als deze waarde niet wordt weergegeven, gebruikt het formulier mogelijk niet de aangepaste sjabloon die is opgegeven.

Tabel is niet gevuld

* De binding Row1 controleren
* Controleer of de herhalingsinstellingen voor Rij1 correct zijn ingesteld (Min =1 en Max = 5 of meer)
