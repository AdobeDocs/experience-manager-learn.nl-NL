---
title: Reactie extraheren uit aangepaste verzending
description: Aangepaste reactie extraheren bij het verzenden van een formulier
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---


# Het json-object uit het antwoord extraheren

Wanneer u een koploos adaptief formulier verzendt naar een aangepaste verzendhandler, kunnen de gegevens die door de verzendhandler worden geretourneerd, worden uitgepakt en weergegeven in uw reactie-app. Het volgende codefragment toont

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```











