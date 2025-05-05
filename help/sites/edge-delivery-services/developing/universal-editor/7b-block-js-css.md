---
title: Een blok ontwikkelen met CSS en JS
description: Ontwikkel een blok met CSS en JavaScript voor Edge Delivery Services, editable gebruikend de Universele Redacteur.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 41c4cfcf-0813-46b7-bca0-7c13de31a20e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---

# Een blok ontwikkelen met CSS en JavaScript

In het [ vorige hoofdstuk ](./7b-block-js-css.md), werd het stileren van een blok gebruikend slechts CSS behandeld. Nu gaat de focus naar het ontwikkelen van een blok met zowel JavaScript als CSS.

In dit voorbeeld wordt getoond hoe u een blok op drie manieren kunt verbeteren:

1. Aangepaste CSS-klassen toevoegen.
1. Gebeurtenislisteners gebruiken om beweging toe te voegen.
1. Bepalingen en voorwaarden afhandelen die optioneel in de tekst van de teaser kunnen worden opgenomen.

## Vaak voorkomende gevallen

Deze aanpak is met name handig in de volgende scenario&#39;s:

- **Extern CSS beheer:** wanneer CSS van het blok buiten Edge Delivery Services wordt beheerd en niet met zijn structuur van HTML richt.
- **Extra attributen:** wanneer de extra attributen, zoals [ ARIA ](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) voor toegankelijkheid of [ microgegevens ](https://developer.mozilla.org/en-US/docs/Web/HTML/Microdata), worden vereist.
- **de verhogingen van JavaScript:** wanneer de interactieve eigenschappen, zoals gebeurtenisluisteraars, noodzakelijk zijn.

Deze methode is afhankelijk van native JavaScript DOM-manipulatie in de browser, maar vereist voorzichtigheid bij het wijzigen van de DOM, vooral bij het verplaatsen van elementen. Dergelijke wijzigingen kunnen de ontwerpervaring van de Universal Editor verstoren. Ideaal gezien, zou het de inhoudsmodel van het blok [&#128279;](./5-new-block.md#block-model) moeten worden bedacht om de behoefte aan uitgebreide DOM veranderingen te minimaliseren.

## HTML blokkeren

Om blokontwikkeling te benaderen, begin door DOM te herzien dat door Edge Delivery Services wordt blootgesteld. De structuur wordt verbeterd met JavaScript en opgemaakt met CSS.

>[!BEGINTABS]

>[!TAB  DOM om te versieren ]

Hieronder ziet u het DOM van het teaserblok dat als doel heeft het versieren met JavaScript en CSS.

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB hoe te om DOM  te vinden]

Als u het DOM wilt zoeken om te versieren, opent u de pagina met het niet-versierde blok in uw lokale ontwikkelomgeving, selecteert u het blok en inspecteert u het DOM.

![ blok DOM van de Inspectie ](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]


## JavaScript blokkeren

Als u JavaScript-functionaliteit aan een blok wilt toevoegen, maakt u een JavaScript-bestand in de map van het blok met dezelfde naam als het blok, bijvoorbeeld `/blocks/teaser/teaser.js` .

Het JavaScript-bestand moet een standaardfunctie exporteren:

```javascript
export default function decorate(block) { ... }
```

De standaardfunctie gebruikt het DOM-element/de DOM-structuur die het blok in de Edge Delivery Services HTML vertegenwoordigt en bevat de aangepaste JavaScript die wordt uitgevoerd wanneer het blok wordt gerenderd.

In dit voorbeeld voert JavaScript drie hoofdhandelingen uit:

1. Voegt een gebeurtenislistener toe aan de CTA-knop en zoomt op de afbeelding tijdens de muisaanwijzer.
1. Hiermee voegt u semantische CSS-klassen toe aan de elementen van het blok, wat handig is bij het integreren van bestaande CSS-ontwerpsystemen.
1. Voegt een speciale CSS-klasse toe aan alinea&#39;s die beginnen met `Terms and conditions:` .

[!BADGE &#x200B; /blocks/teaser/teaser.js]{type=Neutral tooltip="Bestandsnaam van codevoorbeeld hieronder."}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Adds a zoom effect to image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's' DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
   * Entry point to block's JavaScript.
   * Must be exported as default and accept a block's DOM element.
   * This function is called by the project's style.js, and passed the block's element.
   *
   * @param {HTMLElement} block represents the block's' DOM element/tree
   */
export default function decorate(block) {
  /* This JavaScript makes minor adjustments to the block's DOM */

  // Dress the DOM elements with semantic CSS classes so it's obvious what they are.
  // If needed we could also add ARIA roles and attributes, or add/remove/move DOM elements.

  // Add a class to the first picture element to target it with CSS
  block.querySelector('picture').classList.add('image-wrapper');

  // Use previously applied classes to target new elements
  block.querySelector('.image-wrapper img').classList.add('image');

  // Mark the second/last div as the content area (white, bottom aligned box w/ text and cta)
  block.querySelector(':scope > div:last-child').classList.add('content');

  // Mark the first H1-H6 as a title
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();

    // If the paragraph starts with Terms and conditions: then style it as such
    if (innerHTML?.startsWith("Terms and conditions:")) {
      /* If a paragraph starts with '*', add a special CSS class. */
      p.classList.add('terms-and-conditions');
    }
  });

  // Add event listeners to the block
  addEventListeners(block);
}
```

## CSS blokkeren

Als u a `teaser.css` in het [ vorige hoofdstuk ](./7a-block-css.md) creeerde schrap het, of noem het aan `teaser.css.bak` anders, aangezien dit hoofdstuk verschillende CSS voor het teaser blok uitvoert.

Maak een `teaser.css` -bestand in de map van het blok. Dit bestand bevat de CSS-code die het blok opmaakt. Deze CSS-code is gericht op de elementen van het blok en de specifieke, semantische CSS-klassen die door de JavaScript in `teaser.js` zijn toegevoegd.

Zeldelementen kunnen nog steeds rechtstreeks worden vormgegeven, of met de aangepaste, toegepaste CSS-klassen. Voor complexere blokken, kan het toepassen van semantische CSS klassen helpen CSS begrijpelijker en houdbaar maken, vooral wanneer het werken met grotere teams over langere periodes.

[ als vóór ](./7a-block-css.md#develop-a-block-with-css), werkingsgebied CSS aan `.block.teaser` gebruikend [ CSS het nesten ](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting) om conflict met andere blokken te vermijden.

[!BADGE &#x200B; /blocks/teaser/teaser.css]{type=Neutral tooltip="Bestandsnaam van codevoorbeeld hieronder."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
    overflow: hidden; 

    /* The teaser image */
    .image-wrapper {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;
        overflow: hidden; 

        .image {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
            transform: scale(1); 
            transition: transform 0.6s ease-in-out;

            .zoom {
                transform: scale(1.1);
            }            
        }
    }

    /* The teaser text content */
    .content {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;
  
        .title {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        .title::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
            animation: teaser-fade-in .6s;
        
            &.terms-and-conditions {
                font-size: var(--body-font-size-xs);
                color: var(--secondary-color);
                padding: .5rem 1rem;
                font-style: italic;
                border: solid var(--light-color);
                border-width: 0 0 0 10px;
            }
        }

        /* Add underlines to links in the text */
        a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        .button-container {
            margin: 0;
            padding: 0;
        
            .button {   
                background-color: var(--primary-color);
                border-radius: 0;
                color: var(--dark-color);
                font-size: var(--body-font-size-xs);
                font-weight: bold;
                padding: 1em 2.5em;
                margin: 0;
                text-transform: uppercase;
            }
        }
    }
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## Voorwaarden en bepalingen toevoegen

De bovenstaande implementatie voegt ondersteuning toe voor speciale opmaakalinea&#39;s die beginnen met de tekst `Terms and conditions:` . Als u deze functionaliteit wilt valideren, werkt u in de Universal Editor de tekstinhoud van het teasblok bij en voegt u de voorwaarden en bepalingen in.

Volg de stappen in de [ auteur een blok ](./6-author-block.md), en geef de tekst uit om a **termijnen en voorwaarden** paragraaf aan het eind te omvatten:

```
WKND Adventures

Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.

