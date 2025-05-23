---
title: Ingevulde formuliergegevensvelden rapporteren met Adobe Analytics
description: AEM Forms CS met Adobe Analytics integreren om formuliergegevensvelden te rapporteren
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Uw oplossing testen

Bekijk een voorbeeld van een formulier en verzend het formulier met behulp van verschillende combinaties van formulierwaarden. Sta meerdere tot 30 minuten toe om uw gegevens te bekijken in Adobe Analytics-rapporten. Gegevensset die naar props wordt gestuurd, wordt eerder weergegeven dan gegevensset die naar eVars wordt ingesteld.

## Rapportsuite

De formuliergegevens die in Adobe Analytics zijn vastgelegd, worden in donut-indeling weergegeven

**Voorleggen door Staat**

![ applicantsbystate ](assets/donut.png)

Veldvalidatiefouten

![ gebied-bevestiging-fout ](assets/donut-field-validation.png)

## Foutopsporing

Zorg ervoor dat het Adaptief formulier dezelfde configuratiecontainer gebruikt die de Adobe Launch Configuration bevat.

Ga als volgt te werk om te bevestigen dat het formulier gegevens naar Adobe Analytics verzendt

* Open de Developer Tools in uw browser.
* Typ de volgende tekst in het deelvenster Console.

```javascript
_satellite.setDebug(true)
```

Communiceer met uw formulier terwijl u het consolevenster open houdt. Je moet zoiets zien

![ console-debug ](assets/debug.png)

## Adobe Experience Platform Debugger gebruiken

Voeg de [ debugger van AEP uitbreiding ](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html?lang=nl-NL) aan uw browser (u wordt vereist om binnen te ondertekenen) toe om meer het zuiveren informatie te krijgen

![ platform-debugger ](assets/platform-debugger.png)

## Gefeliciteerd

U hebt AEM Forms as a Cloud Service met Adobe Analytics geïntegreerd om formuliergegevensvelden te rapporteren.