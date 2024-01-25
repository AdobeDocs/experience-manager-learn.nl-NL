---
title: Reageer app met AEM Forms en Acrobat Sign
description: Met Acrobat Sign en AEM Forms kunt u complexe transacties automatiseren en juridische e-handtekeningen opnemen als onderdeel van een naadloze digitale ervaring.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 34
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# AEM Forms met Acrobat Sign-webformulier


In deze zelfstudie wordt uitgelegd hoe u een interactief communicatiedocument kunt genereren met de gegevens die door het [Reageren](https://react.dev/) en het gegenereerde document presenteren voor ondertekening met Acrobat Sign-webformulier.

Hieronder ziet u de stroom van het gebruiksgeval

* De gebruiker vult een formulier in React app in.
* De formuliergegevens worden naar een AEM Forms-eindpunt verzonden om een interactief communicatiedocument te genereren.
* Maak een Acrobat Sign-widget-URL met het gegenereerde document.
* Presenteer de widget-URL naar de opvragende toepassing zodat de gebruiker het document kan ondertekenen.

## Vereisten

U hebt het volgende nodig om het gebruik van hoofdletters en kleine letters te laten werken:

* Een AEM server met Forms-add-on in pakket
* An [integratiesleutel voor een Acrobat Sign-toepassing](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## Volgende stappen

Een schrijven [De aangepaste dienst OSGi om Interactive Communication Document te produceren](./create-ic-document.md) gedocumenteerde API gebruiken
