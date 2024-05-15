---
title: Enkele handige UI-tips en -trucs
description: Document om enkele handige tips voor de gebruikersinterface weer te geven
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
exl-id: 13b9cd28-2797-4da9-a300-218e208cd21b
last-substantial-update: 2019-07-07T00:00:00Z
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# Zichtbaarheid wachtwoordveld in-/uitschakelen

Een veelvoorkomend geval is dat de invullers van het formulier kunnen schakelen naar de zichtbaarheid van de tekst die in het wachtwoordveld wordt ingevoerd.
Voor dit gebruik heb ik het oogpictogram van het [Font Awesome Library](https://fontawesome.com/). De vereiste CSS en eye.svg zijn opgenomen in de clientbibliotheek die voor deze demonstratie is gemaakt.



## Voorbeeldcode

Het adaptieve formulier heeft een veld van het type PasswordBox genaamd **ssnField**.

De volgende code wordt uitgevoerd wanneer het formulier wordt geladen

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

De volgende CSS is gebruikt om de positie van **oog** pictogram in het wachtwoordveld

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## Voorbeeld van wachtwoorden in-/uitschakelen implementeren

* Download de [clientbibliotheek](assets/simple-ui-tips.zip)
* Download de [voorbeeldformulier](assets/simple-ui-tricks-form.zip)
* De clientbibliotheek importeren met de [interface van pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Het voorbeeldformulier importeren met het gereedschap [Forms en Document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Een voorbeeld van het formulier bekijken](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)
