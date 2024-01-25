---
title: Ingevulde formuliergegevensvelden rapporteren met Adobe Analytics
description: AEM Forms CS met Adobe Analytics integreren om formuliergegevensvelden te rapporteren
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 44
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Uw oplossing testen

Bekijk een voorbeeld van een formulier en verzend het formulier met behulp van verschillende combinaties van formulierwaarden. Sta meerdere tot 30 minuten toe om uw gegevens te bekijken in Adobe Analytics-rapporten. Gegevensset die naar props wordt gestuurd, wordt eerder weergegeven dan gegevensset die naar eVars wordt ingesteld.

## Rapportsuite

De formuliergegevens die in Adobe Analytics zijn vastgelegd, worden in donut-indeling weergegeven

**Indiening per staat**

![applicantsbystate](assets/donut.png)

Veldvalidatiefouten

![field-validation-error](assets/donut-field-validation.png)

## Foutopsporing

Zorg ervoor dat het Adaptieve formulier dezelfde configuratiecontainer gebruikt die de Adobe Launch Configuration bevat.

Ga als volgt te werk om te bevestigen dat het formulier gegevens naar Adobe Analytics verzendt

* Open de Developer Tools in uw browser.
* Typ de volgende tekst in het deelvenster Console.

```javascript
_satellite.setDebug(true)
```

Communiceer met uw formulier terwijl u het consolevenster open houdt. Je moet zoiets zien

![console-debug](assets/debug.png)

## Adobe Experience Platform Debugger gebruiken

Voeg de [AEP-extensie foutopsporing](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) aan uw browser (u moet zich aanmelden) om meer het zuiveren informatie te krijgen

![platform-debugger](assets/platform-debugger.png)

## Gefeliciteerd

U hebt AEM Forms as a Cloud Service met Adobe Analytics geïntegreerd om formuliergegevensvelden te rapporteren.