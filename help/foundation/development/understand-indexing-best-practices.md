---
title: Indexeren van beste praktijken in AEM
description: Leer over het indexeren van beste praktijken in AEM.
version: 6.4, 6.5, Cloud Service
sub-product: Experience Manager, Experience Manager Sites
feature: Search
doc-type: Article
topic: Development
role: Developer, Architect
level: Beginner
duration: 0
last-substantial-update: 2024-01-04T00:00:00Z
jira: KT-14745
thumbnail: KT-14745.jpeg
source-git-commit: 7f69fc888a7b603ffefc70d89ea470146971067e
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 0%

---


# Indexeren van beste praktijken in AEM

Leer meer over het indexeren van beste praktijken in Adobe Experience Manager (AEM). Apache [Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/query/query.html) bevoegdheden het inhoudonderzoek in AEM en het volgende zijn zeer belangrijke punten:

- Uit de doos, verstrekt AEM diverse indexen om onderzoek en vraagfunctionaliteit, bijvoorbeeld te steunen `damAssetLucene`, `cqPageLucene` en meer.
- Alle indexdefinities worden opgeslagen in de dataopslag onder `/oak:index` knooppunt.
- AEM as a Cloud Service steunt slechts de indexen van Luik.
- De configuratie van de index zou in de AEM projectcodebase moeten worden beheerd en worden opgesteld gebruikend de pijpleidingen van de Manager van de Wolk CI/CD.
- Als er meerdere indexen beschikbaar zijn voor een bepaalde query, wordt de opdracht **index met de laagste geschatte kosten**.
- Als er geen index beschikbaar is voor een bepaalde query, wordt de inhoudsstructuur doorlopen om de overeenkomende inhoud te zoeken. De standaardlimiet via `org.apache.jackrabbit.oak.query.QueryEngineSettingsService` slechts 10.000 knooppunten doorlopen.
- De resultaten van een query zijn **eindelijk gefilterd** om ervoor te zorgen dat de huidige gebruiker leestoegang heeft. Dit betekent dat de vraagresultaten kleiner kunnen zijn dan het aantal geïndexeerde knopen.
- Het opnieuw indexeren van de repository na wijzigingen in de indexdefinitie vergt tijd en hangt af van de grootte van de repository.

Om een efficiënte en correcte onderzoeksfunctionaliteit te hebben die niet de prestaties van de AEM instantie beïnvloedt, is het belangrijk om de het indexeren beste praktijken te begrijpen.

## Aangepaste versus OTB-index

Soms moet u aangepaste indexen maken ter ondersteuning van uw zoekvereisten. Volg echter onderstaande richtlijnen voordat u aangepaste indexen maakt:

- Begrijp de onderzoeksvereisten en controleer als de indexen OOTB de onderzoeksvereisten kunnen steunen. Gebruiken **Query-prestaties**, beschikbaar op [lokale SDK](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) en AEMCS via de Developer Console of `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell`.

