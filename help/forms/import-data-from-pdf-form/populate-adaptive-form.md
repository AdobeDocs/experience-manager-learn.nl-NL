---
title: Aangepast formulier vullen met de methode setData
description: Verzend het geüploade pdf-bestand voor gegevensextractie en vul het adaptieve formulier met de geëxtraheerde gegevens
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 0%

---

# Ajax-aanroep maken

Wanneer de gebruiker het pdf-bestand heeft geüpload, moeten we een POST-aanroep naar een servlet maken en het geüploade PDF-document doorgeven in de POST-aanvraag. De POST-aanvraag retourneert een pad naar de geëxporteerde gegevens in de crx-opslagplaats

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

De servlet op **_/bin/ExtractDataFromPDF_** haalt de gegevens uit het dossier van PDF uit en keert de weg van de crx knoop terug waar de gehaalde gegevens worden opgeslagen.
De [ GuideBridge setData ](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) methode wordt dan gebruikt om de gegevens van de adaptieve vorm te plaatsen.

## Volgende stappen

[De voorbeeldelementen implementeren](./test-the-solution.md)
