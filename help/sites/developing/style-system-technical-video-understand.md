---
title: Code voor het AEM Style System
description: In deze video bekijken we de anatomie van de CSS (of LESS) en JavaScript die worden gebruikt om de component Core Title van Adobe Experience Manage te stijleren met behulp van het Stijlsysteem, en hoe deze stijlen worden toegepast op de HTML en DOM.
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1005
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 0%

---

# Begrijpen hoe u code kunt gebruiken voor het Stijlsysteem{#understanding-how-to-code-for-the-aem-style-system}

In deze video bekijken we de anatomie van de CSS (of [!DNL LESS] ) en JavaScript die worden gebruikt om de component Core Title van Experience Manage te stijlen met behulp van het Stijlsysteem, en hoe deze stijlen worden toegepast op de HTML en DOM.


## Begrijpen hoe u code kunt gebruiken voor het Stijlsysteem {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538?quality=12&learn=on)

Het verstrekte Pakket van AEM (**technisch-review.sites.style-system-1.0.0.zip**) installeert de stijl van de voorbeeldtitel, steekproefbeleid voor de componenten van de Lay-out Wij.Retail en van de Titel, en een steekproefpagina.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### De CSS {#the-css}

Hier volgt de definitie [!DNL LESS] voor de voorbeeldstijl die u vindt op:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Voor diegenen die liever CSS gebruiken, wordt onder dit codefragment de CSS weergegeven waarin deze [!DNL LESS] compileert.

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

Het bovenstaande [!DNL LESS] wordt door Experience Manager native gecompileerd naar de volgende CSS.

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### De JavaScript {#example-javascript}

De volgende JavaScript verzamelt en injecteert de datum en tijd van de laatste wijziging van de huidige pagina onder de titeltekst wanneer de stijl van het Voorbeeld op de component van de Titel wordt toegepast.

Het gebruik van jQuery is optioneel en de gebruikte naamgevingsconventies.

Hier volgt de definitie [!DNL LESS] voor de voorbeeldstijl die u vindt op:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## Best practices voor ontwikkeling {#development-best-practices}

### Aanbevolen procedures voor HTML {#html-best-practices}

* HTML (gegenereerd via HTL) moet zo structureel mogelijk semantisch zijn; onnodige groepering/nesting van elementen vermijden.
* HTML-elementen moeten kunnen worden benaderd via CSS-klassen in BEM-stijl.

**Goed** - Alle elementen in de component zijn adresseerbaar via aantekening BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Onjuist** - de lijst en lijstelementen zijn slechts adresseerbaar door elementnaam:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* Het is beter om meer gegevens bloot te stellen en het te verbergen dan te weinig gegevens bloot te stellen die toekomstige achterste-eindontwikkeling vereisen om het bloot te stellen.

   * Door het implementeren van inhoudskeuzelijsten die geschikt zijn voor de auteur, kunt u deze HTML slanken, waarbij auteurs kunnen selecteren welke inhoudselementen naar de HTML worden geschreven. De optie kan vooral belangrijk zijn bij het schrijven van afbeeldingen naar de HTML die mogelijk niet voor alle stijlen worden gebruikt.
   * De uitzondering op deze regel is wanneer dure bronnen, bijvoorbeeld afbeeldingen, standaard beschikbaar worden gemaakt, aangezien gebeurtenisafbeeldingen die door CSS worden verborgen, in dit geval onnodig worden opgehaald.

      * Moderne afbeeldingscomponenten gebruiken vaak JavaScript om de meest geschikte afbeelding voor het gebruikscase (viewport) te selecteren en te laden.

### Aanbevolen werkwijzen voor CSS {#css-best-practices}

