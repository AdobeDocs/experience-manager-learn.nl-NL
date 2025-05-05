---
title: Traversale waarschuwingen in AEM as a Cloud Service
description: Leer hoe u waarschuwingen over het doorlopen in AEM as a Cloud Service kunt verminderen.
feature: Migration
role: Architect, Developer
level: Beginner
jira: KT-10427
hidefromtoc: true
hide: true
index: false
thumbnail: kt-10427.jpg
exl-id: 8fcc9364-b84c-4458-82e2-66b47429cd4b
duration: 155
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 0%

---

# Traversale waarschuwingen

>[!TIP]
>Bladwijzer maken van deze pagina voor toekomstig gebruik.

_wat zijn traversale waarschuwingen?_

De traversale waarschuwingen zijn __aError__ logboekverklaringen die op slecht het uitvoeren van vragen wijzen worden uitgevoerd op de dienst van AEM Publish. Traversale waarschuwingen worden gewoonlijk op twee manieren AEM:

1. __Trage vragen__ die geen indexen gebruiken, resulterend in langzame reactietijden.
1. __het Ontbreken vragen__, die a `RuntimeNodeTraversalException` werpen, resulterend in een gebroken ervaring.

Door het ongecontroleerd laten van traversale waarschuwingen worden de AEM vertraagd en kunnen de ervaringen voor uw gebruikers worden verbroken.

## Hoe te om traversale waarschuwingen op te lossen

Het verminderen van traversale waarschuwingen kan worden aangepakt gebruikend drie eenvoudige stappen: analyseren, aanpassen, en verifieert. Verwacht verschillende versies van aanpassen en verifiëren voordat u de optimale aanpassingen aanbrengt.

<div class="columns is-multiline">

<!-- Analyze -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Analyze" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#analyze" title="Analyseren" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/1-analyze.png" alt="Analyseren">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">Het probleem analyseren</p>
               <p class="is-size-6">Identificeer en begrijp welke vragen overlopen.</p>
               <a href="#analyze" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> analyseert </span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Adjust -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adjust" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#adjust" title="Aanpassen" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/2-adjust.png" alt="Aanpassen">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">De code of configuratie aanpassen</p>
               <p class="is-size-6">Werk vragen en indexen bij om vraagtraversals te vermijden.</p>
               <a href="#adjust" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> aanpassen </span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Verify -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Verify" tabindex="0">
   <div class="x-card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="#verify" title="Verifiëren" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/3-verify.png" alt="Verifiëren">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
                <p class="headline is-size-5 has-text-weight-bold">De bewerkte aanpassingen controleren</p>                       
               <p class="is-size-6">Verifieer veranderingen in vragen en indexen verwijderen traversals.</p>
               <a href="#verify" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> verifieer </span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>

## 1. Analyseren{#analyze}

