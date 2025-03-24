---
title: Aanklikbare afbeeldingscomponent maken
description: Aanklikbare afbeeldingscomponent maken in AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 54344a6d-51d3-4a63-b1f1-283bddbc0f8f
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 0%

---

# Handle click-gebeurtenis

Maak een clientbibliotheek en koppel de clientbibliotheek aan de component.

Voeg de volgende code toe in uw javascript-bestand van de clientbibliotheek die u wilt afhandelen om op de gebeurtenis te klikken.
Gebaseerd op geselecteerde staat, kunnen de aangewezen gegevens die door het eindpunt zijn teruggekeerd worden getoond. De details van het eindpunt en de te tonen gegevens zullen van uw gebruiksgeval afhangen.



```javascript
document.addEventListener("DOMContentLoaded", function(event)
  {
    const apiUrl = 'API end point to return data based on the state selected';
       // Select all <path> elements
    const paths = document.querySelectorAll('path');
    const tooltip = document.getElementById('tooltip');
    const svg = document.getElementById('svg');
    const states = document.querySelectorAll('path');
    // Loop through each <path> element and add an event listener
    states.forEach(state =>
     {
                     state.addEventListener('click', function(event)
                  {
                     alert('path clicked:'+ this.id);
                    fetch(apiUrl+this.id)
                    .then(response =>
                        {
                            // Check if the response is ok (status code in the range 200-299)
                            if (!response.ok)
                            {
                                  throw new Error('Network response was not ok ' + response.statusText);
                            }
                            // Parse the JSON from the response
                            console.log(response);
                            return response.text();
                          })
                        .then(data => {
                                // Handle the data
                              console.log(data);
                            })
                        .catch(error => {
                            // Handle any errors
                            console.error('There was a problem with the fetch operation:', error);
                            });
                     });
        });

});
```
