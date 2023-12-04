---
title: Eenvoudige gids voor implementatie van zoekopdrachten
description: De Eenvoudige onderzoeksimplementatie is de materialen van het laboratorium van de Top van 2017 AEM Gedetailleerd Onderzoek. Deze pagina bevat de materialen van dit laboratorium. Voor een geleide rondleiding van het laboratorium, te bekijken gelieve het werkboek van het Laboratorium in de sectie van de Presentatie van deze pagina.
version: 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 202
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 0%

---

# Eenvoudige gids voor implementatie van zoekopdrachten{#simple-search-implementation-guide}

De eenvoudige onderzoeksimplementatie is de materialen van **Adobe Summit lab AEM zoekopdracht gedemystificeerd**. Deze pagina bevat de materialen van dit laboratorium. Voor een geleide rondleiding van het laboratorium, te bekijken gelieve het werkboek van het Laboratorium in de sectie van de Presentatie van deze pagina.

![Overzicht van zoekarchitectuur](assets/l4080/simple-search-application.png)

## Presentatiemateriaal {#bookmarks}

* [Lab-werkboek](assets/l4080/l4080-lab-workbook.pdf)
* [Presentatie](assets/l4080/l4080-presentation.pdf)

## Bladwijzers {#bookmarks-1}

### Gereedschappen {#tools}

* [Indexbeheer](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [Query uitvoeren](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [CRX Package Manager](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder-foutopsporing](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Generator voor eekindexdefinitie](https://oakutils.appspot.com/generate/index)

### Hoofdstukken {#chapters}

*De onderstaande hoofdstukkoppelingen gaan uit van de [Eerste pakketten](#initialpackages) zijn geïnstalleerd op AEM auteur op`http://localhost:4502`*

* [Hoofdstuk 1](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [Hoofdstuk 2](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [Hoofdstuk 3](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [Hoofdstuk 4](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [Hoofdstuk 5](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [Hoofdstuk 6](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [Hoofdstuk 7](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [Hoofdstuk 8](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [Hoofdstuk 9](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## Pakketten {#packages}

### Eerste pakketten {#initial-packages}

* [Tags](assets/l4080/summit-tags.zip)
* [Eenvoudig pakket met zoektoepassingen](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### Hoofdstukpakketten {#chapter-packages}

* [Hoofdstuk 1-oplossing](assets/l4080/l4080-chapter1.zip)
* [Hoofdstuk 2-oplossing](assets/l4080/l4080-chapter2.zip)
* [Hoofdstuk 3-oplossing](assets/l4080/l4080-chapter3.zip)
* [Hoofdstuk 4-oplossing](assets/l4080/l4080-chapter4.zip)
* [Hoofdstuk 5 instellen](assets/l4080/l4080-chapter5-setup.zip)
* [Hoofdstuk 5-oplossing](assets/l4080/l4080-chapter5-solution.zip)
* [Hoofdstuk 6-oplossing](assets/l4080/l4080-chapter6.zip)
* [Hoofdstuk 9-oplossing](assets/l4080/l4080-chapter9.zip)

## Materialen waarnaar wordt verwezen {#reference-materials}

* [Github-opslagplaats](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Verkoopmodellen](https://sling.apache.org/documentation/bundles/models.html)
* [Verkoopmodel exporteren](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder-API](https://experienceleague.adobe.com/docs/)
* [AEM Chrome-plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([Documentatiepagina](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Correcties en follow-up {#corrections-and-follow-up}

Correcties en verduidelijkingen uit de laboratoriumdiscussies en antwoorden op vervolgvragen van deelnemers.

1. **Hoe kan ik stoppen met opnieuw indexeren?**

   Opnieuw indexeren kan worden gestopt via IndexStats MBean beschikbaar via [AEM webconsole > JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Uitvoeren `abortAndPause()` om het opnieuw indexeren af te breken. Hierdoor wordt de index vergrendeld en wordt de index opnieuw geïndexeerd tot `resume()` wordt aangeroepen.
      * Uitvoeren `resume()` wordt het indexeringsproces opnieuw gestart.
   * Documentatie: [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Hoe kunnen eak-indexen meerdere huurders ondersteunen?**

   Met de optie Eak kunt u indexen plaatsen door de inhoudsstructuur heen. Deze indexen indexeren alleen binnen die substructuur. Bijvoorbeeld **`/content/site-a/oak:index/cqPageLucene`** kan alleen onder **`/content/site-a`.**

   Een gelijkwaardige aanpak is het gebruik van de **`includePaths`** en **`queryPaths`** eigenschappen op een index onder **`/oak:index`**. Bijvoorbeeld:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   De overwegingen met deze benadering zijn:

   * De vragen MOETEN een wegbeperking specificeren die aan het werkingsgebied van de de vraagweg van de index gelijk is, of daar een nakomeling van zijn.
   * Bereikindexen van de bredere klasse (bijvoorbeeld `/oak:index/cqPageLucene`) zal de gegevens ook indexeren, wat leidt tot dubbele inname en kosten voor schijfgebruik.
   * Mogelijk is dubbel configuratiebeheer vereist (bijvoorbeeld het toevoegen van zelfde indexRules over veelvoudige huurdersindexen als zij de zelfde vraagreeksen moeten voldoen)
   * Deze benadering wordt het best gediend op de AEM Publish rij voor het onderzoek van de douanesite, aangezien op AEM Auteur, het voor vragen gemeenschappelijk is dat bij hoog de inhoudsboom voor verschillende huurders (bijvoorbeeld, via OmniSearch) worden uitgevoerd - de verschillende indexdefinities in verschillend gedrag kunnen resulteren dat slechts op de wegbeperking wordt gebaseerd.

3. **Waar is een lijst van alle beschikbare Analysatoren?**

   Oak stelt een reeks lucene-verstrekt de configuratieelementen van de analysator voor gebruik in AEM bloot.

   * [Documentatie Apache Oak-analyse](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [Filters](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [Tekstfilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **Hoe te om naar Pagina&#39;s en Middelen in de zelfde vraag te zoeken?**

   Nieuw in AEM 6.3 is de capaciteit om voor veelvoudige knoop-types in de zelfde verstrekte vraag te vragen. De volgende query QueryBuilder. Merk op dat elke &quot;sub-query&quot;aan zijn eigen index kan oplossen, zodat in dit voorbeeld, `cq:Page` subquery wordt omgezet in `/oak:index/cqPageLucene` en de `dam:Asset` subquery wordt omgezet in `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   resultaten in het volgende vraag en vraagplan:

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   De query en resultaten verkennen via [QueryBuilder-foutopsporing](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) en [AEM Chrome-plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **Hoe te over veelvoudige wegen in de zelfde vraag te zoeken?**

   Nieuw in AEM 6.3 is de capaciteit om over veelvoudige wegen in de zelfde verstrekte vraag te vragen. De volgende query QueryBuilder. Merk op dat elke &quot;sub-query&quot;tot zijn eigen index kan oplossen.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   resultaten in het volgende vraag en vraagplan

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   De query en resultaten verkennen via [QueryBuilder-foutopsporing](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) en [AEM Chrome-plug-in](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
