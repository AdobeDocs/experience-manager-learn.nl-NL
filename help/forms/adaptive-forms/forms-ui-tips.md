---
title: Enkele handige UI-tips en -trucs
description: Document om enkele handige tips voor de gebruikersinterface weer te geven
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 280ea1ec8fc5da644320753958361488872359cc
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 1%

---

# Zichtbaarheid wachtwoordveld in-/uitschakelen

Een veelvoorkomend geval is dat de invullers van het formulier kunnen schakelen naar de zichtbaarheid van de tekst die in het wachtwoordveld wordt ingevoerd.
Voor dit gebruik heb ik het oogpictogram van het [Font Awesome Library](https://fontawesome.com/). De vereiste CSS en eye.svg zijn opgenomen in de clientbibliotheek die voor deze demonstratie is gemaakt.

## Live sample

[Deze mogelijkheid is hier beschikbaar om te testen](https://forms.enablementadobe.com/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)

## Voorbeeldcode

Het adaptieve formulier heeft een veld van het type PasswordBox met de naam **ssnField**.

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
* [Geef een voorbeeld van het formulier weer](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)

