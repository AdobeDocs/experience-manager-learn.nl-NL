---
title: Variabelen in AEM workflow[Deel4]
seo-title: Variabelen in AEM workflow[Deel4]
description: Variabelen van het type xml,json,arraylist,document in een algemene workflow gebruiken
seo-description: Variabelen van het type xml,json,arraylist,document in een algemene workflow gebruiken
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Ontwikkeling
role: Developer
level: Begin
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# ArrayList-variabele in AEM workflow

Variabelen van het type ArrayList zijn geïntroduceerd in AEM Forms 6.5. Een gemeenschappelijk gebruiksgeval voor het gebruiken van variabele ArrayList moet douaneroutes bepalen die in AssignTask moeten worden gebruikt.

Als u de variabele ArrayList in een AEM workflow wilt gebruiken, moet u een adaptief formulier maken dat herhalende elementen in de verzonden gegevens genereert. Een veel voorkomende praktijk is het definiëren van een schema dat een arrayelement bevat. In het kader van dit artikel heb ik een eenvoudig JSON-schema met arrayelementen gemaakt. Het gebruiksgeval is van een werknemer die een uitgavenrapport invult. In het onkostenrapport leggen we de naam van de manager en de manager van de verzender vast. De namen van de manager worden opgeslagen in een serie genoemd managerchain. In de onderstaande screenshot ziet u het formulier met het onkostenrapport en de gegevens van de Adaptive Forms-verzending.

![expensereport](assets/expensereport.jpg)

Hieronder volgt een overzicht van de gegevens uit het aangepaste formulier dat wordt ingediend. Het adaptieve formulier is gebaseerd op het JSON-schema en de gegevens die aan het schema zijn gebonden, worden opgeslagen onder het gegevenselement van het element afBoundData. De managerchain is een array en we moeten de ArrayList vullen met het naamelement van het object binnen de managerchain-array.

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

Als u de ArrayList-variabele van een subtype-tekenreeks wilt initialiseren, kunt u de JSON-puntnotatie of de XPath-toewijzingsmodus gebruiken. In de volgende schermafbeelding ziet u hoe u een variabele ArrayList met de naam CustomRoutes vult met behulp van de JSON-puntnotatie. Zorg ervoor dat u naar een element in een matrixobject wijst, zoals in de onderstaande schermafbeelding wordt getoond. Wij bevolken CustomRoutes ArrayList met de namen van het managerchain serievoorwerp.
CustomRoutes ArrayList wordt dan gebruikt om de Routes in de component AssignTask te bevolken
![customroutes](assets/arraylist.jpg)
Zodra de variabele CustomRoutes ArrayList met de waarden van de voorgelegde gegevens wordt geïnitialiseerd, worden de Routes van de component AssignTask dan bevolkt gebruikend de variabele CustomRoutes. Het schermafbeelding hieronder toont u de aangepaste routes in een AssignTask
![asingtask](assets/customactions.jpg)

Voer de volgende stappen uit om deze workflow op uw systeem te testen

* Download en sla het bestand ArrayListVariable.zip op in uw bestandssysteem
* [Het ZIP-](assets/arraylistvariable.zip) bestand importeren met AEM Package Manager
* [Open het formulier TravelExpenseReport](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Voer een paar kosten en de namen van de twee managers in
* De verzendknop
* [De inbox openen](http://localhost:4502/aem/inbox)
* U zou een nieuwe taak moeten zien getiteld &quot;toewijzen aan uitgavenbeheerder&quot;
* Het formulier openen dat is gekoppeld aan de taak
* U zou twee douanerouten met de managernamen moeten zien
   [Ontdek de ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Deze workflow gebruikt de variabele ArrayList, variabele van het JSON-type, regeleditor in of-gesplitste component