- Definieer een optimale query en gebruik de opdracht [query&#39;s optimaliseren](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices.html?#optimizing-queries) stroomschema en [JCR-query - Cheat Sheet](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) ter referentie.

- Als de OOTB-indexen de zoekvereisten niet ondersteunen, hebt u twee opties. Herzie echter de [Tips voor het maken van efficiënte indexen](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html?#should-i-create-an-index)
   - Pas de OOTB-index aan: voorkeursoptie omdat u deze eenvoudig kunt onderhouden en upgraden.
   - Volledig aangepaste index: alleen als de bovenstaande optie niet werkt.

### De OOTB-index aanpassen

- In **AEMCS**, bij het aanpassen van het OOTB-indexgebruik **\&lt;ootbindexname>-\&lt;productversion>-custom-\&lt;customversion>** naamgevingsconventie. Bijvoorbeeld: `cqPageLucene-custom-1` of `damAssetLucene-8-custom-1`. Zo kunt u de aangepaste indexdefinitie samenvoegen wanneer de OTB-index wordt bijgewerkt. Zie [Wijzigingen in indexen buiten de box](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#changes-to-out-of-the-box-indexes) voor meer informatie .

- In **AEM 6,X**, de bovenstaande naam _werkt niet_ U kunt de OOTB-index echter eenvoudig bijwerken met extra eigenschappen in het deelvenster `indexRules` knooppunt.

- Kopieer altijd de nieuwste OTB-indexdefinitie van de AEM-instantie met behulp van CRX DE Package Manager (/crx/packmgr/), wijzig de naam en voeg aanpassingen toe in het XML-bestand.

- De indexdefinitie van de opslag in het AEM project bij `ui.apps/src/main/content/jcr_root/_oak_index` en implementeren met behulp van Cloud Manager CI/CD-leidingen. Zie [Aangepaste indexdefinities gebruiken](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#deploying-custom-index-definitions) voor meer informatie .

### Volledig aangepaste index

Het maken van een volledig aangepaste index moet de laatste optie zijn en alleen als de bovenstaande optie niet werkt.

- Gebruik bij het maken van een volledig aangepaste index **\&lt;prefix>.\&lt;customindexname>-\&lt;version>-custom-\&lt;customversion>** naamgevingsconventie. Bijvoorbeeld: `wknd.adventures-1-custom-1`. Zo voorkomt u naamconflicten. Hier, `wknd` is het voorvoegsel en `adventures` is de aangepaste indexnaam. Deze conventie is zowel van toepassing op AEM 6.X als op AEMCS en helpt bij de voorbereiding op toekomstige migratie naar AEMCS.

- AEMCS steunt slechts indexen van Lucene, zodat om voor toekomstige migratie aan AEMCS voor te bereiden, gebruik altijd indexen van Lucene. Zie [Lucene-indexen vs. eigenschapsindexen](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html?#lucene-or-property-indexes) voor meer informatie .

- Maak geen aangepaste index op hetzelfde knooppunttype als de OOTB-index. Pas in plaats daarvan de OOTB-index aan met extra eigenschappen in het dialoogvenster `indexRules` knooppunt. Maak bijvoorbeeld geen aangepaste index voor de `dam:Asset` knooppunttype maar pas OOTB aan `damAssetLucene` index. _Het is een gemeenschappelijke oorzaak van prestaties en functionele kwesties geweest_.

- Vermijd ook het toevoegen van bijvoorbeeld meerdere knooppunttypen `cq:Page` en `cq:Tag` volgens de indexeringsregels (`indexRules`) node. Maak in plaats daarvan afzonderlijke indexen voor elk type knooppunt.

- Zoals vermeld in bovenstaande sectie, sla indexdefinitie in het AEM project op bij `ui.apps/src/main/content/jcr_root/_oak_index` en implementeren met behulp van Cloud Manager CI/CD-leidingen. Zie [Aangepaste indexdefinities gebruiken](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#deploying-custom-index-definitions) voor meer informatie .

- De richtlijnen voor indexdefinitie zijn:
   - Het knooppunttype (`jcr:primaryType`) `oak:QueryIndexDefinition`
   - Het indextype (`type`) `lucene`
   - De eigenschap async (`async`) `async,nrt`
   - Gebruiken `includedPaths` en vermijden `excludedPaths` eigenschap. Altijd instellen `queryPaths` waarde voor dezelfde waarde als `includedPaths` waarde.
   - Om de wegbeperking te handhaven, gebruik `evaluatePathRestrictions` eigenschap en instellen op `true`.
   - Gebruiken `tags` eigenschap om de index te labelen en tijdens het opvragen geeft u deze tagwaarde op om de index te gebruiken. De algemene querysyntaxis is `<query> option(index tag <tagName>)`.

  ```xml
  /oak:index/wknd.adventures-1-custom-1
      - jcr:primaryType = "oak:QueryIndexDefinition"
      - type = "lucene"
      - compatVersion = 2
      - async = ["async", "nrt"]
      - includedPaths = ["/content/wknd"]
      - queryPaths = ["/content/wknd"]
      - evaluatePathRestrictions = true
      - tags = ["customAdvSearch"]
  ...
  ```

### Voorbeelden

Om de beste praktijken te begrijpen, herzien enkele voorbeelden.

#### Onjuist gebruik van eigenschap tags

Onder de afbeelding ziet u een aangepaste en OOTB-indexdefinitie, die de `tags` eigenschap, beide indexen gebruiken hetzelfde `visualSimilaritySearch` waarde.

![Onjuist gebruik van eigenschap tags](./assets/understand-indexing-best-practices/incorrect-tags-property.png)

##### Analyse

Dit is een oneigenlijk gebruik van de `tags` eigenschap op de aangepaste index. De de vraagmotor van het Eak selecteert de douaneindex over de OTB indexoorzaak van de laagste geschatte kosten.

De juiste manier is om de OOTB-index aan te passen en aanvullende eigenschappen toe te voegen in de `indexRules` knooppunt. Zie [De OOTB-index aanpassen](#customize-the-ootb-index) voor meer informatie .

#### Index op de `dam:Asset` knooppunttype

Onder de afbeelding ziet u een aangepaste index voor de `dam:Asset` knooppunttype met `includedPaths` eigenschap ingesteld op een specifiek pad.

![Index van de dam:Asset nodetype](./assets/understand-indexing-best-practices/index-for-damAsset-type.png)

##### Analyse

Als u een uitgebreide zoekopdracht uitvoert op middelen, worden onjuiste resultaten geretourneerd, waardoor de aangepaste index lagere geschatte kosten heeft.

Maak geen aangepaste index op het tabblad `dam:Asset` knooppunttype maar pas OOTB aan `damAssetLucene` index met extra eigenschappen in de `indexRules` knooppunt.

#### Meerdere knooppunttypen onder indexeringsregels

Onder de afbeelding ziet u een aangepaste index met meerdere knooppunttypen onder `indexRules` knooppunt.

![Meerdere knooppunten onder de indexeringsregels](./assets/understand-indexing-best-practices/multiple-nodetypes-in-index.png)

##### Analyse

Het wordt niet aanbevolen meerdere knooppunttypen toe te voegen in één index, maar het is niet verstandig knooppunttypen in dezelfde index te indexeren als de knooppunttypen nauw verwant zijn, bijvoorbeeld `cq:Page` en `cq:PageContent`.

Een geldige oplossing is het aanpassen van de OOTB `cqPageLucene` en `damAssetLucene` index, aanvullende eigenschappen toevoegen onder de bestaande `indexRules` knooppunt.

#### Geen `queryPaths` eigenschap

Onder de afbeelding ziet u een aangepaste index (niet alleen de volgende naamgevingsconventie) zonder `queryPaths` eigenschap.

![Onzekerheid van de eigenschap queryPaths](./assets/understand-indexing-best-practices/absense-of-queryPaths-prop.png)

##### Analyse

Altijd instellen `queryPaths` waarde voor dezelfde waarde als `includedPaths` waarde. Om de padbeperking af te dwingen, stelt u ook `evaluatePathRestrictions` eigenschap aan `true`.

#### Vragen met indextag

Onder de afbeelding ziet u een aangepaste index met `tags` eigenschap en hoe deze te gebruiken tijdens het opvragen.

![Vragen met indextag](./assets/understand-indexing-best-practices/tags-prop-usage.png)

```
/jcr:root/content/dam//element(*,dam:Asset)[(jcr:content/@contentFragment = 'true' and jcr:contains(., '/content/sitebuilder/test/mysite/live/ja-jp/mypage'))]order by @jcr:created descending option (index tag assetPrefixNodeNameSearch)
```

##### Analyse

Toont aan hoe te om te plaatsen non-conflicterend en correct `tags` eigenschapswaarde op de index en gebruik deze tijdens het opvragen. De algemene querysyntaxis is `<query> option(index tag <tagName>)`. Zie ook [Index-tag voor zoekopties](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#query-option-index-tag)

#### Aangepaste index

Onder de afbeelding ziet u een aangepaste index met `suggestion` knooppunt voor het bereiken van de geavanceerde zoekfunctionaliteit.

![Aangepaste index](./assets/understand-indexing-best-practices/custom-index-with-suggestion-node.png)

##### Analyse

Het is een geldig geval van gebruik om een douaneindex voor te creëren [geavanceerd zoeken](https://jackrabbit.apache.org/oak/docs/query/lucene.html#advanced-search-features) functionaliteit. De indexnaam moet echter volgen op **\&lt;prefix>.\&lt;customindexname>-\&lt;version>-custom-\&lt;customversion>** naamgevingsconventie.


## Nuttige gereedschappen

Laten we enkele gereedschappen bekijken die u kunnen helpen de indexen te definiëren, te analyseren en te optimaliseren.

### Gereedschap Index maken

De [Generator voor eekindexdefinitie](https://oakutils.appspot.com/generate/index) hulp bij gereedschappen **om de indexdefinitie te genereren** op basis van de invoerquery&#39;s. Het is een goed beginpunt om een aangepaste index te maken.

### Gereedschap Index analyseren

De [Index Definition Analyzer](https://oakutils.appspot.com/analyze/index) hulp bij gereedschappen **om de indexdefinitie te analyseren** en bevat aanbevelingen om de indexdefinitie te verbeteren.

### Gereedschap Query-prestaties

De OOTB _Query-prestaties_ beschikbaar op [lokale SDK](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) en AEMCS via de Developer Console of `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell` help **om de vraagprestaties te analyseren** en [JCR-query - Cheat Sheet](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) om de optimale query te definiëren.

### Problemen met gereedschappen en tips oplossen

De meeste van de onderstaande opties zijn van toepassing voor AEM 6.X en lokale probleemoplossing.

- Indexbeheer beschikbaar op `http://host:port/libs/granite/operations/content/diagnosistools/indexManager.html` voor het ophalen van indexinformatie zoals type, laatst bijgewerkt, grootte.

- Gedetailleerde logboekregistratie van Java™-pakketten voor eik-query&#39;s en indexering, zoals `org.apache.jackrabbit.oak.plugins.index`, `org.apache.jackrabbit.oak.query`, en `com.day.cq.search` via `http://host:port/system/console/slinglog` voor het oplossen van problemen.

- JMX MBean van _IndexStats_ tekst beschikbaar op `http://host:port/system/console/jmx` voor het krijgen van indexinformatie zoals status, vooruitgang, of statistieken met betrekking tot asynchrone indexering. Het voorziet tevens in _FailingIndexStats_ Als er hier geen resultaten zijn, zijn er geen indexen beschadigd. AsyncIndexerService markeert om het even welke index die (configureerbaar) niet bijwerkt 30 minuten als corrupt en houdt op indexerend hen. Als een vraag niet verwachte resultaten geeft, is het nuttig voor ontwikkelaars om dit te controleren alvorens met het opnieuw indexeren te werk te gaan aangezien het opnieuw indexeren computerduur en tijdrovend is.

- JMX MBean van _LuceneIndex_ tekst beschikbaar op `http://host:port/system/console/jmx` voor Lucene Index-statistieken zoals grootte, aantal documenten per indexdefinitie.

- JMX MBean van _QueryState_ tekst beschikbaar op `http://host:port/system/console/jmx` voor de Statistieken van de Vraag van het Eak met inbegrip van langzame en populaire vragen met details zoals vraag, uitvoeringstijd.

## Aanvullende bronnen

Raadpleeg de volgende documentatie voor meer informatie:

- [Oak-query&#39;s en indexering](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/deploying/queries-and-indexing.html)
- [Aanbevolen werkwijzen voor query en indexering](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices.html)
- [Beste praktijken voor Vragen en het Indexeren](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html)
