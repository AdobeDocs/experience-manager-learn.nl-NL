---
title: Foutberichten vastleggen in de service Formuliergegevensmodel als stap in de workflow
description: Vanaf AEM Forms 6.5.1 kunnen nu foutberichten worden vastgelegd die worden gegenereerd wanneer de service Formuliergegevensmodel aanroepen wordt gebruikt als een stap in AEM workflow. Workflow.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
duration: 61
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# Vastleggen van foutberichten in Stap van de service Formuliergegevensmodel aanroepen

Vanaf AEM Forms 6.5.1 kunnen nu foutberichten worden vastgelegd en validatieopties worden opgegeven. De aanroepservice Formuliergegevensmodel is uitgebreid met de volgende mogelijkheden.

* Optie voor validatie op drie niveaus (&quot;OFF&quot;, &quot;BASIC&quot; en &quot;FULL&quot;) voor het verwerken van uitzonderingen die worden aangetroffen bij het aanroepen van de service Formuliergegevensmodel. De drie opties wijzen achtereenvolgens op een striktere versie van het controleren van gegevensbestand-specifieke vereisten.
  ![validatieniveaus](assets/validation-level.PNG)

* Het aanvinken van een selectievakje voor het aanpassen van de uitvoering van Workflow. Daarom heeft de gebruiker nu de flexibiliteit om door te gaan met de Workflowuitvoering, zelfs als de stap Formuliergegevensmodel aanroepen uitzonderingen genereert.

* Belangrijke informatie opslaan over fouten die zich voordoen als gevolg van validatie-uitzonderingen. Drie variabelen van het type AutoComplete zijn opgenomen om relevante variabelen te selecteren om ErrorCode (Koord), ErrorMessage (Koord) en ErrorDetails (JSON) op te slaan. ErrorDetails zou echter aan ongeldige toename worden geplaatst de uitzondering is geen DermisValidationException.
  ![vastleggen, foutberichten](assets/fdm-error-details.PNG)

Met deze wijzigingen zorgt de stap Service Formuliergegevensmodel aanroepen ervoor dat de invoerwaarden voldoen aan de gegevensbeperkingen die in het kwikbestand zijn opgegeven. Het volgende foutbericht wordt bijvoorbeeld gegenereerd wanneer de waarden voor accountId en balance niet voldoen aan de gegevensbeperkingen die in het wagerbestand zijn opgegeven.

```json
{
    "errorCode": "AEM-FDM-001-049"
    "errorMessage": "Input validations failed during operation execution"
    "violations": {
        "/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],
        "/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]
    }   
}
```
