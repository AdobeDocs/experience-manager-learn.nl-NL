---
title: Smart Translation Search instellen met AEM Assets
description: Met Smart Translation Search kunt u niet-Engelse zoektermen gebruiken om Engelse inhoud op te lossen. Om AEM in te stellen voor Smart Translation Search, moet de Apache OSGi-bundel voor zoekmachines voor zoekmachines worden geïnstalleerd en geconfigureerd, evenals de relevante gratis en open-source Apache Joshua-taalpakketten die de vertaalregels bevatten.
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

Met Smart Translation Search kunt u niet-Engelse zoektermen gebruiken om Engelse inhoud op te lossen. Om AEM in te stellen voor Smart Translation Search, moet de Apache OSGi-bundel voor zoekmachines voor zoekmachines worden geïnstalleerd en geconfigureerd, evenals de relevante gratis en open-source Apache Joshua-taalpakketten die de vertaalregels bevatten.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>Smart Translation Search moet worden ingesteld voor elke AEM die dit nodig heeft.

1. Download en installeer de OSGi-bundel van Oak Search Machine Translation
   * [Download de OSGi-bundel van Oak Search Machine Translation](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) die overeenkomt met AEM eik-versie.
   * Installeer de gedownloade OSGi-bundel voor Oak Search Machine Translation in AEM via [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Apache Joshua-taalpakketten downloaden en bijwerken
   * Download en decomprimeer de gewenste [Apache Joshua-taalpakketten](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Bewerk de `joshua.config` file en commentaren uit de 2 lijnen die met beginnen:

     ```
     feature-function = LanguageModel ...
     ```

   * Bepaal en registreer de grootte van de modelomslag van het taalpak, aangezien dit beïnvloedt hoeveel extra heapruimte AEM zal vereisen.
   * Verplaats de niet-gecomprimeerde Apache Joshua taalpakketmap (met de `joshua.config` bewerkingen) tot

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
      * Bijvoorbeeld: als in de pretaal verpakte AEM 8 GB heap vereist is om te worden uitgevoerd en de modelmap van het taalpakket 3,8 GB niet is gecomprimeerd, is de nieuwe heapgrootte:

        Het origineel `8GB` + ( `3.75GB` afgerond naar boven `2GB`, die `4GB`) voor een totaal van `12GB`

   * Controleer of de computer over deze hoeveelheid extra beschikbaar geheugen beschikt.
   * AEM opstartscripts bijwerken om deze aan te passen aan de nieuwe heapgrootte

      * Voorbeeld. `java -Xmx12g -jar cq-author-p4502.jar`

   * Start AEM opnieuw met de grotere heapgrootte.

   >[!NOTE]
   >
   >De vereiste heapruimte voor taalpakketten kan groter worden, vooral wanneer meerdere taalpakketten worden gebruikt.
   >
   >
   >Altijd controleren **de instantie heeft genoeg geheugen** om de toename in toegewezen heapruimte aan te passen.
   >
   >
   >De **basisheap moet altijd worden berekend om acceptabele prestaties te ondersteunen zonder taalpakketten** geïnstalleerd.

4. Registreer de taalpakken via Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi configuraties

   * Voor elke taalverpakking [Maak een nieuwe Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi configuratie](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) via de configuratiemanager van de AEM webconsole.

      * `Joshua Config Path` is de absolute weg aan het joshua.config- dossier. Het AEM moet alle bestanden in de map van het taalpakket kunnen lezen.
      * `Node types` zijn de types van kandidaatknoop waarvan full-text onderzoek dit taalpak voor vertaling zal in dienst nemen.
      * `Minimum score` is de minimale betrouwbaarheidsscore voor een vertaalde term die moet worden gebruikt.

         * Hombre (Spaans voor &quot;man&quot;) kan bijvoorbeeld worden vertaald naar het Engelse woord &quot;man&quot; met een betrouwbaarheidsscore van `0.9` en vertalen naar het engelse woord &quot; human &quot; met een betrouwbaarheidsscore `0.2`. De minimumscore aanpassen aan `0.3`, zou de vertaling &quot;hombre&quot; behouden aan &quot;man&quot;, maar het &#39;hombre&#39; in &quot;human&quot; vertaling negeren als de vertaalscore van `0.2` is kleiner dan de minimumscore van `0.3`.

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

Om [Slimme tags AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) waarop AEM Smart Translation van invloed is, AEM `/oak   :index  /damAssetLucene` De index moet worden bijgewerkt om de predictedTags (de systeemnaam voor &quot;Slimme tags&quot;) te markeren als onderdeel van de samengestelde Lucene-index van het element.

Onder `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`dient u te zorgen dat de configuratie als volgt is:

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
