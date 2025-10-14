---
title: Reageer app met AEM Forms en Acrobat Sign
description: Met Acrobat Sign en AEM Forms kunt u complexe transacties automatiseren en juridische e-handtekeningen opnemen als onderdeel van een naadloze digitale ervaring.
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 31
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# AEM Forms met Acrobat Sign-webformulier


Dit leerprogramma begeleidt u door het gebruiksgeval van het produceren van een interactief communicatie document met de gegevens die van [&#x200B; worden voorgelegd Reageer &#x200B;](https://react.dev/) app en het voorstellen van het geproduceerde document voor het ondertekenen gebruikend Acrobat Sign webform.

Hieronder ziet u de stroom van het gebruiksgeval

* De gebruiker vult een formulier in React app in.
* De formuliergegevens worden naar een AEM Forms-eindpunt verzonden om een interactief communicatiedocument te genereren.
* Maak een Acrobat Sign-widget-URL met het gegenereerde document.
* Presenteer de widget-URL naar de opvragende toepassing zodat de gebruiker het document kan ondertekenen.

## Vereisten

U hebt het volgende nodig om het gebruik van hoofdletters en kleine letters te laten werken:

* Een AEM-server met Forms-add-on
* Een [&#x200B; integratiesleutel voor een toepassing van Acrobat Sign &#x200B;](https://helpx.adobe.com/nl/sign/kb/how-to-create-an-integration-key.html)

## Volgende stappen

Schrijf de dienst van A [&#x200B; douane OSGi om Interactief Communicatie Document &#x200B;](./create-ic-document.md) te produceren gebruikend gedocumenteerde API
