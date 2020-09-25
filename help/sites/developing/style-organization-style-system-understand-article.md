---
title: Best practices voor stijlsystemen met AEM Sites
description: Een gedetailleerd artikel waarin de beste werkwijzen worden uitgelegd voor het implementeren van het stijlsysteem met Adobe Experience Manager Sites.
feature: style-system
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1528'
ht-degree: 0%

---


# Best practices voor stijlsystemen{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Lees de inhoud bij het [Begrijpen hoe te om voor het Systeem](style-system-technical-video-understand.md)van de Stijl te coderen, om een inzicht in de BEM-achtige overeenkomsten te verzekeren die door AEM Systeem van de Stijl worden gebruikt.

Er zijn twee hoofdstijlen of stijlen die voor het Systeem van de Stijl van de AEM worden uitgevoerd:

* **Lay-outstijlen**
* **Stijlen weergeven**

**Lay-outstijlen** zijn van invloed op veel elementen van een component om een goed definieerbare en identificeerbare uitvoering (ontwerp en lay-out) van de component te maken, die vaak wordt uitgelijnd op een specifiek herbruikbaar merk. Een teascomponent kan bijvoorbeeld worden weergegeven in de traditionele, op kaarten gebaseerde lay-out, in een horizontale promotiestijl of als een hoofdlay-out die tekst op een afbeelding bedekt.

**Weergavestijlen** worden gebruikt om kleine variaties in layoutstijlen aan te brengen, maar veranderen niet de fundamentele aard of intentie van de indelingsstijl. Een hoofdopmaakstijl kan bijvoorbeeld weergavestijlen hebben die het kleurenschema wijzigen van het kleurenschema voor het primaire merk in het kleurenschema voor het secundaire merk.

## Tips en trucs voor organisatie {#style-organization-best-practices}

Wanneer u de stijlnamen definieert die beschikbaar zijn voor AEM auteurs, kunt u het beste:

* Stijlen benoemen met behulp van een woordenlijst die de auteurs begrijpen
* Het aantal stijlopties minimaliseren
* Alleen stijlopties en combinaties beschikbaar maken die zijn toegestaan door merkstandaarden
* Alleen stijlcombinaties met een effect beschikbaar maken
   * Als ineffectieve combinaties worden blootgesteld, moet u ervoor zorgen dat deze ten minste geen slecht effect hebben

Aangezien het aantal mogelijke stijlcombinaties beschikbaar aan AEM auteurs stijgt, bestaan de meer permutaties die QA&#39;d moeten zijn en tegen merknormen moeten worden bevestigd. Te veel opties kunnen auteurs ook verwarren, omdat het onduidelijk kan worden welke optie of combinatie nodig is om het gewenste effect te verkrijgen.

### Stijlnamen versus CSS-klassen {#style-names-vs-css-classes}

Stijlnamen, of de opties die aan AEM auteurs worden voorgesteld, en de het uitvoeren CSS klassennamen worden ontkoppeld in AEM.

Hierdoor kunnen stijlopties worden gelabeld in een woordenschat die duidelijk is en door de AEM auteurs wordt begrepen, maar kunnen CSS-ontwikkelaars de CSS-klassen op een semantische manier een naam geven die toekomstbestendig is. Bijvoorbeeld:

Een component moet de opties hebben om met de **primaire** en **secundaire** kleuren van het merk te worden gekleurd, echter, de AEM auteurs kennen de kleuren als **groen** en **geel**, eerder dan de ontwerptaal van primair en secundair.

Het systeem van de Stijl van de AEM kan deze het kleurenStijlen van de Vertoning blootstellen gebruikend auteursvriendelijke etiketten **Groen** en **Geel**, terwijl het toestaan van de CSS ontwikkelaars om semantische noeming van te gebruiken `.cmp-component--primary-color` `.cmp-component--secondary-color` en de daadwerkelijke stijlimplementatie in CSS te bepalen.

De stijlnaam **Groen** is toegewezen aan `.cmp-component--primary-color`, en **Geel** aan `.cmp-component--secondary-color`.

Als de merkkleur van het bedrijf in de toekomst verandert, is alles dat moet worden veranderd de enige implementaties van `.cmp-component--primary-color` en `.cmp-component--secondary-color`, en de namen van de Stijl.

## De component Teaser als voorbeeld gebruikt hoofdletters/kleine letters {#the-teaser-component-as-an-example-use-case}

Hieronder ziet u een voorbeeld van het gebruik van de stijl van een Taser-component voor verschillende lay-out- en weergavestijlen.

Hiermee wordt uitgelegd hoe stijlnamen (beschikbaar voor auteurs) en hoe de ondersteunende CSS-klassen worden ingedeeld.

### Configuratie van componentstijlen voor teaser {#component-styles-configuration}

In de volgende afbeelding ziet u de [!UICONTROL Styles] configuratie voor de component Teaser voor de variaties die in het gebruiksgeval worden besproken.

De [!UICONTROL Style Group] namen, de Lay-out, en de Vertoning, door voorkomen passen aan de algemene concepten de stijlen van de Vertoning en de stijlen van de Lay-out die worden gebruikt om types van stijlen in dit artikel conceptueel te categoriseren.

