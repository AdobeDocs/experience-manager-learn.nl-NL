---
title: Inhoudsfragmenten en ervaringsfragmenten
description: Leer de gelijkenissen en de verschillen tussen de Fragmenten van de Inhoud en de Fragmenten van de Ervaring, en wanneer en hoe te om elk type te gebruiken.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
duration: 204
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 0%

---

# Inhoudsfragmenten en ervaringsfragmenten

Adobe Experience Manager Content Fragments and Experience Fragments kunnen er ongeveer hetzelfde uitzien op het oppervlak, maar elk speelt een sleutelrol in verschillende gebruiksgevallen. Leer hoe de Fragmenten van de Inhoud en de Fragmenten van de Ervaring gelijkaardig zijn, verschillend, en wanneer en hoe te om elk te gebruiken.

## Vergelijking

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Inhoudsfragmenten (CF)</strong></td>
<td><strong>Geniet van fragmenten (XF)</strong></td>
</tr><tr><td><strong>Definitie</strong></td>
<td><ul>
<li>Herbruikbaar, presentatie-agnostisch <strong>content</strong>, bestaande uit gestructureerde gegevenselementen (tekst, datums, verwijzingen, enz.)</li>
</ul>
</td>
<td><ul>
<li>Een herbruikbare samenstelling van een of meer AEM Componenten die inhoud en presentatie definiëren die een <strong>ervaring</strong> dat op zichzelf zinvol is</li>
</ul>
</td>
</tr><tr><td><strong>Core Tenets</strong></td>
<td><ul>
<li>Inhoud-centrisch</li>
<li>Gedefinieerd door a <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">gestructureerd, op vorm-gebaseerd, gegevensmodel.</a></li>
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
<li>Gedefinieerd door a <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Inhoudsfragmentmodel</a></li>
</ul>
</td>
<td><ul>
<li>Geïmplementeerd als een <strong>cq:pagina</strong></li>
<li>Door bewerkbare sjablonen gedefinieerd</li>
<li>Native HTML-uitvoering</li>
</ul>
</td>
</tr><tr><td><strong>Variaties</strong></td>
<td><ul>
<li>De mastervariant is de canonieke variant</li>
<li>Variaties zijn specifiek voor het gebruik en kunnen worden uitgelijnd op kanalen.</li>
</ul>
</td>
<td><ul>
<li>Variaties zijn specifiek voor kanalen of context</li>
<li>Variaties worden gesynchroniseerd gehouden via AEM Live Copy</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Bouwstenen</a> hergebruik van inhoud door verschillende variaties toestaan</li>
</ul>
</td>
</tr><tr><td><strong>Functies</strong></td>
<td><ul>
<li>Variaties</li>
<li>Versies</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Synchronisatie</a> inhoud van verschillende variaties</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Visuele diff</a> van versies van inhoudsfragmenten</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">Annotaties</a> van tekstelementen met meerdere regels</li>
<li>Intelligent <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">samenvatting</a> van tekstelementen met meerdere regels.</li>
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
<li>Complex gegevensmodel via verwijzingen naar inhoudsfragmenten</li>
<li>Voorvertoning in app</li>
</ul>
</td>
</tr><tr><td><strong>Gebruiken</strong></td>
<td><ul>
<li>JSON exporteren via <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html">GraphQL-API's zonder koppen AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM component Inhoud van kerncomponenten fragmenteren</a> voor gebruik in AEM Sites, AEM Screens of in Experience Fragments.</li>
<li>JSON exporteren via <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> voor verbruik door derden</li>
<li>JSON-export naar Adobe Target voor gerichte aanbiedingen</li>
<li>JSON via AEM HTTP Assets-API's voor gebruik door derden</li>
</ul>
</td>
<td><ul>
<li>AEM de component van het Fragment van de Ervaring voor gebruik in AEM Sites, AEM Screens of andere Fragmenten van de Ervaring.</li>
<li>Exporteren als <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Gewone HTML</a> voor gebruik door systemen van derden</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">HTML exporteren naar Adobe Target</a> voor gerichte aanbiedingen</li>
<li>JSON-export naar Adobe Target voor gerichte aanbiedingen</li>
</ul>
</td>
</tr><tr><td><strong>Vaak voorkomende gevallen</strong></td>
<td><ul>
<li>Maken van eindeloze gebruikskassen boven GraphQL</li>
<li>Gestructureerde gegevensinvoer/op formulieren gebaseerde inhoud</li>
<li>Redactionele inhoud met lange vorm (element met meerdere regels)</li>
<li>Inhoud die buiten de levenscyclus van de kanalen die het leveren wordt beheerd</li>
</ul>
</td>
<td><ul>
<li>Gecentraliseerd beheer van meerkanaals promotiemateriaal met behulp van variaties per kanaal.</li>
<li>Inhoud opnieuw gebruikt op meerdere pagina's in een website.</li>
<li>Website-chroom (bijvoorbeeld kop- en voettekst)</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Documentatie van de Adobe over ervaringsfragmenten</a></li>
</ul>
</td>
</tr></tbody></table>

