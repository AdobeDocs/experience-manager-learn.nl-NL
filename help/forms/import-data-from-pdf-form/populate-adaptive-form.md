---
title: Aangepast formulier vullen met de methode setData
description: Verzend het geüploade pdf-bestand voor gegevensextractie en vul het adaptieve formulier met de geëxtraheerde gegevens
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 0%

---

# Ajax-aanroep maken

Wanneer de gebruiker het pdf- dossier heeft geupload, moeten wij een POST tot een servlet roepen en het geuploade document van de PDF in het verzoek van de POST overgaan. Het verzoek van de POST keert een weg naar de uitgevoerde gegevens in crx bewaarplaats terug

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

De servlet gemonteerd op **_/bin/ExtractDataFromPDF_** extraheert de gegevens uit het PDF-bestand en retourneert het pad van het crx-knooppunt waar de geëxtraheerde gegevens zijn opgeslagen.
De [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) wordt vervolgens gebruikt om de gegevens van het adaptieve formulier in te stellen.

## Volgende stappen

[De voorbeeldelementen implementeren](./test-the-solution.md)

