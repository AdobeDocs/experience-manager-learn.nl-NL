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
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 42
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Gegevenselementen maken

In het bezit van Markeringen hebben wij twee nieuwe gegevenselementen (ApplicantsStateOfResidence en validationError) toegevoegd.

![ adaptive-form ](assets/data_elements.png)

## Verzoekende partijStateOfResidence

Het **ApplicationStateOfResidence** gegevenselement werd gevormd door **Kern** in de uitbreidingsdrop-down en **Douane Code** voor het Type van Element van Gegevens zoals aangetoond in het hieronder ontsproten scherm te selecteren
![ aanvrager-staat-verblijf ](assets/applicantstateofresidence.png)

De volgende douanecode werd gebruikt om de waarde van het **_staat_** adaptieve vormgebied te vangen.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

Het **ValidationError** gegevenselement werd gevormd door **Kern** in de uitbreidingsdrop-down en **Douane Code** voor het Type van Element van Gegevens zoals aangetoond in het scherm hieronder ontsproten

![ bevestiging-fout ](assets/validation-error.png)

De volgende aangepaste code is geschreven om de waarde van het gegevenselement `validationError` in te stellen.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## Volgende stappen

[Regels maken](./rules.md)