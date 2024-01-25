---
title: Clientbibliotheek maken
description: Clientbibliotheekcode voor het ophalen van het volgende formulier ter ondertekening
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
duration: 42
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---

# Een clientbibliotheek maken

Creeer een Bibliotheek van de douaneCliënt, clientlib voor kort, om de url parameters te halen gaat die parameters in de vraag van de GET over. De aanroep van de GET wordt uitgevoerd naar een servlet die is gemonteerd op /bin/getnextformtosign en die de URL retourneert van het volgende formulier dat moet worden ondertekend in het pakket.

Het volgende is de code die in de clientlib javascript functie wordt gebruikt


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Assets

[Clientlib kan hier worden gedownload](assets/get-next-form-client-lib.zip)

## Volgende stappen

[Aangepaste formuliersjabloon maken voor dit gebruiksgeval](./create-af-template.md)