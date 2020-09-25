---
title: Inhoudsfragmenten en ervaringsfragmenten begrijpen
description: Adobe Experience Manager Content Fragments and Experience Fragments kunnen er ongeveer hetzelfde uitzien op het oppervlak, maar elk speelt een sleutelrol in verschillende gebruiksgevallen. Leer hoe de Fragmenten van de Inhoud en de Fragmenten van de Ervaring gelijkaardig zijn, verschillend, en wanneer en hoe te om elk te gebruiken.
sub-product: activa, sites, inhoudsdiensten
feature: content fragments, experience fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 0%

---


# Inhoudsfragmenten en ervaringsfragmenten begrijpen

Adobe Experience Manager Content Fragments and Experience Fragments kunnen er ongeveer hetzelfde uitzien op het oppervlak, maar elk speelt een sleutelrol in verschillende gebruiksgevallen. Leer hoe de Fragmenten van de Inhoud en de Fragmenten van de Ervaring gelijkaardig zijn, verschillend, en wanneer en hoe te om elk te gebruiken.

## Vergelijking van inhoudsfragmenten en ervaringsfragmenten

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Inhoudsfragmenten (CF)</strong></td>
<td><strong>Geniet van fragmenten (XF)</strong></td>
</tr><tr><td><strong>Definitie</strong></td>
<td><ul>
<li>Herbruikbare, presentatieagnostische <strong>inhoud</strong>, samengesteld uit gestructureerde gegevenselementen (tekst, datums, verwijzingen, enz.)</li>
</ul>
</td>
<td><ul>
<li>Een herbruikbare samenstelling van een of meer AEM Componenten die inhoud en presentatie definiëren en een <strong>ervaring</strong> vormen die op zichzelf zinnig is</li>
</ul>
</td>
</tr><tr><td><strong>Core Tenders</strong></td>
<td><ul>
<li>Inhoud-centrisch</li>
<li>Gedefinieerd door een <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">gestructureerd, op formulieren gebaseerd gegevensmodel.</a></li>
<li>Ontwerp en layout agnostisch.</li>
<li>Het kanaal is eigenaar van de presentatie van de inhoud van het inhoudsfragment (layout en ontwerp)</li>
</ul>
</td>
<td><ul>
<li>Op presentatie gericht</li>
<li>Gedefinieerd door ongestructureerde samenstelling van AEM componenten</li>
<li>Definieert ontwerp en lay-out van inhoud</li>
<li>Gebruikt "as is" in kanalen</li>
</ul>
</td>
</tr><tr><td><strong>Technische details</strong></td>
<td><ul>
<li>Geïmplementeerd als <strong>dam:Asset</strong></li>
<li>Gedefinieerd door een <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">inhoudsfragmentmodel</a></li>
</ul>
</td>
<td><ul>
<li>Geïmplementeerd als een <strong>cq:pagina</strong></li>
<li>Gedefinieerd door bewerkbare sjablonen</li>
<li>Systeemeigen HTML-uitvoering</li>
</ul>
</td>
</tr><tr><td><strong>Variaties</strong></td>
<td><ul>
<li>De Master variatie is de canonieke variatie</li>
<li>Variaties zijn specifiek voor het gebruik en kunnen worden uitgelijnd op kanalen.</li>
</ul>
</td>
<td><ul>
<li>Variaties zijn specifiek voor het kanaal of de context</li>
<li>Variaties worden gesynchroniseerd gehouden via AEM Live Copy</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Bouwstenen</a> maken hergebruik van inhoud door verschillende variaties mogelijk</li>
</ul>
</td>
</tr><tr><td><strong>Functies</strong></td>
<td><ul>
<li>Variaties</li>
<li>Versies</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank">Synchronisatie</a> van inhoud tussen variaties</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">Visuele scheiding</a> van de versies van het Fragment van de Inhoud</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank">Annotaties</a> van tekstelementen met meerdere regels</li>
<li>Intelligente <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">samenvatting</a> van tekstelementen met meerdere regels.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">Vertaling/lokalisatie</a></li>
</ul>
</td>
<td><ul>
<li>Variaties</li>
<li>Variaties als actieve kopieën</li>
<li>Versies</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Bouwstenen</a></li>
<li>Annotaties</li>
<li>Responsieve lay-out en voorvertoning</li>
<li>Vertaling/lokalisatie</li>
</ul>
</td>
</tr><tr><td><strong>Gebruiken</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM de component</a> van het Fragment van de Inhoud van Componenten van de Kern voor gebruik in AEM Sites, AEM Screens of in de Fragmenten van de Ervaring.</li>
<li>JSON exporteren via <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a> voor gebruik door derden</li>
<li>JSON via AEM HTTP Assets API's voor gebruik door derden.</li>
</ul>
</td>
<td><ul>
<li>AEM de component van het Fragment van de Ervaring voor gebruik in AEM Sites, AEM Screens of andere Fragmenten van de Ervaring.</li>
<li>Exporteren als gewone HTML <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank"></a> voor gebruik door systemen van derden</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">HTML exporteren naar Adobe Target</a> voor gerichte aanbiedingen</li>
<li>JSON-export naar Adobe Target voor gerichte aanbiedingen</li>
</ul>
</td>
</tr><tr><td><strong>Vaak voorkomende gevallen</strong></td>
<td><ul>
<li>Zeer gestructureerde gegevensinvoer/op formulieren gebaseerde inhoud</li>
<li>Redactionele inhoud met lange vorm (element met meerdere regels)</li>
<li>Inhoud die buiten de levenscyclus van de kanalen die het leveren wordt beheerd</li>
</ul>
</td>
<td><ul>
<li>Gecentraliseerd beheer van meerkanaals promotiemateriaal met behulp van variaties per kanaal.</li>
<li>Inhoud opnieuw gebruikt op meerdere pagina's in een website.</li>
<li>Website-chroom (bijv. kop- en voettekst)</li>
<li>Een ervaring die buiten de levenscyclus van de kanalen die het leveren wordt beheerd</li>
</ul>
</td>
</tr><tr><td><strong>Documentatie</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Gebruikershandleiding voor AEM inhoudsfragmenten</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">Inhoudsfragmenten in AEM gebruiken</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Adobe documentatie over Experience Fragments</a></li>
</ul>
</td>
</tr></tbody></table>

