---
title: Smart Translation Search instellen met AEM Assets
seo-title: Smart Translation Search instellen met AEM Assets
description: Met Smart Translation Search kunt u niet-Engelse zoektermen gebruiken om Engelse inhoud op te lossen. Om AEM in te stellen voor Smart Translation Search, moet de Apache OSGi-bundel voor zoekmachines voor zoekmachines worden geïnstalleerd en geconfigureerd, evenals de relevante gratis en open-source Apache Joshua-taalpakketten die de vertaalregels bevatten.
seo-description: Met Smart Translation Search kunt u niet-Engelse zoektermen gebruiken om Engelse inhoud op te lossen. Om AEM in te stellen voor Smart Translation Search, moet de Apache OSGi-bundel voor zoekmachines voor zoekmachines worden geïnstalleerd en geconfigureerd, evenals de relevante gratis en open-source Apache Joshua-taalpakketten die de vertaalregels bevatten.
uuid: b0e8dab2-6bc4-4158-91a1-4b9811359798
discoiquuid: 4db1b4db-74f4-4646-b5de-cb891612cc90
topics: authoring, search, metadata, localization
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '929'
ht-degree: 0%

---


# Smart Translation Search instellen met AEM Assets{#set-up-smart-translation-search-with-aem-assets}

Met Smart Translation Search kunt u niet-Engelse zoektermen gebruiken om Engelse inhoud op te lossen. Om AEM in te stellen voor Smart Translation Search, moet de Apache OSGi-bundel voor zoekmachines voor zoekmachines worden geïnstalleerd en geconfigureerd, evenals de relevante gratis en open-source Apache Joshua-taalpakketten die de vertaalregels bevatten.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>Smart Translation Search moet worden ingesteld voor elke AEM die dit nodig heeft.

1. Download en installeer de OSGi-bundel van Oak Search Machine Translation
   * [Download de OSGi-bundel](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) Oak Search Machine Translation die overeenkomt met AEM Oak-versie.
   * Installeer de gedownloade OSGi-bundel voor Oak Search Machine Translation in AEM via [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Apache Joshua-taalpakketten downloaden en bijwerken
   * Download en decomprimeer de gewenste [Apache Joshua-taalpakketten](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Bewerk het `joshua.config` bestand en becommentariëer de twee regels die beginnen met:

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

      * AEM heapgrootte vóór het taalgebrek + de grootte van de modeldirectory afgerond naar de dichtstbijzijnde 2 GB
      * Bijvoorbeeld: Als de pre-taalpakken de AEM installatie 8 GB van hoop vereist om te lopen, en de modelomslag van het taalpak is niet samengedrukt 3.8 GB, is de nieuwe heapgrootte:

         Het origineel `8GB` + ( `3.75GB` afgerond naar de dichtstbijzijnde `2GB`, hetgeen `4GB`) voor een totaal van `12GB`
   * Controleer of de computer over deze hoeveelheid extra beschikbaar geheugen beschikt.
   * AEM opstartscripts bijwerken om deze aan te passen aan de nieuwe heapgrootte

      * Voorbeeld. `java -Xmx12g -jar cq-author-p4502.jar`
   * Start AEM opnieuw met de grotere heapgrootte.

   >[!NOTE]
   >
   >De vereiste heapruimte voor taalpakketten kan groter worden, vooral wanneer meerdere taalpakketten worden gebruikt.
   >
   >
   >Zorg altijd ervoor dat **de instantie voldoende geheugen** heeft om de toename van toegewezen heapruimte te compenseren.
   >
   >
   >De **basisheap moet altijd worden berekend om acceptabele prestaties te ondersteunen zonder dat taalpakketten** zijn geïnstalleerd.

4. Registreer de taalpakken via Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi configuraties

   * Voor elk taalpak, [creeer een nieuw Apache Jackrabbit Oak Machine Translation Full-text de Leverancier OSGi van de Vraag van de Leverancier van de Termen](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) via de Manager van de Configuratie van de Console van het AEM Web.

      * `Joshua Config Path` is de absolute weg aan het joshua.config- dossier. Het AEM moet alle bestanden in de map van het taalpakket kunnen lezen.
      * `Node types` zijn de types van kandidaatknoop waarvan full-text onderzoek dit taalpak voor vertaling zal in dienst nemen.
      * `Minimum score` is de minimale betrouwbaarheidsscore voor een vertaalde term die moet worden gebruikt.

         * Hombre (Spaans voor &quot;man&quot;) kan bijvoorbeeld naar het Engelse woord &quot;man&quot; vertalen met een betrouwbaarheidsscore van `0.9` en ook naar het Engelse woord &quot;human&quot; vertalen met een betrouwbaarheidsscore `0.2`. Als de minimumscore op `0.3`wordt ingesteld, blijft de vertaling &#39;hombre&#39; behouden in &#39;man&#39;, maar wordt het &#39;hombre&#39; genegeerd in &#39;human&#39; vertaling, omdat deze vertaalscore van `0.2` minder is dan de minimumscore van `0.3`.

5. Een zoekopdracht in volledige tekst uitvoeren op elementen
   * Omdat dam:Asset het knooptype is dit taalpak opnieuw wordt geregistreerd, moeten wij naar AEM Assets zoeken gebruikend full-text onderzoek om dit te bevestigen.
   * Ga naar AEM > Middelen en open Onderzoek. Zoeken naar een term in de taal waarvan het taalpakket is geïnstalleerd.
   * Stel zo nodig de minimale score in de OSGi-configuraties in om de nauwkeurigheid van de resultaten te garanderen.

6. Taalpakketten bijwerken
   * Het Apache Joshua-taalpakket wordt volledig onderhouden door het Apache Joshua-project en het bijwerken of corrigeren ervan wordt door het Apache Joshua-project bepaald.
   * Als een taalpakket wordt bijgewerkt en de updates in AEM worden geïnstalleerd, moeten de bovenstaande stappen 2 - 4 worden gevolgd en moet de heapgrootte naar boven of beneden worden aangepast.

      * Houd er rekening mee dat wanneer u het niet-gecomprimeerde taalpakket verplaatst naar de map crx-quickstart/opt, u een bestaande taalpakketmap verplaatst voordat u het nieuwe taalpakket kopieert.
   * Als AEM geen nieuw begin vereist, dan moeten de relevante Apache Jackrabbit Oak Machine Translation Fulltext Query Terms Provider OSGi configuratie(s) die tot het bijgewerkte taalpak(s) behoren opnieuw worden opgeslagen zodat AEM de bijgewerkte bestanden verwerkt.


## damAssetLucene Index bijwerken {#updating-damassetlucene-index}

Om ervoor te zorgen dat [AEM Slimme Markeringen](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) door AEM Slimme Vertaling worden beïnvloed, AEM `/oak   :index  /damAssetLucene` index moet worden bijgewerkt om de predictedTags (de systeemnaam voor &quot;Slimme Markeringen&quot;) te markeren als onderdeel van de samengestelde Lucene-index van het Element.

Controleer `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`onder of de configuratie als volgt is:

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

* [Apache Oak Search Machine Translation OSGi bundle](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua Language Packs](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Slimme tags AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Beste praktijken voor het Vragen en het Indexeren](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)