---
title: Functies en code-editor gebruiken
description: Functies en code-editor gebruiken om de bedrijfsregel van de auteur te schrijven
feature: Adaptieve Forms
version: 6.4,6.5
kt: 4270
thumbnail: 22282.jpg
topic: Ontwikkeling
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---


# Aangepaste functies en code-editor gebruiken {#using-functions-and-code-editor}

In dit deel, zullen wij douanefuncties en de coderedacteur aan auteur bedrijfsregels gebruiken.

u hebt [ClientLib met douanefunctie](assets/client-libs-and-logo.zip) eerder in deze zelfstudie geïnstalleerd.

Een clientbibliotheek bestaat meestal uit CSS- en JavaScript-bestanden. Deze clientbibliotheek bevat een javascript-bestand waarin een functie voor het invullen van vervolgkeuzelijstwaarden beschikbaar wordt gemaakt.


## Functie om vervolgkeuzelijst te vullen {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### Samenvattingstitel van deelvenster instellen {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### Deelvenster Valideren {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

Hier volgt de code waarmee deelvenstervelden worden gevalideerd.

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
