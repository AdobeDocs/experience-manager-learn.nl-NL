---
title: Functies en code-editor gebruiken
seo-title: Functies en code-editor gebruiken
description: Functies en code-editor gebruiken om de bedrijfsregel van de auteur te schrijven
seo-description: Functies en code-editor gebruiken om de bedrijfsregel van de auteur te schrijven
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: Adaptive Forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---


# Aangepaste functies en code-editor {#using-functions-and-code-editor} gebruiken

In dit deel, zullen wij douanefuncties en de coderedacteur aan auteur bedrijfsregels gebruiken.

u hebt [ClientLib met douanefunctie](assets/client-libs-and-logo.zip) eerder in deze zelfstudie geïnstalleerd.

Een clientbibliotheek bestaat meestal uit CSS- en JavaScript-bestanden. Deze clientbibliotheek bevat een javascript-bestand waarin een functie voor het invullen van vervolgkeuzelijstwaarden beschikbaar wordt gemaakt.


## Functie om vervolgkeuzelijst {#function-to-populate-drop-down-list} te vullen

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### Samenvattingstitel van deelvenster {#set-the-summary-title-of-panels} instellen

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### Deelvenster {#validate-panels-using-rule-editor} valideren

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