Eerst identificeert u welke AEM Publish-services waarschuwingen over de hele wereld tonen. Om dit, van Cloud Manager te doen, `aemerror` logboeken van de diensten van Publish van 0&rbrace; downloaden [&#128279;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs.html?lang=nl-NL#cloud-manager){target="_blank"}  van alle milieu&#39;s (Dev, Stadium en Productie) voor het verleden __drie dagen__.

![ Logboeken van AEM as a Cloud Service van de Download ](./assets/traversals/download-logs.jpg)

Open de logbestanden en zoek naar de Java™-klasse `org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor` . Het logboek met traversal waarschuwingen bevat een reeks instructies die er ongeveer als volgt uitzien:

```log
24.05.2022 14:18:46.146 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.plugins.index.Cursors$TraversingCursor Traversed 5000 nodes with filter 
Filter(query=select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content') 
/* xpath: /jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]]) 
called by apps.wknd.components.search.example__002e__jsp._jspService; 
consider creating an index or changing the query
```

Afhankelijk van de context van de uitvoering van de query, kunnen de logboekverklaringen nuttige informatie over de schepper van de vraag bevatten:

+ HTTP-aanvraag-URL gekoppeld aan queryuitvoering

   + Voorbeeld: `GET /content/wknd/us/en/example.html HTTP/1.1`

+ Oak-querysyntaxis

   + Voorbeeld: `select [jcr:path], [jcr:score], * from [nt:base] as a where [xyz] = 'abc' and isdescendantnode(a, '/content')`

+ XPath-query

   + Voorbeeld: `/jcr:root/content//element(*, nt:base)[(@xyz = 'abc')] */, path=/content//*, property=[xyz=[abc]])`

+ Code die de query uitvoert

   + Voorbeeld: `apps.wknd.components.search.example__002e__jsp._jspService` → `/apps/wknd/components/search/example.html`

__het Vervallen vragen__ worden gevolgd door a `RuntimeNodeTraversalException` verklaring, gelijkend op:

```log
24.05.2022 14:18:47.240 [cm-p123-e456-aem-author-9876-edcba] *WARN* [192.150.10.214 [1653401908419] GET /content/wknd/us/en/example.html HTTP/1.1] 
org.apache.jackrabbit.oak.query.FilterIterators The query read or traversed more than 100000 nodes.
org.apache.jackrabbit.oak.query.RuntimeNodeTraversalException: 
    The query read or traversed more than 100000 nodes. To avoid affecting other tasks, processing was stopped.
    ...
```

## 2. Aanpassen{#adjust}

Zodra de beledigende vragen en hun het aanhalen code worden ontdekt, moeten aanpassingen worden aangebracht. Er kunnen twee soorten aanpassingen worden aangebracht om de doorlopende waarschuwingen te beperken:

### De query aanpassen

__verander de vraag__ om nieuwe vraagbeperkingen toe te voegen die aan bestaande indexbeperkingen oplossen. Wijzig indien mogelijk de query liever in indexen.

+ [ leer hoe te om vraagprestaties ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=nl-NL#query-performance-tuning){target="_blank"} te stemmen 

### De index aanpassen

__Verandering (of creeer) een AEM index__ dusdanig dat de bestaande vraagbeperkingen aan de indexupdates oplosbaar zijn.

+ [ leren hoe te om bestaande indexen ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=nl-NL#query-performance-tuning){target="_blank"} te stemmen 
+ [ leren hoe te om indexen ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/bestpractices/troubleshooting-slow-queries.html?lang=nl-NL#create-a-new-index){target="_blank"} tot stand te brengen 

## 3. Verifiëren{#verify}

Aanpassingen aan de query&#39;s, indexen of beide moeten worden geverifieerd om ervoor te zorgen dat ze de doorlopende waarschuwingen verminderen.

![ verklaart vraag ](./assets/traversals/verify.gif)

Als slechts [ aanpassingen aan de vraag ](#adjust-the-query) worden gemaakt, kan de vraag direct op AEM as a Cloud Service via Developer Console worden getest [ verklaart Vraag ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=nl-NL#queries){target="_blank"} . Verklaar de looppas van de Vraag tegen de AEM dienst van de Auteur, echter aangezien de indexdefinities het zelfde over de Diensten van de Auteur en van Publish zijn, volstaat het bevestigen van vragen tegen de dienst van de AEMAuteur.

Als [ aanpassingen aan de index ](#adjust-the-index) worden gemaakt, moet de index aan AEM as a Cloud Service worden opgesteld. Met de opgestelde indexaanpassingen, verklaart Developer Console [ Vraag ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html?lang=nl-NL#queries){target="_blank"}  kan worden gebruikt om de vraag verder uit te voeren en te stemmen.

Uiteindelijk, worden alle veranderingen (vraag en code) geëngageerd aan Git en opgesteld aan AEM as a Cloud Service gebruikend Cloud Manager. Als u de code eenmaal hebt geïmplementeerd, test u de codepaden die aan de oorspronkelijke traversal-waarschuwingen zijn gekoppeld, opnieuw en controleert u of traversal-waarschuwingen niet meer voorkomen in het `aemerror` -logbestand.

## Overige middelen

Bekijk deze andere nuttige bronnen voor het begrijpen van AEM indexen, zoekopdrachten en waarschuwingen voor het doorlopen.

<div class="columns is-multiline">

<!-- Cloud 5 - Search &amp; Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud 5 - Search &amp; Indexing" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=nl-NL" title="Wolk 5 - Zoeken en indexeren" tabindex="-1"><img class="is-bordered-r-small" src="../../../expert-resources/cloud-5/imgs/009-thumb.png" alt="Wolk 5 - Zoeken en indexeren"></a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=nl-NL" title="Wolk 5 - Zoeken en indexeren">Wolk 5 - Zoeken en indexeren</a></p>
               <p class="is-size-6">Het team van Cloud 5 laat zien wat de voordelen en uitvallen zijn van het zoeken en indexeren op AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/cloud-5/cloud5-aem-search-and-indexing.html?lang=nl-NL" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Content Search and Indexing -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Content Search and Indexing
" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=nl-NL" title="Inhoud zoeken en indexeren" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--docs.png" alt="Inhoud zoeken en indexeren">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=nl-NL" title="Inhoud zoeken en indexeren">Documentatie voor zoeken en indexeren van inhoud</a></p>
               <p class="is-size-6">Leer hoe u indexen maakt en beheert in AEM as a Cloud Service.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?lang=nl-NL" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Modernizing your Oak indexes -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modernizing your Oak indexes" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=nl-NL" title="Oak-indexen moderniseren" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--aem-experts-series.png" alt="Oak-indexen moderniseren">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=nl-NL" title="Oak-indexen moderniseren">Oak-indexen moderniseren</a></p>
               <p class="is-size-6">Leer hoe u AEM 6 Oak-indexdefinities omzet in AEM as a Cloud Service-compatibiliteit en hoe u de indexen in de toekomst kunt bijhouden.</p>
               <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/search-and-indexing.html?lang=nl-NL" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
               </a>
           </div>
       </div>
   </div>
</div>

<!-- Index definition documentation -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Index definition documentation" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentatie indexdefinitie" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/traversals/resources--oak-docs.png" alt="Documentatie indexdefinitie">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" title="Documentatie indexdefinitie">Documentatie van de Lucene-index</a></p>
               <p class="has-ellipsis is-size-6">De Apache Oak Jackrabbit Lucene-indexverwijzing die alle ondersteunde Lucene-indexconfiguraties documenteert.</p>
               <a href="https://jackrabbit.apache.org/oak/docs/query/lucene.html" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer meer </span>
               </a>
           </div>
       </div>
   </div>
</div>

</div>
