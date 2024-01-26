---
title: Best practices voor stijlsystemen met AEM Sites
description: Een gedetailleerd artikel waarin de beste werkwijzen worden uitgelegd voor het implementeren van het stijlsysteem met Adobe Experience Manager Sites.
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Article
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
duration: 432
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 0%

---

# Best practices voor stijlsystemen{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Controleer de inhoud op [Begrijpen hoe u code kunt gebruiken voor het Stijlsysteem](style-system-technical-video-understand.md), om inzicht te krijgen in de BEM-achtige conventies die worden gebruikt door AEM Stijlsysteem.

Er zijn twee hoofdstijlen of stijlen die voor het Systeem van de Stijl van de AEM worden uitgevoerd:

* **Lay-outstijlen**
* **Stijlen weergeven**

**Lay-outstijlen** is van invloed op veel elementen van een component om een goed definieerbare en identificeerbare uitvoering (ontwerp en lay-out) van de component te maken, die vaak wordt uitgelijnd op een specifiek herbruikbaar merkconcept. Een teascomponent kan bijvoorbeeld worden weergegeven in de traditionele, op kaarten gebaseerde lay-out, in een horizontale promotiestijl of als een hoofdlay-out die tekst op een afbeelding bedekt.

**Stijlen weergeven** worden gebruikt om kleine variaties in layoutstijlen aan te passen, maar ze veranderen de fundamentele aard of intentie van de layoutstijl niet. Een hoofdopmaakstijl kan bijvoorbeeld weergavestijlen hebben die het kleurenschema wijzigen van het kleurenschema voor het primaire merk in het kleurenschema voor het secundaire merk.

## Best practices voor organisatie {#style-organization-best-practices}

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

Een onderdeel moet de opties hebben die met het merk moeten worden gekleurd **primair** en **secundair** kleuren, echter, de AEM auteurs kennen de kleuren als **groen** en **geel**, in plaats van de ontwerptaal van primair en secundair.

Het systeem van de Stijl van de AEM kan deze het kleurenStijlen van de Vertoning blootstellen gebruikend auteursvriendelijke etiketten **Groen** en **Geel**, terwijl de CSS-ontwikkelaars semantische naamgeving van `.cmp-component--primary-color` en `.cmp-component--secondary-color` om de daadwerkelijke stijlimplementatie in CSS te bepalen.

De stijlnaam van **Groen** is toegewezen aan `.cmp-component--primary-color`, en **Geel** tot `.cmp-component--secondary-color`.

Als de merkkleur van het bedrijf in de toekomst verandert, is het enige dat moet worden veranderd de enige implementatie van `.cmp-component--primary-color` en `.cmp-component--secondary-color`en de stijlnamen.

## De component Teaser als voorbeeld gebruikt case {#the-teaser-component-as-an-example-use-case}

Hieronder ziet u een voorbeeld van het gebruik van de stijl van een Taser-component voor verschillende lay-out- en weergavestijlen.

Hiermee wordt uitgelegd hoe stijlnamen (beschikbaar voor auteurs) en hoe de ondersteunende CSS-klassen worden ingedeeld.

### Configuratie van componentstijlen voor teaser {#component-styles-configuration}

In de volgende afbeelding wordt de [!UICONTROL Styles] configuratie voor de component Teaser voor de variaties die in het gebruiksgeval worden besproken.

De [!UICONTROL Style Group] namen, lay-out en weergave komen per geval overeen met de algemene concepten Weergavestijlen en Lay-outstijlen die worden gebruikt om typen stijlen in dit artikel conceptueel te categoriseren.

De [!UICONTROL Style Group] en het aantal [!UICONTROL Style Groups] moet worden aangepast aan de gebruiksscenario&#39;s van de component en aan de stijlconventies van de projectspecifieke component.

Bijvoorbeeld de **Weergave** naam stijlgroep had kunnen worden genoemd **Kleuren**.

![Stijlgroep weergeven](assets/style-config.png)

### Menu Stijlselectie {#style-selection-menu}

In de onderstaande afbeelding wordt de [!UICONTROL Style] gebruiken om de juiste stijlen voor de component te selecteren. Noteer de [!UICONTROL Style Grpi] namen en de stijlnamen worden allemaal weergegeven aan de auteur.

![Vervolgkeuzelijst Stijl](assets/style-menu.png)

### Standaardstijl {#default-style}

De standaardstijl is vaak de meest gebruikte stijl van de component en de standaard, niet-opgemaakte weergave van het gummetje wanneer deze aan een pagina wordt toegevoegd.

