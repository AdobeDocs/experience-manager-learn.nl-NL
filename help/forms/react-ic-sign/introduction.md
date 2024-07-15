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
duration: 31
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# AEM Forms met Acrobat Sign-webformulier


Dit leerprogramma begeleidt u door het gebruiksgeval van het produceren van een interactief communicatie document met de gegevens die van [ worden voorgelegd Reageer ](https://react.dev/) app en het voorstellen van het geproduceerde document voor het ondertekenen gebruikend Acrobat Sign webform.

Hieronder ziet u de stroom van het gebruiksgeval

* De gebruiker vult een formulier in React app in.
* De formuliergegevens worden naar een AEM Forms-eindpunt verzonden om een interactief communicatiedocument te genereren.
* Maak een Acrobat Sign-widget-URL met het gegenereerde document.
* Presenteer de widget-URL naar de opvragende toepassing zodat de gebruiker het document kan ondertekenen.

## Vereisten

U hebt het volgende nodig om het gebruik van hoofdletters en kleine letters te laten werken:

* Een AEM server met Forms-add-on in pakket
* Een [ integratiesleutel voor een toepassing van Acrobat Sign ](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## Volgende stappen

Schrijf de dienst van A [ douane OSGi om Interactief Communicatie Document ](./create-ic-document.md) te produceren gebruikend gedocumenteerde API
