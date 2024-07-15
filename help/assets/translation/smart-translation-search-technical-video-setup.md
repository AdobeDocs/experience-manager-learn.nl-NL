---
title: Smart Translation Search instellen met AEM Assets
description: Met Smart Translation Search kunt u niet-Engelse zoektermen gebruiken om Engelse inhoud op te lossen. Voor het instellen van AEM voor Smart Translation Search moet de Apache Oak Search Machine Translation OSGi-bundel geïnstalleerd en geconfigureerd zijn, evenals de relevante gratis en open-source Apache Joshua-taalpakketten die de vertaalregels bevatten.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 603
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# Smart Translation Search instellen met AEM Assets{#set-up-smart-translation-search-with-aem-assets}

Met Smart Translation Search kunt u niet-Engelse zoektermen gebruiken om Engelse inhoud op te lossen. Voor het instellen van AEM voor Smart Translation Search moet de Apache Oak Search Machine Translation OSGi-bundel geïnstalleerd en geconfigureerd zijn, evenals de relevante gratis en open-source Apache Joshua-taalpakketten die de vertaalregels bevatten.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>Smart Translation Search moet worden ingesteld voor elke AEM die dit nodig heeft.

1. Download en installeer de Oak Search Machine Translation OSGi-bundel
   * [ Download de bundel van OSGi van de Vertaling van de Machine van het Onderzoek van Oak ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) die aan AEM versie van Oak beantwoordt.
   * Installeer de gedownloade OSGi-bundel voor het vertalen van Oak Search Machine in AEM via [`/system/console/bundles` ](http://localhost:4502/system/console/bundles) .

2. Apache Joshua-taalpakketten downloaden en bijwerken
   * De download en decomprimeert de gewenste [ Apache Joshua taalpakken ](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Bewerk het `joshua.config` -bestand en becommentariëer de twee regels die beginnen met:

     ```
     feature-function = LanguageModel ...
     ```

   * Bepaal en registreer de grootte van de modelomslag van het taalpak, aangezien dit beïnvloedt hoeveel extra heapruimte AEM zal vereisen.
   * Verplaats de niet-gecomprimeerde Apache Joshua-taalpakketmap (met de `joshua.config` bewerkingen) naar

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     Bijvoorbeeld:

     ```
      .../crx-quickstart/opt/es-en
     ```

3. AEM opnieuw starten met bijgewerkte heapgeheugentoewijzing
   * AEM stoppen
   * De nieuwe vereiste heapgrootte voor AEM bepalen

      * AEM de heapgrootte voor het ontbreken van talen + de grootte van de modeldirectory afgerond naar de dichtstbijzijnde 2 GB
      * Bijvoorbeeld: als in de pretaal verpakte AEM 8 GB heap vereist is om te worden uitgevoerd en de modelmap van het taalpakket 3,8 GB niet is gecomprimeerd, is de nieuwe heapgrootte:

        Het origineel `8GB` + ( `3.75GB` afgerond naar de dichtstbijzijnde `2GB` , hetgeen `4GB` is) voor een totaal van `12GB`

   * Controleer of de computer over deze hoeveelheid extra beschikbaar geheugen beschikt.
   * AEM opstartscripts bijwerken om deze aan te passen aan de nieuwe heapgrootte

      * Voorbeeld. `java -Xmx12g -jar cq-author-p4502.jar`

   * Start AEM opnieuw met de grotere heapgrootte.

   >[!NOTE]
   >
   >De vereiste heapruimte voor taalpakketten kan groter worden, vooral wanneer meerdere taalpakketten worden gebruikt.
   >
   >
   >Zorg altijd ervoor **de instantie genoeg geheugen** heeft om de verhogingen in toegewezen heapruimte aan te passen.
   >
   >
   >De **basisheap moet altijd worden berekend om aanvaardbare prestaties zonder enige geïnstalleerde taalpakken** te steunen.

4. Registreer de taalpakketten via Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi configuraties

   * Voor elk taalpak, [ creeer een nieuwe Apache Jackrabbit Oak Machine Translation Full-text configuratie van de Leverancier OSGi van de Vraag van de Vraag ](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) via de manager van de Configuratie van de Console van het AEM.

      * `Joshua Config Path` is het absolute pad naar het bestand joshua.config. Het AEM moet alle bestanden in de map van het taalpakket kunnen lezen.
      * `Node types` zijn de kandidaatknooppunttypes waarvan full-text onderzoek dit taalpak voor vertaling zal in dienst nemen.
      * `Minimum score` is de minimale betrouwbaarheidsscore voor een vertaalde term die moet worden gebruikt.

         * Hombre (Spaans voor &quot;man&quot;) kan bijvoorbeeld worden vertaald naar het Engelse woord &quot;man&quot; met een betrouwbaarheidsscore van `0.9` en ook naar het Engelse woord &quot;human&quot; met een betrouwbaarheidsscore `0.2` . Als de minimumscore wordt ingesteld op `0.3` , blijft de vertaling &#39;hombre&#39; behouden in &#39;man&#39;, maar wordt de vertaling &#39;hombre&#39; genegeerd in &#39;human&#39;, omdat de vertaalscore van `0.2` lager is dan de minimumscore van `0.3` .

5. Een zoekopdracht in volledige tekst uitvoeren op elementen
   * Omdat dam:Asset het knooptype is dit taalpak opnieuw wordt geregistreerd, moeten wij naar AEM Assets zoeken gebruikend full-text onderzoek om dit te bevestigen.
   * Ga naar AEM > Assets en open Onderzoek. Zoeken naar een term in de taal waarvan het taalpakket is geïnstalleerd.
   * Stel zo nodig de minimale score in de OSGi-configuraties in om de nauwkeurigheid van de resultaten te garanderen.

6. Taalpakketten bijwerken
   * Het Apache Joshua-taalpakket wordt volledig onderhouden door het Apache Joshua-project en het bijwerken of corrigeren ervan wordt door het Apache Joshua-project bepaald.
   * Als een taalpakket wordt bijgewerkt en de updates in AEM worden geïnstalleerd, moeten de bovenstaande stappen 2 - 4 worden gevolgd en moet de heapgrootte naar boven of beneden worden aangepast.

      * Houd er rekening mee dat wanneer u het niet-gecomprimeerde taalpakket verplaatst naar de map crx-quickstart/opt, u een bestaande taalpakketmap verplaatst voordat u het nieuwe taalpakket kopieert.

   * Als AEM geen nieuw begin vereist, dan moeten de relevante configuratie(s) van OSGi van de Leverancier van de Vraag van de Machine van Apache Jackrabbit Oak van de Vertaling van Fulltext die tot de bijgewerkte taalverpakking(en) behoren opnieuw worden bewaard zodat AEM de bijgewerkte dossiers verwerkt.

## damAssetLucene Index bijwerken {#updating-damassetlucene-index}

Opdat [ AEM Slimme Markeringen ](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) door AEM Slimme Vertaling worden beïnvloed, moet AEM `/oak   :index  /damAssetLucene` index worden bijgewerkt om predictedTags (de systeemnaam voor &quot;Slimme Markeringen&quot;) te merken om deel uit te maken van de samengestelde index van Lucene van het Activum.

Controleer onder `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags` of de configuratie als volgt is:

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## Aanvullende bronnen{#additional-resources}

* [ Apache Oak de Vertaling OSGi van de Machine van het Onderzoek bundel ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [ de Pakketten van de Taal van Apache Joshua ](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [ AEM Slimme Markeringen ](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [ Beste praktijken voor het Vragen en het Indexeren ](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
