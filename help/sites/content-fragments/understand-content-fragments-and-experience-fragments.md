---
title: Inhoudsfragmenten en ervaringsfragmenten begrijpen
description: Adobe Experience Manager Content Fragments and Experience Fragments kunnen er ongeveer hetzelfde uitzien op het oppervlak, maar elk speelt een sleutelrol in verschillende gebruiksgevallen. Leer hoe de Fragmenten van de Inhoud en de Fragmenten van de Ervaring gelijkaardig zijn, verschillend, en wanneer en hoe te om elk te gebruiken.
sub-product: assets, sites, content services
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1037'
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
<li>Herbruikbaar, presentatie-agnostisch <strong>content</strong>, samengesteld uit gestructureerde gegevenselementen (tekst, datums, verwijzingen, enz.)</li>
</ul>
</td>
<td><ul>
<li>Een herbruikbare samenstelling van een of meer AEM Componenten die inhoud en presentatie definiëren die een <strong>ervaring</strong> vormen die op zichzelf zinnig is</li>
</ul>
</td>
</tr><tr><td><strong>Core Tenders</strong></td>
<td><ul>
<li>Inhoud-centrisch</li>
<li>Gedefinieerd door een <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">gestructureerd, op vorm gebaseerd gegevensmodel.</a></li>
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
<li>Geïmplementeerd als een <strong>dam:Asset</strong></li>
<li>Gedefinieerd door een <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Inhoudsfragmentmodel</a></li>
</ul>
</td>
<td><ul>
<li>Geïmplementeerd als een <strong>cq:Pagina</strong></li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Gebouwend </a> blocksallow inhoud hergebruik over variaties</li>
</ul>
</td>
</tr><tr><td><strong>Functies</strong></td>
<td><ul>
<li>Variaties</li>
<li>Versies</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Synchronisatie van inhoud </a> tussen variaties</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Visuele </a> verschillende versies van inhoudsfragmenten</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank"></a> Annotaties van tekstelementen met meerdere regels</li>
<li>Intelligente <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">samenvatting</a> van tekstelementen met meerdere regels.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">Vertaling/lokalisatie</a></li>
</ul>
</td>
<td><ul>
<li>Variaties</li>
<li>Variaties als actieve kopieën</li>
<li>Versies</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">Bouwstenen</a></li>
<li>Annotaties</li>
<li>Responsieve lay-out en voorvertoning</li>
<li>Vertaling/lokalisatie</li>
</ul>
</td>
</tr><tr><td><strong>Gebruiken</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM de </a> component van het Fragment van de Inhoud van Componenten van de Kern voor gebruik in AEM Sites, AEM Screens of in de Fragmenten van de Ervaring.</li>
<li>JSON exporteren via <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> voor consumptie door derden</li>
<li>JSON via AEM HTTP Assets API's voor gebruik door derden.</li>
</ul>
</td>
<td><ul>
<li>AEM de component van het Fragment van de Ervaring voor gebruik in AEM Sites, AEM Screens of andere Fragmenten van de Ervaring.</li>
<li>Exporteren als <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Onbewerkte HTML</a> voor gebruik door systemen van derden</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">HTML-export naar </a> doelaanbiedingen voor Adobe</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Gebruikershandleiding voor AEM inhoudsfragmenten</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">Inhoudsfragmenten in AEM gebruiken</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe documentatie over Experience Fragments</a></li>
</ul>
</td>
</tr></tbody></table>

## Architectuur van inhoudsfragmenten

Het volgende diagram illustreert de algemene architectuur voor AEM Inhoudsfragmenten

!![Architectuur van inhoudsfragmenten](./assets/content-fragments-architecture.png)

+ **Content Fragment** Models definiëren de elementen (of velden) die definiëren welke inhoud het Content Fragment kan vastleggen en beschikbaar maken.
+ Het **Inhoudsfragment** is een instantie van een Inhoudsfragmentmodel dat een logische inhoudsentiteit vertegenwoordigt.
+ Inhoudsfragment **variaties** houden zich aan het Inhoudsfragmentmodel, maar hebben variaties in inhoud.
+ Inhoudsfragmenten kunnen worden belicht/verbruikt door:
   + Inhoudsfragmenten gebruiken op **AEM Sites** (of AEM Screens) via de inhoudsfragmentcomponent van AEM WCM Core Components&#39;.
   + Insluiten van een inhoudsfragment in een **Ervaar fragment** via de inhoudsfragmentcomponent van AEM WCM Core-componenten, voor gebruik in elke Gebruikszaak van het Fragment van de Ervaring.
   + Door de inhoud van een inhoudsfragment toegankelijk te maken als JSON via **AEM Content Services** en API-pagina&#39;s voor alleen-lezen gebruik.
   + Inhoud van inhoudsfragment (alle variaties) rechtstreeks beschikbaar maken als JSON via directe aanroepen naar AEM Assets via de **AEM Assets HTTP API** voor CRUD-gebruiksgevallen.

## Ervaar de architectuur van Fragmenten

!![Ervaar de architectuur van Fragmenten](./assets/experience-fragments-architecture.png)

+ **Bewerkbare sjablonen**, die op hun beurt worden gedefinieerd door  **bewerkbare** sjabloontypen en een implementatie **van de component** AEM Pagina, definiëren de toegestane AEM componenten die kunnen worden gebruikt om een ervaringsfragment samen te stellen.
+ Het **Ervingsfragment** is een instantie van een bewerkbare sjabloon die een logische ervaring vertegenwoordigt.
+ Ervaar het Fragment **variaties** houden zich aan het Bewerkbare Malplaatje, echter, hebben variaties in ervaring (inhoud en ontwerp).
+ De Fragmenten van de ervaring kunnen worden blootgesteld/worden verbruikt door:
   + Het gebruiken van de Fragmenten van de Ervaring op AEM Sites (of AEM Screens) via de AEM component van het Fragment van de Ervaring.
   + Bij het beschikbaar maken van een Experience Fragment wordt de inhoud gewijzigd als JSON (met ingesloten HTML) via **AEM Content Services** en API Pages.
   + Direct beschikbaar maken van een variant van het Fragment van de Ervaring zoals **&quot;Onbewerkte HTML&quot;**.
   + Exporteren van Experience Fragments naar **Adobe Target** als HTML of JSON aanbiedingen.
   + AEM Sites biedt standaard ondersteuning voor HTML-aanbiedingen, maar JSON-aanbiedingen vereisen aangepaste ontwikkeling.

## Materialen voor inhoudsfragmenten ondersteunen

+ [Gebruikershandleiding voor inhoudsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Inhoudsfragmenten in AEM gebruiken](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM inhoudfragmentcomponent van WCM Core-componenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Inhoudsfragmenten en AEM zonder kop gebruiken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [Aan de slag met AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Ondersteunende materialen voor ervaringsfragmenten

+ [Adobe documentatie over Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Werken met AEM ervaringsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Fragmenten voor AEM ervaring gebruiken](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Werken met AEM Experience Fragments met Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
