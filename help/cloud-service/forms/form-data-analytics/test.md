---
title: Ingevulde formuliergegevensvelden rapporteren met Adobe Analytics
description: AEM Forms CS met Adobe Analytics integreren om formuliergegevensvelden te rapporteren
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Uw oplossing testen

Bekijk een voorbeeld van een formulier en verzend het formulier met behulp van verschillende combinaties van formulierwaarden. Sta meerdere tot 30 minuten toe om uw gegevens te bekijken in Adobe Analytics-rapporten. Gegevensset die naar PROPS wordt gestuurd, wordt eerder weergegeven dan gegevensset die naar Vars wordt ingesteld.

## Rapportsuite

De formuliergegevens die in Adobe Analytics zijn vastgelegd, worden in donut-indeling weergegeven

**Indiening per staat**

![applicantsbystate](assets/donut.png)

Veldvalidatiefouten

![field-validation-error](assets/donut-field-validation.png)

## Foutopsporing

Zorg ervoor dat het Adaptieve formulier dezelfde configuratiecontainer gebruikt die de configuratie voor het starten van de Adobe bevat.

Ga als volgt te werk om te bevestigen dat het formulier gegevens naar Adobe Analytics verzendt

* Open de Developer Tools in uw browser.
* Typ de volgende tekst in het deelvenster Console.

```javascript
_satellite.setDebug(true)
```

Communiceer met uw formulier terwijl u het consolevenster open houdt. Je moet zoiets zien

![console-debug](assets/debug.png)

## Adobe Experience Platform Debugger gebruiken

Voeg de [AEP-extensie voor foutopsporing](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) aan uw browser (u moet zich aanmelden) om meer het zuiveren informatie te krijgen

![platform-debugger](assets/platform-debugger.png)





