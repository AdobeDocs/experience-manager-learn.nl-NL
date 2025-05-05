---
title: Inhoudsfragmenten en ervaringsfragmenten
description: Leer de gelijkenissen en de verschillen tussen de Fragmenten van de Inhoud en de Fragmenten van de Ervaring, en wanneer en hoe te om elk type te gebruiken.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
duration: 168
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
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
<li>Herbruikbaar, presentatie-agnostieke <strong> inhoud </strong>, die uit gestructureerde gegevenselementen (tekst, data, verwijzingen, enz.) wordt samengesteld</li>
</ul>
</td>
<td><ul>
<li>Een herbruikbare, samenstelling van één of meerdere Componenten die van AEM inhoud en presentatie bepalen die een <strong> ervaring </strong> vormen die op zijn eigen steek houdt</li>
</ul>
</td>
</tr><tr><td><strong>Core Tenets</strong></td>
<td><ul>
<li>Inhoud-centrisch</li>
<li>Gedefinieerd door a <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank"> gestructureerd, op vorm-gebaseerd, gegevensmodel.</a></li>
<li>Ontwerp en layout agnostisch.</li>
<li>Het kanaal is eigenaar van de presentatie van de inhoud van het inhoudsfragment (layout en ontwerp)</li>
</ul>
</td>
<td><ul>
<li>Op presentatie gericht</li>
<li>Gedefinieerd door ongestructureerde samenstelling van AEM Components</li>
<li>Definieert ontwerp en lay-out van inhoud</li>
<li>Gebruikt "as is" in kanalen</li>
</ul>
</td>
</tr><tr><td><strong>Technische details</strong></td>
<td><ul>
<li>Geïmplementeerd als a <strong> dam:Activa </strong></li>
<li>Gedefinieerd door het Model van het Fragment van de a <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank"> Inhoud </a></li>
</ul>
</td>
<td><ul>
<li>Geïmplementeerd als a <strong> cq:Pagina </strong></li>
<li>Door bewerkbare sjablonen gedefinieerd</li>
<li>Oorspronkelijke HTML-uitvoering</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank"> Bouwstenen </a> staan inhoud toe hergebruik over variaties</li>
</ul>
</td>
</tr><tr><td><strong>Functies</strong></td>
<td><ul>
<li>Variaties</li>
<li>Versies</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank"> Synchronisatie </a> van inhoud over variaties</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank"> Visuele diff </a> van de versies van het Fragment van de Inhoud</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank"> Annotaties </a> van multi-lijn tekstelementen</li>
<li>Intelligente <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank"> samenvatting </a> van multi-line tekstelementen.</li>
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
<li>JSON-export via <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html"> AEM Headless GraphQL API's </a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank"> de component van het Fragment van de Inhoud van de Componenten van de Kern van AEM de component </a> voor gebruik in AEM Sites, AEM Screens of in de Fragmenten van de Ervaring.</li>
<li>JSON de uitvoer via <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank"> de Diensten van de Inhoud van AEM </a> voor derdeconsumptie</li>
<li>JSON-export naar Adobe Target voor gerichte aanbiedingen</li>
<li>JSON via AEM HTTP Assets API's voor gebruik door derden</li>
</ul>
</td>
<td><ul>
<li>AEM Experience Fragment component voor gebruik in AEM Sites, AEM Screens of andere Experience Fragments.</li>
<li>De uitvoer als <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank"> Onbewerkte HTML </a> voor gebruik door derdesystemen</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank"> de uitvoer van HTML naar Adobe Target </a> voor gerichte aanbiedingen</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Gebruikershandleiding voor AEM Content Fragments</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">Inhoudsfragmenten in AEM gebruiken</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe-documentatie over Experience Fragments</a></li>
</ul>
</td>
</tr></tbody></table>

## Architectuur van inhoudsfragmenten

Het volgende diagram illustreert de algemene architectuur voor AEM Content Fragments

