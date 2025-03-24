---
title: Automatisch aanvullen in AEM Forms
description: Hiermee kunnen gebruikers snel een vooraf ingevulde lijst met waarden zoeken en selecteren terwijl ze typen, waarbij zoeken en filteren worden gebruikt.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
duration: 47
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# Automatisch uitvoeren voltooid

Implementeer de functie Automatisch aanvullen in AEM-formulieren met de functie Automatisch aanvullen van jQuery.
In het voorbeeld dat bij dit artikel wordt geleverd, worden diverse gegevensbronnen gebruikt (statische array, dynamische array die is gevuld met een REST API-reactie) om de suggesties te vullen terwijl de gebruiker in het tekstveld begint te typen.

De code die wordt gebruikt voor het uitvoeren van de functie voor automatisch aanvullen, is gekoppeld aan de gebeurtenis initialize van het veld.

## Voorstellen voor adres

![ land-suggesties ](assets/auto-complete2.png)



Hier volgt de code die wordt gebruikt om suggesties voor adres op straat op te geven

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```





## Suggesties met emoji&#39;s

![ land-suggesties ](assets/auto-complete3.png)

De volgende code is gebruikt om emoji&#39;s weer te geven in de lijst met suggesties

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

De [ steekproefvorm kan ](assets/auto-complete-form.zip) van hier worden gedownload. Zorg ervoor dat u uw eigen gebruikersnaam/API-sleutel opgeeft met de code-editor voor de code om REST-aanroepen te kunnen uitvoeren.

>[!NOTE]
>
> Voor auto volledig om te werken zorg ervoor uw vorm de volgende cliëntbibliotheek **cq.jquery.ui** gebruikt. Deze clientbibliotheek wordt geleverd bij AEM.
