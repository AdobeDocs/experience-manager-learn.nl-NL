---
title: Reageer app met AEM Forms en Acrobat Sign
description: Met Acrobat Sign en AEM Forms kunt u complexe transacties automatiseren en juridische e-handtekeningen opnemen als onderdeel van een naadloze digitale ervaring.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '159'
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