![&#128279;](./assets/content-fragments-architecture.png) architectuur van de Fragmenten van 0&rbrace; Inhoud

+ **Modellen van het Fragment van de Inhoud** bepalen de elementen (of gebieden) die bepalen welke inhoud het Fragment van de Inhoud kan vangen en blootstellen.
+ Het **Fragment van de Inhoud** is een geval van een Model van het Fragment van de Inhoud dat een logische inhoudsentiteit vertegenwoordigt.
+ De veranderingen van het Fragment van de inhoud **&#x200B;**&#x200B;houden zich aan het Model van het Fragment van de Inhoud, echter, hebben variaties in inhoud.
+ Inhoudsfragmenten kunnen worden belicht/verbruikt door:
   + Gebruikend de Fragmenten van de Inhoud op **AEM Sites** (of AEM Screens) via de component van het Fragment van de Inhoud van de Componenten van de Kern van AEM WCM.
   + Verbruik **het Fragment van de Inhoud** van headless apps gebruikend AEM Headless GraphQL APIs.
   + Het blootstellen van de inhoud van de Variaties van het Fragment van de Inhoud als JSON via **de Diensten van de Inhoud van AEM** en API Pagina&#39;s voor read-only gebruiksgevallen.
   + Direct blootstellend de inhoud van het Fragment van de Inhoud (alle variaties) als JSON via directe vraag aan AEM Assets via **AEM Assets HTTP API** voor CRUD gebruiksgevallen.

## Architectuur van ervaringsfragmenten

![ de architectuur van Fragmenten van de Ervaring ](./assets/experience-fragments-architecture.png)

+ **Bewerkbare Malplaatjes**, die beurtelings door **Bewerkbare Types van Malplaatje** en een **de componentenimplementatie van de Pagina van AEM** worden bepaald, bepalen de toegestane Componenten van AEM die kunnen worden gebruikt om een Fragment van de Ervaring samen te stellen.
+ Het **Fragment van de Ervaring** is een geval van een Bewerkbaar Malplaatje dat een logische ervaring vertegenwoordigt.
+ De veranderingen van het Fragment van de ervaring **&#x200B;**&#x200B;houden zich aan het Bewerkbare Malplaatje aan, echter, hebben variaties in ervaring (inhoud en ontwerp).
+ De Fragmenten van de ervaring kunnen worden blootgesteld/worden verbruikt door:
   + Het gebruiken van de Fragmenten van de Ervaring op AEM Sites (of AEM Screens) via de component van het Fragment van de Ervaring van AEM.
   + Het blootstellen van een de veranderingsinhoud van het Fragment van de Ervaring als JSON (met ingebedde HTML) via **de Diensten van de Inhoud van AEM** en API Pagina&#39;s.
   + Direct blootstellend een variatie van het Fragment van de Ervaring zoals **&quot;Onbewerkte HTML&quot;**.
   + Het uitvoeren van de Fragmenten van de Ervaring aan **Adobe Target** als of HTML of JSON aanbiedingen.
   + AEM Sites biedt native ondersteuning voor HTML-aanbiedingen, maar JSON-aanbiedingen vereisen aangepaste ontwikkeling.

## De bron voor inhoudsfragmenten ondersteunen

+ [ Gids van de Gebruiker van de Fragmenten van de Inhoud ](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [ Inleiding aan Adobe Experience Manager als Headless CMS ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html)
+ [ Gebruikend Inhoudsfragmenten in AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [ de component van het Fragment van de Inhoud van de Componenten van de Kern van AEM WCM &lbrace;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [ Gebruikend de Fragmenten van de Inhoud en Hoofdloze AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [ Begonnen het worden met de Diensten van de Inhoud van AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Bronnen voor ervaringsfragmenten ondersteunen

+ [ documentatie van Adobe over de Fragmenten van de Ervaring ](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [ Begrijpingsfragmenten van AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [ Gebruikend de Fragmenten van de Ervaring van AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [ Gebruikend de Fragmenten van de Ervaring van AEM met Adobe Target ](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