De [!UICONTROL Style Group] namen en het aantal van [!UICONTROL Style Groups] moeten worden aangepast aan de gebruiks- en projectspecifieke conventies voor componentstijlen.

De naam van de stijlgroep **Weergeven** had bijvoorbeeld de naam **Kleuren** kunnen hebben.

![Stijlgroep weergeven](assets/style-config.png)

### Menu Stijlselectie {#style-selection-menu}

In de onderstaande afbeelding ziet u de interactie tussen de auteurs van het [!UICONTROL Style] menu en de juiste stijlen voor de component. Merk op dat de [!UICONTROL Style Grpi] namen, evenals de namen van de Stijl, allen aan de auteur worden blootgesteld.

![Vervolgkeuzelijst Stijl](assets/style-menu.png)

### Standaardstijl {#default-style}

De standaardstijl is vaak de meest gebruikte stijl van de component en de standaard, niet-opgemaakte weergave van het gummetje wanneer deze aan een pagina wordt toegevoegd.

Afhankelijk van de standaardinstelling kan de CSS-stijl rechtstreeks worden toegepast op de `.cmp-teaser` (zonder modifiers) of op een `.cmp-teaser--default`.

Als de standaardstijlregels vaker dan niet op alle variaties van toepassing zijn, is het best om als klassen van CSS van de standaardstijl te gebruiken, aangezien alle variaties hen impliciet zouden moeten erven, veronderstellend de overeenkomsten van BEM worden gevolgd. `.cmp-teaser` Als niet, zouden zij via de standaardbepaling, zoals moeten worden toegepast, `.cmp-teaser--default`, die beurtelings aan het gebied van de de stijlconfiguratie van de [component Standaard CSS Klassen](#component-styles-configuration) moet worden toegevoegd, anders zullen deze stijlregels in elke variatie moeten worden met voeten getreden.

Het is zelfs mogelijk om een &quot;genoemde&quot;stijl als standaardstijl toe te wijzen, bijvoorbeeld, de hieronder `(.cmp-teaser--hero)` bepaalde stijl van het Hoofdlettergebruik, nochtans is het duidelijker om de standaardstijl tegen de `.cmp-teaser` of `.cmp-teaser--default` CSS klassenimplementaties uit te voeren.

>[!NOTE]
>
>De standaardlay-outstijl heeft GEEN weergavestijlnaam, maar de auteur kan een weergaveoptie selecteren in het selectiegereedschap AEM Stijl.
>
>Dit is in strijd met de beste praktijken:
>
>**Alleen stijlcombinaties met een effect beschikbaar maken**
>
>Als een auteur de weergavestijl **Groen** selecteert, gebeurt er niets.
>
>In dit geval geven we deze schending toe, aangezien alle andere layoutstijlen kleurbaar moeten zijn met de merkkleuren.
>
>In de **sectie Promo (rechts uitgelijnd)** hieronder zien we hoe u ongewenste stijlcombinaties kunt voorkomen.

![standaardstijl](assets/default.png)

* **Lay-outstijl**
   * Standaard
* **Weergavestijl**
   * Geen
* **Effectieve CSS-klassen**: `.cmp-teaser--promo` of `.cmp-teaser--default`

### Promo-stijl {#promo-style}

De lay-outstijl **** Promo wordt gebruikt om inhoud van hoge waarde op de plaats te bevorderen en horizontaal opgemaakt om een band van ruimte op de Web-pagina op te nemen en moet stijl-geschikt door merkkleuren zijn, met de standaard lay-outstijl Promo die zwarte teksten gebruikt.

Hiertoe worden een **lay-outstijl** van **Promo** en de **vertoningsstijlen** van **Groen** en **Geel** gevormd in het Systeem van de Stijl van de AEM voor de component van de Teaser.

#### Standaardwaarde voor aanbieding

![standaard](assets/promo-default.png)

* **Lay-outstijl**
   * Stijlnaam: **Aanbieding**
   * CSS-klasse: `cmp-teaser--promo`
* **Weergavestijl**
   * Geen
* **Effectieve CSS-klassen**: `.cmp-teaser--promo`

#### Primaire aanbieding

![promo primary](assets/promo-primary.png)

* **Lay-outstijl**
   * Stijlnaam: **Aanbieding**
   * CSS-klasse: `cmp-teaser--promo`
* **Weergavestijl**
   * Stijlnaam: **Groen**
   * CSS-klasse: `cmp-teaser--primary-color`
* **Effectieve CSS-klassen**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Secundaire aanbieding

![Secundaire aanbieding](assets/promo-secondary.png)

* **Lay-outstijl**
   * Stijlnaam: **Aanbieding**
   * CSS-klasse: `cmp-teaser--promo`
* **Weergavestijl**
   * Stijlnaam: **Geel**
   * CSS-klasse: `cmp-teaser--secondary-color`
* **Effectieve CSS-klassen**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Promo Right-align style {#promo-r-align}

De lay-outstijl **Promo rechts uitgelijnd** is een variant van de Promo-stijl die de locatie van de afbeelding en tekst (afbeelding rechts, tekst links) omdraait.

De juiste uitlijning, in de kern, is een weergavestijl die u in het AEM Stijlsysteem kunt invoeren als een weergavestijl die u samen met de stijl van de Promo-lay-out selecteert. Dit is in strijd met de beste praktijken van:

**Alleen stijlcombinaties met een effect beschikbaar maken**

..die al is overtreden in de [standaardstijl](#default-style).

Omdat de juiste uitlijning alleen van invloed is op de indelingsstijl Promo en niet op de andere 2 lay-outstijlen: Standaard en als held kunnen we een nieuwe indelingsstijlpromotie maken (rechts uitgelijnd) die de CSS-klasse bevat die de inhoud van de Promo-lay-outstijlen rechts uitlijnt: `cmp -teaser--alternate`.

Deze combinatie van meerdere stijlen tot één Stijl-item kan ook het aantal beschikbare stijlen en stijlpermutaties verminderen. Dit is het beste om deze te minimaliseren.

De naam van de CSS-klasse hoeft niet overeen te komen met de auteurvriendelijke nomenclatuur van &#39;rechts uitgelijnd&#39;. `cmp-teaser--alternate`

#### Standaard naar rechts uitgelijnde aanbieding

![promotie rechts uitgelijnd](assets/promo-alternate-default.png)

* **Lay-outstijl**
   * Stijlnaam: **Aanbieding (rechts uitgelijnd)**
   * CSS-klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Weergavestijl**
   * Geen
* **Effectieve CSS-klassen**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Primaire aanbieding rechts uitgelijnd

![Primaire aanbieding rechts uitgelijnd](assets/promo-alternate-primary.png)

* **Lay-outstijl**
   * Stijlnaam: **Aanbieding (rechts uitgelijnd)**
   * CSS-klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Weergavestijl**
   * Stijlnaam: **Groen**
   * CSS-klasse: `cmp-teaser--primary-color`
* **Effectieve CSS-klassen**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Secundaire promotie rechts uitgelijnd

![Secundaire promotie rechts uitgelijnd](assets/promo-alternate-secondary.png)

* **Lay-outstijl**
   * Stijlnaam: **Aanbieding (rechts uitgelijnd)**
   * CSS-klassen: `cmp-teaser--promo cmp-teaser--alternate`
* **Weergavestijl**
   * Stijlnaam: **Geel**
   * CSS-klasse: `cmp-teaser--secondary-color`
* **Effectieve CSS-klassen**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Hero style {#hero-style}

In de lay-outstijl Hoofdafbeelding wordt de afbeelding van de componenten weergegeven als een achtergrond met de titel en de koppeling overlay. De hoofdlay-outstijl moet, net als de Promo-lay-outstijl, kleurbaar zijn met brandkleuren.

Als u de hoofdlay-outstijl wilt kleuren met merkkleuren, kunt u dezelfde weergavestijlen gebruiken die voor de Promo-lay-outstijl worden gebruikt.

Per component wordt de stijlnaam toegewezen aan de enkele set CSS-klassen. Dit betekent dat de CSS-klassennamen die de achtergrond van de indelingsstijl Promo kleuren, de tekst en koppeling van de hoofdlay-outstijl moeten kleuren.

Dit kan triviaal worden bereikt door de CSS-regels te verkennen, maar dit vereist wel dat de CSS-ontwikkelaars begrijpen hoe deze permutaties worden toegepast op AEM.

CSS voor het kleuren van de achtergrond van de lay-outstijl **Promote** met de primaire (groene) kleur:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS voor het kleuren van de tekst van de **hoofdlay-outstijl** met de primaire (groene) kleur:

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Hero Default

![Hero Style](assets/hero.png)

* **Lay-outstijl**
   * Stijlnaam: **Hero**
   * CSS-klasse: `cmp-teaser--hero`
* **Weergavestijl**
   * Geen
* **Effectieve CSS-klassen**: `.cmp-teaser--hero`

#### Primaire hoofdafbeelding van Hero

![Primaire hoofdafbeelding van Hero](assets/hero-primary.png)

* **Lay-outstijl**
   * Stijlnaam: **Aanbieding**
   * CSS-klasse: `cmp-teaser--hero`
* **Weergavestijl**
   * Stijlnaam: **Groen**
   * CSS-klasse: `cmp-teaser--primary-color`
* **Effectieve CSS-klassen**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero Secondary

![Hero Secondary](assets/hero-secondary.png)

* **Lay-outstijl**
   * Stijlnaam: **Aanbieding**
   * CSS-klasse: `cmp-teaser--hero`
* **Weergavestijl**
   * Stijlnaam: **Geel**
   * CSS-klasse: `cmp-teaser--secondary-color`
* **Effectieve CSS-klassen**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Aanvullende bronnen {#additional-resources}

* [Stijlsysteemdocumentatie](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM Client-bibliotheken maken](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Documentatiewebsite BEM (Block Element Modifier)](https://getbem.com/)
* [LESS-documentatiewebsite](https://lesscss.org/)
* [jQuery-website](https://jquery.com/)