## Architectuur van inhoudsfragmenten

Het volgende diagram illustreert de algemene architectuur voor AEM Inhoudsfragmenten

!![Architectuur van inhoudsfragmenten](./assets/content-fragments-architecture.png)

+ **Met Inhoudsfragmentmodellen** worden de elementen (of velden) gedefinieerd die definiëren welke inhoud het inhoudsfragment kan vastleggen en beschikbaar maken.
+ Het **inhoudsfragment** is een instantie van een inhoudsfragmentmodel dat een logische inhoudsentiteit vertegenwoordigt.
+ De **variaties** van het inhoudsfragment passen echter bij het model van het inhoudsfragment.
+ Inhoudsfragmenten kunnen worden belicht/verbruikt door:
   + Inhoudsfragmenten op **AEM Sites** (of AEM Screens) gebruiken via de inhoudsfragmentcomponent van AEM WCM Core-componenten.
   + Insluiten van een inhoudsfragment in een **Ervingsfragment** via de inhoudsfragmentcomponent van AEM WCM Core-componenten, voor gebruik in alle gevallen waarin u Fragment gebruikt.
   + Inhoud in fragmenten verdelen varieert als JSON via **AEM Content Services** en API-pagina&#39;s voor gebruik met de eigenschap Alleen-lezen.
   + Inhoud van inhoudsfragment (alle variaties) rechtstreeks beschikbaar maken als JSON via directe aanroepen naar AEM Assets via de HTTP-API **van** AEM Assets voor gebruik met CRUD.

## Ervaar de architectuur van Fragmenten

!![Ervaar de architectuur van Fragmenten](./assets/experience-fragments-architecture.png)

+ **Bewerkbare sjablonen**, die op hun beurt worden gedefinieerd door **Bewerkbare sjabloontypen** en een implementatie **van de component** AEM Pagina, definiëren de toegestane AEM componenten die kunnen worden gebruikt om een ervaringsfragment samen te stellen.
+ Het fragment **van de** Ervaring is een geval van een Bewerkbare Malplaatje dat een logische ervaring vertegenwoordigt.
+ De **variaties** van het Fragment van de ervaring houden zich aan het Bewerkbare Malplaatje aan, echter, hebben variaties in ervaring (inhoud en ontwerp).
+ De Fragmenten van de ervaring kunnen worden blootgesteld/worden verbruikt door:
   + Het gebruiken van de Fragmenten van de Ervaring op AEM Sites (of AEM Screens) via de AEM component van het Fragment van de Ervaring.
   + Bij het beschikbaar maken van een Experience Fragment wordt de inhoud gewijzigd als JSON (met ingesloten HTML) via **AEM Content Services** en API Pages.
   + Een Experience Fragment rechtstreeks beschikbaar maken als **&quot;Plain HTML&quot;**.
   + Exporteren van Experience Fragments naar **Adobe Target** als HTML- of JSON-aanbiedingen.
   + AEM Sites biedt standaard ondersteuning voor HTML-aanbiedingen, maar JSON-aanbiedingen vereisen aangepaste ontwikkeling.

## Materialen voor inhoudsfragmenten ondersteunen

+ [Gebruikershandleiding voor inhoudsfragmenten](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Inhoudsfragmenten in AEM gebruiken](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM inhoudfragmentcomponent van WCM Core-componenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Inhoudsfragmenten en AEM Content Services gebruiken](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [Aan de slag met AEM Content Services](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## Ondersteunende materialen voor ervaringsfragmenten

+ [Adobe documentatie over Experience Fragments](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [Werken met AEM ervaringsfragmenten](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [Fragmenten voor AEM ervaring gebruiken](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Werken met AEM Experience Fragments met Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