>[!NOTE]
>
>Het Systeem van de Stijl maakt een kleine technische divergentie van [ BEM ](https://en.bem.info/), in die zin dat `BLOCK` en `BLOCK--MODIFIER` niet op het zelfde element worden toegepast, zoals die door [ BEM ](https://en.bem.info/) wordt gespecificeerd.
>
>Vanwege productbeperkingen wordt `BLOCK--MODIFIER` in plaats daarvan toegepast op het bovenliggende element van het element `BLOCK` .
>
>Alle andere huurders van [ BEM ](https://en.bem.info/) zouden met moeten worden gericht.

* De preprocessoren van het gebruik zoals [ LESS ](https://lesscss.org/) (die door AEM wordt gesteund) of [ SCSS ](https://sass-lang.com/) (vereist douane bouwt systeem) om voor duidelijke CSS definitie, en herbruikbaarheid toe te staan.

* Houd het gewicht/de specificiteit van de kiezer gelijk. Hiermee voorkomt en lost u conflicten met CSS-trapsgewijze opmaak op.
* Elke stijl indelen in een afzonderlijk bestand.
   * Deze bestanden kunnen worden gecombineerd met behulp van LESS/SCSS `@imports` of als onbewerkte CSS is vereist, via het opnemen van bestanden in de HTML Client Library of aangepaste front-end systemen voor het bouwen van elementen.
* Vermijd het mengen van vele complexe stijlen.
   * Hoe meer stijlen tegelijk op een component kunnen worden toegepast, hoe groter de variatie aan permutaties. Dit kan moeilijk te handhaven/QA/verzekeren merkgroepering worden.
* Gebruik altijd CSS-klassen (na BEM-notatie) om CSS-regels te definiëren.
   * Als het selecteren van elementen zonder CSS-klassen (dus blote elementen) absoluut noodzakelijk is, verplaatst u deze hoger in de CSS-definitie om duidelijk te maken dat ze minder specifiek zijn dan eventuele botsingen met elementen van dat type die wel selecteerbare CSS-klassen hebben.
* Maak de stijl van de `BLOCK--MODIFIER` niet rechtstreeks omdat deze aan het responsieve raster is gekoppeld. Het wijzigen van de weergave van dit element kan van invloed zijn op de rendering en functionaliteit van het responsieve raster. Stijl dus alleen op dit niveau wanneer u het gedrag van het responsieve raster wilt wijzigen.
* Pas stijlbereik toe met `BLOCK--MODIFIER` . `BLOCK__ELEMENT--MODIFIERS` kan in de Component worden gebruikt, maar aangezien `BLOCK` de Component vertegenwoordigt, en de Component is wat wordt gestileerd, is de Stijl &quot;bepaald&quot;en scoped via `BLOCK--MODIFIER`.

Voorbeeld-CSS-selectiestructuur moet als volgt zijn:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Kiezer op het eerste niveau</p> <p>BLOK—MODIFIER</p> </td> 
   <td valign="bottom"><p>Selector op het tweede niveau</p> <p>BLOKKEN</p> </td> 
   <td valign="bottom"><p>Selector op het derde niveau</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Effectieve CSS-kiezer</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—donker</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—donker</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span> <strong><span class="code"> .cmp-list__item </span></strong></p> <p><strong> kleur: blauw;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> kleur: rood;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

In het geval van geneste componenten overschrijdt de CSS-selectierdiepte voor deze geneste componentelementen de selector op het derde niveau. Herhaal hetzelfde patroon voor de geneste component, maar bereik ingesteld door de bovenliggende component `BLOCK` . Met andere woorden, start de geneste component `BLOCK` op het derde niveau en is het niveau van de geneste component `ELEMENT` op het vierde selectorniveau.

### Aanbevolen procedures voor JavaScript {#javascript-best-practices}

De in deze sectie gedefinieerde aanbevolen procedures hebben betrekking op &quot;style-JavaScript&quot; of JavaScript die specifiek is bedoeld om de component te manipuleren voor stilistische in plaats van voor functionele doeleinden.

* Style-JavaScript moet op een verstandige manier worden gebruikt en is een minderheidskwestie.
* Style-JavaScript moet vooral worden gebruikt voor het manipuleren van het DOM van de component ter ondersteuning van opmaakmethoden door CSS.
* Evalueer het gebruik van JavaScript opnieuw als componenten meerdere keren op een pagina worden weergegeven en begrijp de kosten voor berekeningen en opnieuw tekenen.
* Evalueer het gebruik van Javascript opnieuw als deze asynchroon (via AJAX) nieuwe gegevens/inhoud invoegt wanneer de component meerdere keren op een pagina wordt weergegeven.
* Verwerk zowel de publicatie- als de ontwerpervaring.
* Gebruik style-JavaScript waar mogelijk opnieuw.
   * Als bijvoorbeeld voor meerdere stijlen van een component de afbeelding naar een achtergrondafbeelding moet worden verplaatst, kan de stijl-JavaScript één keer worden geïmplementeerd en aan meerdere `BLOCK--MODIFIERs` worden gekoppeld.
* Scheid stijl-JavaScript waar mogelijk van functionele JavaScript.
* Evalueer de kosten van JavaScript versus het direct via HTML zichtbaar maken van deze DOM-wijzigingen in de HTML.
   * Wanneer voor een component die style-JavaScript gebruikt, serverwijzigingen nodig zijn, moet u nagaan of de JavaScript-manipulatie op dit moment kan worden geïntroduceerd en wat de effecten/vertakkingen zijn voor de prestaties en de ondersteuning van de component.

#### Prestatieoverwegingen {#performance-considerations}

* Style-JavaScript moet licht en mager worden gehouden.
* Als u flikkering en onnodige hertekeningen wilt voorkomen, verbergt u eerst de component via `BLOCK--MODIFIER BLOCK` en geeft u deze weer wanneer alle DOM-manipulaties in de JavaScript zijn voltooid.
* De prestaties van de stijl-JavaScript-manipulaties lijken op de standaard jQuery-plug-ins die aan elementen op DOMReady zijn gekoppeld en deze wijzigen.
* Zorg ervoor dat aanvragen worden gecomprimeerd en dat CSS en JavaScript worden geminiateerd.

## Aanvullende bronnen {#additional-resources}

* [ Documentatie van het Systeem van de Stijl ](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [ Creërend de bibliotheken van de Cliënt van AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [ BEM (de Modifier van het Element van het Blok) documentatiewebsite ](https://getbem.com/)
* [ LESS de website van de Documentatie ](https://lesscss.org/)
* [ jQuery website ](https://jquery.com/)
