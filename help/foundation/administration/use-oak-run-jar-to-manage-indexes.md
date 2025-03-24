---
title: Gebruik oak-run.jar om indexen te beheren
description: Met de opdracht index van oak-run.jar wordt een aantal functies geconsolideerd voor het beheer van Oak-indexen in AEM, variërend van het verzamelen van indexstatistieken, het uitvoeren van indexconsistentiecontroles en het opnieuw/indexeren van indexen zelf.
version: Experience Manager 6.4, Experience Manager 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
duration: 726
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Gebruik oak-run.jar om indexen te beheren

Het indexbevel van [!DNL oak-run.jar] consolideert een aantal eigenschappen om [!DNL Oak] 200 indexen in AEM te beheren, van het verzamelen van indexstatistieken, lopende controles van de indexconsistentie, en re/indexeert indexen zelf.

>[!NOTE]
>
>In dit artikel en in video&#39;s worden de termen indexeren en opnieuw indexeren door elkaar gebruikt en als dezelfde bewerking beschouwd.

## [!DNL oak-run.jar] Basisinformatie over indexopdrachten

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* De gebruikte versie van [[!DNL oak-run.jar] ](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) moet de versie van Oak aanpassen die op de instantie van AEM wordt gebruikt.
* Als u indexen beheert met [!DNL oak-run.jar], gebruikt u de opdracht **[!DNL index]** met verschillende markeringen om verschillende bewerkingen te ondersteunen.

   * `java -jar oak-run*.jar index ...`

## Indexstatistieken

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* In `oak-run.jar` worden alle indexdefinities, belangrijke indexstatussen en indexinhoud voor offline analyse verwijderd.
* De verzameling van indexstatistieken kan veilig worden uitgevoerd op AEM-instanties in gebruik.

## Consistentiecontrole index

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` bepaalt snel of de indexen van lucene Oak corrupt zijn.
* De consistentiecontrole kan veilig worden uitgevoerd op in-use AEM-instantie voor consistentiecontroleniveaus 1 en 2.

## TarMK Online indexeren met [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Het online indexeren van [!DNL TarMK] met [!DNL oak-run.jar] is sneller dan het instellen van `reindex=true` op het knooppunt `oak:queryIndexDefinition` . Ondanks deze prestatieverhoging, vereist online indexeren gebruikend [!DNL oak-run.jar] nog een onderhoudsvenster om het indexeren uit te voeren.

* Het online indexeren van [!DNL TarMK] het gebruiken [!DNL oak-run.jar] zou **niet** tegen de instanties van AEM buiten het venster van het de instantieonderhoud van AEM moeten worden uitgevoerd.

## TarMK Offline indexeren met eiken-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Offlineindexering van [!DNL TarMK] using [!DNL oak-run.jar] is de eenvoudigste [!DNL oak-run.jar] gebaseerde indexeringsmethode voor [!DNL TarMK] omdat hiervoor één [!DNL oak-run.jar] -opdracht vereist is. Hiervoor moet de AEM-instantie echter worden afgesloten.

## TarMK out-of-band indexering met eak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* out-of-band indexering bij [!DNL TarMK] gebruik van [!DNL oak-run.jar] minimaliseert het effect van indexering op AEM-instanties in gebruik.
* De out-of-band indexering is de geadviseerde het indexeren benadering voor de installaties van AEM waar de tijd aan re/index de beschikbare onderhoudstijden overschrijdt.

## MongoMK online indexeren met eikenrun.jar

* Online index met [!DNL oak-run.jar] on [!DNL MongoMK] en [!DNL RDBMK] is de aanbevolen methode voor het opnieuw indexeren/opnieuw indexeren van AEM-installaties met [!DNL MongoMK] (en [!DNL RDBMK] ). **geen andere methode zou voor [!DNL MongoMK] of [!DNL RDBMK] moeten worden gebruikt.**
* Deze indexering hoeft alleen te worden uitgevoerd voor één AEM-instantie in de cluster.
* Het online indexeren van [!DNL MongoMK] kan veilig worden uitgevoerd in combinatie met een actief AEM-cluster, aangezien de dataopslag slechts op één [!DNL MongoDB] -knooppunt wordt doorlopen, zodat de andere knooppunten aanvragen kunnen blijven doorgeven zonder dat dit van invloed is op de prestaties.

Het [!DNL oak-run.jar] indexbevel om online het indexeren van [!DNL MongoMK] uit te voeren is het [ zelfde als  [!DNL TarMK]  Online het indexeren met  [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) met het verschil dat de parameter van de segmentopslag aan de [!DNL MongoDB] instantie richt die de opslag van de Knoop bevat.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Ondersteunende materialen

* [ Download  [!DNL oak-run.jar] ](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *verzeker de gedownloade versie de versie van Oak aanpast die op AEM zoals hierboven beschreven wordt geïnstalleerd*
* [ Apache Jackrabbit Oak oak-run.jar de Documentatie van het Bevel van de Index ](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
