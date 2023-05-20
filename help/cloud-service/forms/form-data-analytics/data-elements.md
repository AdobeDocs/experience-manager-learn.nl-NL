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
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# Passende gegevenselementen maken

In het bezit van Markeringen hebben wij twee nieuwe gegevenselementen (ApplicantsStateOfResidence en validationError) toegevoegd.

![adaptief](assets/data_elements.png)

## Verzoekende partijStateOfResidence

De **Verzoekende partijStateOfResidence** data element was gevormd door te selecteren **Kern** in de vervolgkeuzelijst met extensies en **Aangepaste code** voor het type gegevenselement, zoals wordt weergegeven in de onderstaande schermafbeelding
![ingezetene van de verzoekende staat](assets/applicantstateofresidence.png)

De volgende aangepaste code is gebruikt om de waarde van de **_state_** adaptief formulierveld.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

De **ValidationError** data element was gevormd door te selecteren **Kern** in de vervolgkeuzelijst met extensies en **Aangepaste code** voor het type gegevenselement, zoals wordt weergegeven in de onderstaande schermafbeelding

![validatiefout](assets/validation-error.png)

De volgende aangepaste code is geschreven om de waarde van het gegevenselement validationError in te stellen.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");
_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)
if(tel.isValid == false)
{  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if(email.isValid == false)
{  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```
