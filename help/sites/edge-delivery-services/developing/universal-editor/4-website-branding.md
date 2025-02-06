---
title: Websitemarkering toevoegen
description: Definieer algemene CSS-, CSS-variabelen en weblettertypen voor een site Edge Delivery Services.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: ecd3ce33204fa6f3f2c27ebf36e20ec26e429981
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# Websitemarkering toevoegen

Begin met het instellen van de algemene branding door algemene stijlen bij te werken, CSS-variabelen te definiëren en weblettertypen toe te voegen. Deze basiselementen zorgen ervoor dat de website consistent en onderhoudsbaar blijft en consistent op de hele site moet worden toegepast.

## Een GitHub-probleem maken

Om alles georganiseerd te houden, gebruik GitHub om het werk te volgen. Eerst, creeer een kwestie GitHub voor dit lichaam van werk:

1. Ga naar de bewaarplaats GitHub (verwijs naar [ creeer een hoofdstuk van het codeproject ](./1-new-code-project.md) voor details).
2. Klik op het **lusje van Kwesties** en dan **Nieuwe kwestie**.
3. Schrijf a **titel** en **beschrijving** voor het te doen werk.
4. Klik **voorleggen nieuwe kwestie**.

De kwestie GitHub wordt later gebruikt wanneer [ creërend een trekkingsverzoek ](#merge-code-changes).

![ GitHub creeert nieuwe kwestie ](./assets/4-website-branding/github-issues.png)

## Een werkende vertakking maken

Om organisatie te handhaven en codekwaliteit te verzekeren, creeer een nieuwe tak voor elk lichaam van het werk. Dit voorkomt dat nieuwe code de prestaties negatief beïnvloedt en zorgt ervoor dat wijzigingen niet actief zijn voordat ze zijn voltooid.

Voor dit hoofdstuk, dat zich richt op de basisstijlen voor de website, maakt u een vertakking met de naam `wknd-styles` .

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## Algemene CSS

Edge Delivery Services gebruiken een algemeen CSS-bestand, dat zich bevindt op `styles/styles.css` , om de algemene stijlen voor de gehele website in te stellen. Met `styles.css` kunt u aspecten zoals kleuren, lettertypen en spatiëring beheren, zodat alles er op de hele site consistent uitziet.

Globale CSS zou agnostisch aan laag-vlakke concepten zoals blokken moeten zijn, die zich op de algemene blik en het gevoel van de plaats, en gedeelde visuele behandelingen concentreren.

Houd er rekening mee dat algemene CSS-stijlen indien nodig kunnen worden overschreven.

### CSS-variabelen

[ CSS variabelen ](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) zijn een grote manier om ontwerpmontages zoals kleuren, doopvonten, en grootte op te slaan. Door variabelen te gebruiken, kunt u deze elementen op één plaats veranderen en het hebben door de volledige plaats bijwerken.

Ga als volgt te werk om de CSS-variabelen aan te passen:

1. Open het bestand `styles/styles.css` in de code-editor.
2. Zoek de declaratie `:root` , waarin algemene CSS-variabelen worden opgeslagen.
3. Wijzig de kleur- en lettertypevariabelen zodat deze overeenkomen met het WKND-merk.

Hier volgt een voorbeeld:


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

Verken de andere variabelen in de sectie `:root` en bekijk de standaardinstellingen.

Als u een website ontwikkelt en dezelfde CSS-waarden herhaalt, kunt u overwegen nieuwe variabelen te maken om de stijlen eenvoudiger te beheren. Voorbeelden van andere CSS-eigenschappen die van CSS-variabelen kunnen profiteren, zijn: `border-radius` , `padding` , `margin` en `box-shadow` .

### Bare-elementen

Bare-elementen worden rechtstreeks via hun elementnaam opgemaakt in plaats van via een CSS-klasse. In plaats van bijvoorbeeld een `.page-heading` CSS-klasse op te maken, worden stijlen op het `h1` -element toegepast met `h1 { ... }` .

In het `styles/styles.css` -bestand wordt een set basisstijlen toegepast op elementen met blote HTML. Websites van Edge Delivery Services geven prioriteit aan het gebruik van onbewerkte elementen, omdat deze zich richten op de native semantische HTML van Edge Delivery Service.

Als u wilt uitlijnen met WKND-branding, kunt u enkele blote elementen opmaken in `styles.css` :

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

Deze stijlen zorgen ervoor dat `h2` -elementen, tenzij overschreven, consistent worden vormgegeven met de WKND-branding, zodat een duidelijke visuele hiërarchie ontstaat. De gedeeltelijke gele onderstreping onder elke `h2` voegt een kenmerkende aanraking aan de rubrieken toe.

### Overgenomen elementen

In Edge Delivery Services verbeteren de code `scripts.js` en `aem.js` van het project automatisch specifieke blote HTML-elementen op basis van hun context binnen de HTML.

Ankerelementen (`<a>`) die bijvoorbeeld op een eigen regel zijn gemaakt (in plaats van inline met omringende tekst) worden als knoppen op basis van deze context beschouwd. Deze ankers worden automatisch voorzien van een container `div` met een CSS-klasse `button-container` en het ankerelement heeft een `button` CSS-klasse toegevoegd.

Wanneer bijvoorbeeld een koppeling op een eigen regel wordt gemaakt, werkt Edge Delivery Services JavaScript zijn DOM als volgt bij:

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

Deze knoppen kunnen worden aangepast aan het WKND-merk, dat bepaalt dat knoppen worden weergegeven als gele rechthoeken met zwarte tekst.

Hier is een voorbeeld van hoe u de &quot;afgeleide knoppen&quot; in `styles.css` kunt opmaken:

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

Dit CSS definieert de basisknopstijlen en bevat WKND-specifieke behandelingen, zoals hoofdletters, een gele achtergrond en zwarte tekst. De eigenschappen `background-color` en `color` gebruiken CSS-variabelen waardoor de knopstijl uitgelijnd blijft met de kleuren van het merk. Deze aanpak zorgt ervoor dat de knoppen op de hele site consistent worden vormgegeven terwijl ze flexibel blijven.

## Weblettertypen

Met Edge Delivery Services worden projecten geoptimaliseerd voor het gebruik van weblettertypen om hoge prestaties te behouden en de impact op Lighthouse-scores tot een minimum te beperken. Deze methode zorgt voor snelle rendering zonder de visuele identiteit van de site in gevaar te brengen. Hieronder wordt beschreven hoe u weblettertypen efficiënt kunt implementeren voor optimale prestaties.

### Lettertypen

Voeg aangepaste weblettertypen toe met CSS `@font-face` -declaraties in het `styles/fonts.css` -bestand. Als u `@font-faces` toevoegt aan `fonts.css` , zorgt u ervoor dat weblettertypen op het optimale moment worden geladen, zodat de Lighthouse-scores behouden blijven.

1. Open `styles/fonts.css`.
2. Voeg de volgende `@font-face` -declaraties toe om de WKND-lettertypen op te nemen: `Asar` en `Source Sans Pro` .

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

De doopvonten die in dit leerprogramma worden gebruikt zijn afkomstig van de Doopvonten van Google, maar de Webdoopvonten kunnen van om het even welke doopvontleverancier, met inbegrip van [ Adobe Fonts ](https://fonts.adobe.com/) worden verkregen.

+++Lokale bestanden met weblettertypen gebruiken

U kunt ook bestanden met weblettertypen kopiëren naar project in de map `/fonts` en naar deze bestanden verwijzen in de declaraties van `@font-face` .

In deze zelfstudie worden externe, gehoste weblettertypen gebruikt, zodat u deze gemakkelijker kunt volgen.

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

Werk ten slotte de CSS-variabelen van `styles/styles.css` bij om de nieuwe lettertypen te gebruiken:

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

`roboto-fallback` en `roboto-condensed-fallback` zijn fallback doopvonten die in de [ sectie van de Fallback doopvonten ](#fallback-fonts) worden bijgewerkt om zich te richten om de douane `Asar` en `Source Sans Pro` weblettertypen te steunen.

### Alternatieve lettertypen

Weblettertypen zijn vaak van invloed op de prestaties vanwege hun grootte, waardoor Cumulatieve CLS-scores (Layout Shift) worden verhoogd en de totale Lightroom-scores afnemen. Voor directe tekstweergave tijdens het laden van weblettertypen gebruiken Edge Delivery Services voor projecten browsereigen fallback-lettertypen. Deze aanpak zorgt voor een vloeiende gebruikerservaring terwijl het gewenste lettertype van toepassing is.

Om de beste fallback doopvont te selecteren, de uitbreiding van Chrome van de Doopvont van de gebruiksAdobe [ Hallback ](https://www.aem.live/developer/font-fallback), die een dicht passende doopvont voor browsers bepaalt om te gebruiken alvorens de ladingen van de douanedoopvont. De resulterende fontdeclaraties voor fallback moeten aan het `styles/styles.css` -bestand worden toegevoegd om de prestaties te verbeteren en gebruikers een naadloze ervaring te bieden.

![ de uitbreiding van Chrome van de Fallback van de Doopvont 1} {align=center}](./assets/4-website-branding/font-fallback-chrome-plugin.png)

Om de [ extensie van Chrome van de Fontfallback van de Helix te gebruiken ](https://www.aem.live/developer/font-fallback), zorg ervoor dat de Web-pagina weblettertypen heeft die in de zelfde variaties worden toegepast die op de website van Edge Delivery Services worden gebruikt. Dit leerprogramma toont de uitbreiding op [ wknd.site ](http://wknd.site/us/en.html) aan. Wanneer het ontwikkelen van een website, pas de uitbreiding op de plaats toe die aan eerder dan aan [ wknd.site ](http://wknd.site/us/en.html) wordt gewerkt.

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

Voeg de fallback font-family namen toe aan de CSS-lettertypen in `styles/styles.css` na de &#39;echte&#39; namen van lettertypen.

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## Ontwikkelvoorbeeld

Terwijl u CSS toevoegt, worden de wijzigingen automatisch opnieuw geladen in de lokale ontwikkelomgeving van de AEM CLI, zodat u snel en gemakkelijk kunt zien hoe de CSS het blok beïnvloedt.

![ Voorproef van de Ontwikkeling van het merk CSS van WKND ](./assets/4-website-branding/preview.png)


## Definitieve CSS-bestanden downloaden

U kunt de bijgewerkte CSS-bestanden downloaden van de onderstaande koppelingen:

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## De CSS-bestanden plaatsen

Zorg ervoor aan [ vaak ](./3-local-development-environment.md#linting) uw codeveranderingen plukken om ervoor te zorgen zij schoon en verenigbaar zijn. Regelmatig koppelen helpt problemen vroegtijdig op te vangen en verkort de totale ontwikkelingstijd. U kunt uw werk pas samenvoegen met de hoofdvertakking als alle problemen met de koppeling zijn opgelost.

```bash
$ npm run lint:css
```

## Codewijzigingen samenvoegen

Voeg de veranderingen in de `main` tak op GitHub samen om toekomstig werk op deze updates te bouwen.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

Zodra de veranderingen aan de `wknd-styles` tak worden geduwd, creeer een trekkrachtverzoek op GitHub om hen in de `main` tak samen te voegen.

1. Navigeer aan de bewaarplaats GitHub van [ tot een nieuw project ](./1-new-code-project.md) hoofdstuk.
2. Klik de **verzoeken van de Trek** tabel en selecteer **Nieuwe trekkingsverzoek**.
3. Stel `wknd-styles` in als de bronvertakking en `main` als de doelvertakking.
4. Herzie de veranderingen en klik **creeer trekkingsverzoek**.
5. In de details van het trekkingsverzoek, **voeg het volgende** toe:

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * De `Fix #1` verwijst naar de eerder gemaakte GitHub-kwestie.
   * De test URLs vertelt AEM de Synchronisatie van de Code welke takken voor bevestiging en vergelijking te gebruiken. De URL &quot;Na&quot; gebruikt de werkvertakking `wknd-styles` om te controleren hoe de wijzigingen in de code van invloed zijn op de prestaties van de website.

6. Klik **creëren trektrekverzoek**.
7. Wacht op de [ AEM app GitHub van de Synchronisatie van de Code ](./1-new-code-project.md) aan **volledige kwaliteitscontroles**. Als ze mislukken, lost u de fouten op en voert u de controles opnieuw uit.
8. Zodra de controles overgaan, **voegt het trektrekkingsverzoek** in `main` samen.

Als de wijzigingen zijn samengevoegd in `main`, worden ze niet beschouwd als geïmplementeerd voor de productie en kan de nieuwe ontwikkeling op basis van deze updates plaatsvinden.
