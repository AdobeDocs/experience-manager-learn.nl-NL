---
title: DAM-afbeeldingen inline weergeven in Adaptive Forms
description: DAM-afbeeldingen inline weergeven in Adaptive Forms
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
duration: 60
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# DAM-afbeelding weergeven in Adaptief Forms

Een veelvoorkomend geval is het inline weergeven van de afbeelding(en) in de crx-opslagplaats in een adaptief formulier.

## Afbeelding voor plaatsaanduiding toevoegen

De eerste stap bestaat uit het vooraf toevoegen van een plaatsaanduiding voor div aan de deelvenstercomponent. In de code onder de deelvenstercomponent wordt de CSS-klassenaam van het uploaden van foto&#39;s aangeduid. De JavaScript-functie is onderdeel van de clientbibliotheek die is gekoppeld aan de adaptieve formulieren. Deze functie wordt aangeroepen in de gebeurtenis initialize van de component voor bestandsbijlagen.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Inline-afbeelding weergeven

Nadat de gebruiker de afbeelding heeft geselecteerd, wordt het verborgen veld ImageName gevuld met de geselecteerde afbeeldingsnaam. Deze afbeeldingsnaam wordt vervolgens doorgegeven aan de URLToFile-functie dam die de functie createFile aanroept om een URL om te zetten in een blob voor FileReader.readAsDataURL().

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### Distribueren op uw server

* Download en installeer de [ cliëntbibliotheek en steekproefbeelden ](assets/InlineDAMImage.zip) op uw instantie van AEM gebruikend de Manager van het Pakket van AEM.
* Download en installeer de [ steekproefvorm ](assets/FieldInspectionForm.zip) op u uw instantie van AEM gebruikend het pakketmanager van AEM.
* Punt uw browser aan [ FileInspectionForm ](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Selecteer een van de correcties
* De afbeelding moet in het formulier worden weergegeven