Terms and conditions: By signing up, you agree to the rules for participation and booking.
```

Controleer of de alinea wordt gerenderd met de stijl van de voorwaarden en bepalingen in de lokale ontwikkelomgeving. Herinner me, wijzen deze codeveranderingen niet op in Universele Redacteur tot zij [ aan een tak op GitHub ](#preview-in-universal-editor) worden geduwd dat de Universele Redacteur aan gebruik is gevormd.

## Ontwikkelvoorbeeld

Terwijl de CSS en JavaScript worden toegevoegd, laadt de lokale ontwikkelomgeving van de AEM CLI de wijzigingen hot-reload, zodat u snel en gemakkelijk kunt zien hoe code van invloed is op het blok. Houd de muisaanwijzer boven de CTA en controleer of de afbeelding van de taser in- en uitzoomt.

![ Lokale ontwikkelingsvoorproef van teaser gebruikend CSS en JS ](./assets/7b-block-js-css/local-development-preview.png)

## Uw code plaatsen

Zorg ervoor aan [ vaak ](./3-local-development-environment.md#linting) uw codescheidingen om het schoon en verenigbaar te houden. Regelmatige lijnen helpen vangstkwesties vroeg, die algemene ontwikkelingstijd verminderen. U kunt uw ontwikkelingswerk pas samenvoegen in de `main` -vertakking als alle problemen met koppelingen zijn opgelost.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## Voorvertoning in Universal Editor

Als u wijzigingen wilt weergeven in de AEM Universal Editor, voegt u deze toe, past u ze toe en duwt u ze door naar de vertakking Git-opslagplaats die door de Universal Editor wordt gebruikt. Dit zorgt ervoor dat de blokimplementatie de ontwerpervaring niet verstoort.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for teaser block"
$ git push origin teaser
```

U kunt nu een voorvertoning van de wijzigingen weergeven in de Universal Editor wanneer u de queryparameter `?ref=teaser` toevoegt.

![ Taser in Universele Redacteur ](./assets/7b-block-js-css/universal-editor-preview.png)
