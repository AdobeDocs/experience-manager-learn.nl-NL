---
title: Eenvoudige gids voor implementatie van zoekopdrachten
description: De Eenvoudige onderzoeksimplementatie is de materialen van het het laboratorium van de Top van AEM van 2017 Gedemystificeerde Onderzoek. Deze pagina bevat de materialen van dit laboratorium. Voor een geleide rondleiding van het laboratorium, te bekijken gelieve het werkboek van het Laboratorium in de sectie van de Presentatie van deze pagina.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 138
source-git-commit: 1048beba42011eccb1ebdd43458591c8e953fb8a
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 0%

---

# Eenvoudige gids voor implementatie van zoekopdrachten{#simple-search-implementation-guide}

De Eenvoudige onderzoeksimplementatie is de materialen van het **gedemystificeerde het laboratoriumAEM van Adobe Summit Onderzoek**. Deze pagina bevat de materialen van dit laboratorium. Voor een geleide rondleiding van het laboratorium, te bekijken gelieve het werkboek van het Laboratorium in de sectie van de Presentatie van deze pagina.

![ Overzicht van de Architectuur van het Onderzoek ](assets/l4080/simple-search-application.png)

## Presentatiemateriaal {#bookmarks}

* [Lab-werkboek](assets/l4080/l4080-lab-workbook.pdf)
* [Presentatie](assets/l4080/l4080-presentation.pdf)

## Bladwijzers {#bookmarks-1}

### Gereedschappen {#tools}

* [ Manager van de Index ](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [ verklaart Vraag ](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [ CRXDE Lite ](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak :index/cqPageLucene
* [ de Manager van het Pakket van CRX ](http://localhost:4502/crx/packmgr/index.jsp)
* [ Foutopsporing QueryBuilder ](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [ Generator van de Definitie van de Index van Oak ](https://thomasmueller.github.io/oakTools/indexDefGenerator.html)

### Hoofdstukken {#chapters}

*de verbindingen van het Hoofdstuk hieronder veronderstellen de [ Aanvankelijke Pakketten ](#initialpackages) op de Auteur van AEM bij`http://localhost:4502`* geïnstalleerd zijn

* [ Hoofdstuk 1 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [ Hoofdstuk 2 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [ Hoofdstuk 3 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [ Hoofdstuk 4 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [ Hoofdstuk 5 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [ Hoofdstuk 6 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [ Hoofdstuk 7 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [ Hoofdstuk 8 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [ Hoofdstuk 9 ](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

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

* [ Github bewaarplaats ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [ Sling Models ](https://sling.apache.org/documentation/bundles/models.html)
* [ het Verdelen ModelExporter ](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [ QueryBuilder API ](https://experienceleague.adobe.com/docs/)
* [ AEM Chrome Plug-in ](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([ pagina van de Documentatie ](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## Correcties en follow-up {#corrections-and-follow-up}

Correcties en verduidelijkingen uit de laboratoriumdiscussies en antwoorden op vervolgvragen van deelnemers.

1. **hoe te ophouden opnieuw indexerend?**

   Het opnieuw indexeren kan via IndexStats MBean beschikbaar via [ de Console van het Web van AEM > JMX ](http://localhost:4502/system/console/jmx) worden tegengehouden

   * [ http://localhost:4502 /system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats ](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * Voer `abortAndPause()` uit om het opnieuw indexeren af te breken. Hierdoor wordt de index vergrendeld om opnieuw te indexeren totdat `resume()` wordt aangeroepen.
      * Als u `resume()` uitvoert, wordt het indexeringsproces opnieuw gestart.
   * Documentatie: [ https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **hoe kan de indexen veelvoudige huurders steunen?**

   Oak ondersteunt het plaatsen van indexen door de inhoudsstructuur en deze indexen indexeren alleen binnen die substructuur. **`/content/site-a/oak:index/cqPageLucene`** kan bijvoorbeeld alleen onder **`/content/site-a`worden gemaakt om inhoud te indexeren.**

   Een equivalente aanpak is het gebruik van de eigenschappen **`includePaths`** en **`queryPaths`** op een index onder **`/oak:index`** . Bijvoorbeeld:

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   De overwegingen met deze benadering zijn:

   * De vragen MOETEN een wegbeperking specificeren die aan het werkingsgebied van de de vraagweg van de index gelijk is, of daar een nakomeling van zijn.
   * Met bredere bereikindexen (bijvoorbeeld `/oak:index/cqPageLucene`) worden de gegevens ook geïndexeerd, wat leidt tot dubbele invoer en kosten voor schijfgebruik.
   * Mogelijk is dubbel configuratiebeheer vereist (bijvoorbeeld het toevoegen van zelfde indexRules over veelvoudige huurdersindexen als zij de zelfde vraagreeksen moeten voldoen)
   * Deze benadering wordt het best gediend op de AEM Publish rij voor het onderzoek van de douanesite, aangezien op de Auteur van AEM, het voor vragen gemeenschappelijk is om bij hoog de inhoudsboom voor verschillende huurders (bijvoorbeeld, via OmniSearch) te worden uitgevoerd - de verschillende indexdefinities kunnen in verschillend gedrag resulteren die slechts op de wegbeperking wordt gebaseerd.

3. **waar is een lijst van alle beschikbare Analysatoren?**

   Oak stelt een reeks lucene-verstrekt de configuratieelementen van de analysator voor gebruik in AEM bloot.

   * [ Apache Oak Analyzers documentatie ](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [ Tokenizers ](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [ Filters ](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [ CharFilters ](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **hoe te om naar Pagina&#39;s en Assets in de zelfde vraag te zoeken?**

   Nieuw in AEM 6.3 is de capaciteit om voor veelvoudige knoop-types in de zelfde verstrekte vraag te vragen. De volgende query QueryBuilder. Elke subquery kan worden omgezet in een eigen index. In dit voorbeeld wordt de subquery `cq:Page` omgezet in `/oak:index/cqPageLucene` en de subquery `dam:Asset` wordt omgezet in `/oak:index/damAssetLucene` .

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

   Onderzoek de vraag en de resultaten via [ Debugger QueryBuilder ](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) en [ AEM Chrome Plug-in ](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).

5. **hoe te over veelvoudige wegen in de zelfde vraag te zoeken?**

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

   Onderzoek de vraag en de resultaten via [ Debugger QueryBuilder ](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) en [ AEM Chrome Plug-in ](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
