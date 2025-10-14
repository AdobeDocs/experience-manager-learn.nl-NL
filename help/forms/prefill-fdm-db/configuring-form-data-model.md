---
title: Formuliergegevensmodel configureren
description: Formuliergegevensmodel maken op basis van RDBMS-gegevensbron
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
duration: 103
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 0%

---

# Formuliergegevensmodel configureren

## Apache Sling Connection Pooled DataSource

De eerste stap bij het maken van een formulier met RDBMS-ondersteuning is het configureren van Apache Sling Connection Pooled DataSource. Volg onderstaande stappen om de gegevensbron te configureren:

* Wijs uw browser aan [&#x200B; configMgr &#x200B;](http://localhost:4502/system/console/configMgr) aan
* Onderzoek naar **Apache die Verbinding Pooled DataSource** schrapt
* Voeg een nieuwe ingang toe en verstrek de waarden zoals aangetoond in het schermafbeelding.
* ![&#x200B; gegeven-bron &#x200B;](assets/data-source.png)
* Uw wijzigingen opslaan

>[!NOTE]
>De URI van de verbinding JDBC, de gebruikersnaam en het Wachtwoord zullen afhankelijk van uw MySQL gegevensbestandconfiguratie veranderen.


## Formuliergegevensmodel maken

* Punt uw browser aan [&#x200B; Integraties van Gegevens &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* Klik _creëren_ -> _Model van de Gegevens van de Vorm_
* Verstrek zinvolle naam en titel aan het model van vormgegevens zoals **Werknemer**
* Klik _daarna_
* Selecteer de gegevensbron die in de vorige sectie (forums) is gemaakt
* Klik _creëren_ ->geef uit om het pas gecreëerde model van vormgegevens op uit te geven wijze te openen
* Breid de _forums_ knoop uit om het werknemersschema te zien. Breid de werknemersknoop uit om de 2 lijsten te zien

## Entiteiten aan uw model toevoegen

* Zorg ervoor dat het werknemersknooppunt wordt uitgebreid
* Selecteer nieuwe entiteiten en begunstigden en klik op _toevoegen Geselecteerde_

## Lees service toevoegen aan nieuwe entiteit

* Vernieuwde entiteit selecteren
* Klik op _uitgeven Eigenschappen_
* Selecteer get in de vervolgkeuzelijst Leesservice
* Klik + pictogram om parameter aan de get dienst toe te voegen
* Geef de waarden op die worden weergegeven in de schermafbeelding
* ![&#x200B; get-service &#x200B;](assets/get-service.png)
>[!NOTE]
> De get-service verwacht een waarde die is toegewezen aan de empID-kolom van een nieuwe entiteit. Er zijn meerdere manieren om deze waarde door te geven en in deze zelfstudie wordt de empID doorgegeven via de aanvraagparameter empID.
>* Klik _Gedaan_ om de argumenten voor de get dienst te bewaren
>* Klik _Gereed_ om veranderingen in het model van vormgegevens te bewaren

## Koppeling toevoegen tussen 2 entiteiten

De koppelingen die zijn gedefinieerd tussen database-entiteiten, worden niet automatisch gemaakt in het formuliergegevensmodel. De koppelingen tussen entiteiten moeten worden gedefinieerd met behulp van de formuliergegevensmodeleditor. Elke nieuwe entiteit kan een of meer begunstigden hebben, we moeten een-op-een-associatie definiëren tussen de entiteiten die het liefst van de Unie en de begunstigden zijn.
De volgende stappen zullen u door het proces lopen om de één-aan-vele vereniging tot stand te brengen

* Selecteer nieuwe entiteit en klik op _toevoegen Vereniging_
* Geef de koppeling en andere eigenschappen een betekenisvolle titel en id, zoals in de onderstaande schermafbeelding wordt getoond
  ![&#x200B; vereniging &#x200B;](assets/association-entities-1.png)

* Klik op _uitgeven_ pictogram onder de sectie van Argumenten

* Waarden opgeven zoals deze schermafbeelding wordt weergegeven
* ![&#x200B; vereniging-2 &#x200B;](assets/association-entities.png)
* **wij verbinden de twee entiteiten gebruikend de empID kolom van begunstigden en naburige entiteiten.**
* Klik op _Gereed_ om uw veranderingen te bewaren

## Uw formuliergegevensmodel testen

Ons model van vormgegevens heeft nu **_krijgen_** dienst die empID goedkeurt en de details van het netwerk en zijn begunstigden terugkeert. Volg onderstaande stappen om de service voor ophalen te testen.

* Vernieuwde entiteit selecteren
* Klik op _Modelvoorwerp van de Test_
* Verstrek geldige empID en klik op _Test_
* U zou resultaten moeten krijgen zoals aangetoond in het hieronder ontsproten scherm
* ![&#x200B; test-fdm &#x200B;](assets/test-form-data-model.png)

## Volgende stappen

[empID ophalen van de URL](./get-request-parameter.md)