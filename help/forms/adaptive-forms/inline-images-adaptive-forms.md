---
title: Inline-afbeeldingen weergeven in Adaptive Forms
description: Geüploade afbeeldingen inline weergeven in Adaptive Forms
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 58
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# Inline-afbeeldingen in Adaptive Forms

Een veelvoorkomend geval is het weergeven van de geüploade afbeelding als een inline-afbeelding in Adaptieve vorm. Standaard wordt de geüploade afbeelding weergegeven als een koppeling en deze ervaring kan worden vergroot door de afbeelding weer te geven in Adaptief formulier. Dit artikel doorloopt de stappen voor de weergave van inline-afbeeldingen.

## Afbeelding voor plaatsaanduiding toevoegen

De eerste stap bestaat uit het vooraf toevoegen van een plaatsaanduiding voor een div-element aan de bestandsbijlage. In de code onder de component voor bestandsbijlagen wordt de CSS-klassennaam van het uploaden van foto&#39;s weergegeven. De JavaScript-functie is onderdeel van de clientbibliotheek die is gekoppeld aan de adaptieve formulieren. Deze functie wordt aangeroepen in de gebeurtenis initialize van de component voor bestandsbijlagen.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### Inline-afbeelding weergeven

Nadat de gebruiker de afbeelding heeft geüpload, wordt de hieronder vermelde functie aangeroepen voor het vastleggen van de gebeurtenis van de bestandsbijlage. De functie ontvangt het geüploade bestandsobject als een argument.

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### Distribueren op uw server

* Download en installeer de [&#x200B; cliëntbibliotheek &#x200B;](assets/inline-image-client-library.zip) op uw instantie van AEM gebruikend het pakketmanager van AEM.
* Download en installeer de [&#x200B; steekproefvorm &#x200B;](assets/inline-image-af.zip) op u uw instantie van AEM gebruikend het pakketmanager van AEM.
* Punt uw browser aan [&#x200B; voegt het Beeld Inline &#x200B;](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled) toe
* Klik op de knop &quot;Foto koppelen&quot; om een afbeelding toe te voegen