## Architectuur van inhoudsfragmenten

Het volgende diagram illustreert de algemene architectuur voor AEM Content Fragments

![Architectuur van inhoudsfragmenten](./assets/content-fragments-architecture.png)

+ **Modellen van inhoudsfragmenten** Definieer de elementen (of velden) die definiëren welke inhoud het inhoudsfragment kan vastleggen en beschikbaar maken.
+ De **Inhoudsfragment** is een instantie van een Content Fragment Model dat een logische inhoudentiteit vertegenwoordigt.
+ Inhoudsfragment **variaties** Houd u aan het inhoudsfragmentmodel, maar u kunt de inhoud op verschillende manieren aanpassen.
+ Inhoudsfragmenten kunnen worden belicht/verbruikt door:
   + Inhoudsfragmenten gebruiken op **AEM Sites** (of AEM Screens) via de inhoudsfragmentcomponent van de AEM WCM Core Components.
   + Verbruik **Inhoudsfragment** van toepassingen zonder kop met behulp van AEM GraphQL-API&#39;s zonder koppen.
   + Inhoud van inhoudfragmentvariaties beschikbaar maken als JSON via **AEM Content Services** en API-pagina&#39;s voor alleen-lezen gebruik.
   + Inhoud van inhoudsfragment (alle variaties) rechtstreeks toegankelijk maken als JSON via directe aanroepen naar AEM Assets via het dialoogvenster **AEM ASSETS HTTP API** voor CRUD-gebruik.

## Architectuur van ervaringsfragmenten

![Architectuur van ervaringsfragmenten](./assets/experience-fragments-architecture.png)

+ **Bewerkbare sjablonen**, die op hun beurt worden gedefinieerd door **Bewerkbare sjabloontypen** en **Implementatie van AEM** definieert u de toegestane AEM componenten die kunnen worden gebruikt om een ervaringsfragment samen te stellen.
+ De **Ervaar fragment** is een instantie van een bewerkbare sjabloon die een logische ervaring vertegenwoordigt.
+ Ervaar fragment **variaties** Houd u aan de Bewerkbare sjabloon, maar u hebt verschillende ervaringen (inhoud en ontwerp).
+ De Fragmenten van de ervaring kunnen worden blootgesteld/worden verbruikt door:
   + Het gebruiken van de Fragmenten van de Ervaring op AEM Sites (of AEM Screens) via de AEM component van het Fragment van de Ervaring.
   + De inhoud van een Experience Fragment wordt als JSON (met ingesloten HTML) beschikbaar via **AEM Content Services** en API-pagina&#39;s
   + Een Experience Fragment rechtstreeks toegankelijk maken als **&quot;Gewone HTML&quot;**.
   + Erviteitsfragmenten exporteren naar **Adobe Target** zoals HTML of JSON aanbiedt.
   + AEM Sites biedt native ondersteuning voor HTML-aanbiedingen, maar JSON-aanbiedingen vereisen aangepaste ontwikkeling.

## De bron voor inhoudsfragmenten ondersteunen

+ [Gebruikershandleiding voor inhoudsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Inleiding tot Adobe Experience Manager als een headless CMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html)
+ [Inhoudsfragmenten in AEM gebruiken](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM inhoudfragmentcomponent van WCM Core-componenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Inhoudsfragmenten en AEM zonder kop gebruiken](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [Aan de slag met AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Bronnen voor ervaringsfragmenten ondersteunen

+ [Documentatie van de Adobe over ervaringsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Werken met AEM ervaringsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Fragmenten voor AEM ervaring gebruiken](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Werken met AEM Experience Fragments met Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