Afhankelijk van de standaardinstelling kan de CSS-code rechtstreeks worden toegepast op de `.cmp-teaser` (zonder modifiers) of op een `.cmp-teaser--default`.

Als de standaardstijlregels vaker dan niet op alle variaties van toepassing zijn, is het beter om te gebruiken `.cmp-teaser` als de CSS-klassen van de standaardstijl, aangezien alle variaties deze impliciet moeten overnemen, ervan uitgaande dat BEM-achtige conventies worden gevolgd. Als niet, zouden zij via de standaardbepaling, zoals moeten worden toegepast `.cmp-teaser--default`, die op zijn beurt moet worden toegevoegd aan de [De standaard CSS-klassen van de stijlconfiguratie van de component](#component-styles-configuration) veld, anders moeten deze stijlregels in elke variatie worden overschreven.

Het is zelfs mogelijk om een stijl &quot;benoemd&quot; toe te wijzen als de standaardstijl, bijvoorbeeld de hoofdstijl `(.cmp-teaser--hero)` wordt hieronder gedefinieerd, maar het is duidelijker om de standaardstijl toe te passen tegen de `.cmp-teaser` of `.cmp-teaser--default` Implementaties van CSS-klassen.

>[!NOTE]
>
>De standaardlay-outstijl heeft GEEN weergavestijlnaam, maar de auteur kan een weergaveoptie selecteren in het selectiegereedschap AEM Stijl.
>
>Dit is in strijd met de beste praktijken:
>
>**Alleen stijlcombinaties met een effect beschikbaar maken**
>
>Als een auteur de weergavestijl van **Groen** er zal niets gebeuren .
>
>In dit geval geven we deze schending toe, aangezien alle andere layoutstijlen kleurbaar moeten zijn met de merkkleuren.
>
>In de **Aanbieding (rechts uitgelijnd)** in het onderstaande gedeelte ziet u hoe u ongewenste stijlcombinaties kunt voorkomen.

![standaardstijl](assets/default.png)

* **Lay-outstijl**
   * Standaard
* **Weergavestijl**
   * Geen
* **Effectieve CSS-klassen**: `.cmp-teaser--promo` of `.cmp-teaser--default`

### Promo-stijl {#promo-style}

De **Stijl van Promo-indeling** wordt gebruikt voor het promoten van hoogwaardige inhoud op de site. De lay-out wordt horizontaal aangepast om een spatieband op de webpagina in te nemen en moet stijlbaar zijn volgens de kleur van een merk. De standaard Promo-lay-outstijl gebruikt zwarte tekst.

Om dit te bereiken **indelingsstijl** van **Aanbieding** en de **weergavestijlen** van **Groen** en **Geel** worden gevormd in het Systeem van de Stijl van de AEM voor de component van het Teaser.

#### Standaardwaarde voor aanbieding

![standaard promo](assets/promo-default.png)

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

De **Aanbieding rechts uitgelijnd** layoutstijl is een variant van de Promo-stijl waarmee de locatie van de afbeelding en tekst (afbeelding aan de rechterkant, tekst aan de linkerkant) wordt gespiegeld.

De juiste uitlijning, in de kern, is een weergavestijl die u in het AEM Stijlsysteem kunt invoeren als een weergavestijl die u samen met de stijl van de Promo-lay-out selecteert. Dit is in strijd met de beste praktijken van:

**Alleen stijlcombinaties met een effect beschikbaar maken**

..die reeds is geschonden in de [Standaardstijl](#default-style).

Omdat de juiste uitlijning alleen van invloed is op de stijl van de Promo-lay-out en niet op de andere twee lay-outstijlen: standaard en held, kunnen we een nieuwe lay-outstijlpromotie (rechts uitgelijnd) maken die de CSS-klasse bevat die de inhoud van de Promo-lay-outstijlen rechts uitlijnt: `cmp -teaser--alternate`.

Deze combinatie van meerdere stijlen tot één Stijl-item kan ook het aantal beschikbare stijlen en stijlpermutaties verminderen. Dit is het beste om deze te minimaliseren.

Let op de naam van de CSS-klasse. `cmp-teaser--alternate`, hoeft niet overeen te komen met de auteurvriendelijke nomenclatuur van &quot;rechts uitgelijnd&quot;.

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

CSS voor het kleuren van de achtergrond **Bevorderen** layout met de primaire (groene) kleur:

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS voor het kleuren van de tekst van **Hero** layout met de primaire (groene) kleur:

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
