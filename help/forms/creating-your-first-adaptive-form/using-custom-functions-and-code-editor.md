---
title: Functies en code-editor gebruiken
description: Functies en code-editor gebruiken om de bedrijfsregel van de auteur te schrijven
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
duration: 460
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Aangepaste functies en code-editor gebruiken {#using-functions-and-code-editor}

In dit deel, zullen wij douanefuncties en de coderedacteur aan auteur bedrijfsregels gebruiken.

u hebt reeds [ ClientLib met douanefunctie ](assets/client-libs-and-logo.zip) vroeger in dit leerprogramma geïnstalleerd.

Een clientbibliotheek bestaat meestal uit CSS- en JavaScript-bestanden. Deze clientbibliotheek bevat een javascript-bestand waarin een functie voor het invullen van vervolgkeuzelijstwaarden beschikbaar wordt gemaakt.


## Functie om vervolgkeuzelijst te vullen {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### Samenvattingstitel van deelvenster instellen {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### Deelvenster Valideren {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=12&learn=on)

Hier volgt de code waarmee deelvenstervelden worden gevalideerd:

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

U kunt regel 1 verwijderen om fouten op te sporen in de code in het browservenster.

Regel 4 - Krijg het huidige paneel

Regel 5 - Valideer het huidige paneel.

Regel 9 - Als er geen fouten worden weergegeven, gaat u naar het volgende venster

Geef een voorbeeld van het formulier weer en test de nieuw ingeschakelde functionaliteit.
