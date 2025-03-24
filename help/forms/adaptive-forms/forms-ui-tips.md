---
title: Enkele handige UI-tips en -trucs
description: Document om enkele handige tips voor de gebruikersinterface weer te geven
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
last-substantial-update: 2019-06-09T00:00:00Z
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# Zichtbaarheid wachtwoordveld in-/uitschakelen

Een veelvoorkomend geval is dat de invullers van het formulier kunnen schakelen naar de zichtbaarheid van de tekst die in het wachtwoordveld wordt ingevoerd.
Om dit gebruiksgeval te verwezenlijken, heb ik het oogpictogram van de [ Font Awesome Bibliotheek ](https://fontawesome.com/) gebruikt. De vereiste CSS en eye.svg zijn opgenomen in de clientbibliotheek die voor deze demonstratie is gemaakt.


## Voorbeeldcode

De adaptieve vorm heeft een gebied van type PasswordBox genoemd **ssnField**.

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

Het volgende CSS werd gebruikt om het **oog** pictogram binnen het wachtwoordgebied te plaatsen

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

* Download de [ cliëntbibliotheek ](assets/simple-ui-tips.zip)
* Download de [ steekproefvorm ](assets/simple-ui-tricks-form.zip)
* Importeer de cliëntbibliotheek gebruikend [ pakketmanager UI ](http://localhost:4502/crx/packmgr/index.jsp)
* Importeer de steekproefvorm gebruikend [ Forms en Document ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [ Voorproef de vorm ](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


