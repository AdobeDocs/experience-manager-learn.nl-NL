---
title: Gebruik oak-run.jar om indexen te beheren
description: Het indexbevel van oak-run.jar consolideert een aantal eigenschappen om indexen van het Eak in AEM te beheren, van het verzamelen van indexstatistieken, het runnen van indexconsistentiecontroles, en re/indexeert indexen zelf.
version: 6.4, 6.5
feature: Search
doc-type: Technical Video
topic: Performance
role: Developer
level: Experienced
exl-id: be49718e-f1f5-4ab2-9c9d-6430a52bb439
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Gebruik oak-run.jar om indexen te beheren

[!DNL oak-run.jar]De indexopdracht van de index consolideert een aantal te beheren functies [!DNL Oak]200 indexen in AEM, van het verzamelen van indexstatistieken, de lopende controles van de indexconsistentie, en re/indexeert indexen zelf.

>[!NOTE]
>
>In dit artikel en in video&#39;s worden de termen indexeren en opnieuw indexeren door elkaar gebruikt en als dezelfde bewerking beschouwd.

## [!DNL oak-run.jar] Basisbeginselen van indexopdrachten

>[!VIDEO](https://video.tv.adobe.com/v/21475?quality=12&learn=on)

* De versie van [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) gebruikt moet overeenkomen met de versie van Oak die op de AEM-instantie wordt gebruikt.
* Indexen beheren met [!DNL oak-run.jar] gebruikt de **[!DNL index]** gebruiken met verschillende markeringen om verschillende bewerkingen te ondersteunen.

   * `java -jar oak-run*.jar index ...`

## Indexstatistieken

>[!VIDEO](https://video.tv.adobe.com/v/21477?quality=12&learn=on)

* `oak-run.jar` Hiermee plaatst u alle indexdefinities, belangrijke indexstatussen en indexinhoud voor offline analyse.
* Het verzamelen van indexstatistieken is veilig om op in-use AEM instanties uit te voeren.

## Consistentiecontrole index

>[!VIDEO](https://video.tv.adobe.com/v/21476?quality=12&learn=on)

* `oak-run.jar` bepaalt snel of de indexen van de eikenhout van de lucene corrupte zijn.
* De consistentiecontrole kan veilig worden uitgevoerd op AEM instantie voor consistentiecontroleniveaus 1 en 2.

## TarMK Online indexeren met [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479?quality=12&learn=on)

* Online indexeren van [!DNL TarMK] gebruiken [!DNL oak-run.jar] is sneller dan instellen `reindex=true` op de `oak:queryIndexDefinition` knooppunt. Ondanks deze prestatieverbetering kunt u online indexeren met [!DNL oak-run.jar] nog vereist een onderhoudsvenster om het indexeren uit te voeren.

* Online indexeren van [!DNL TarMK] gebruiken [!DNL oak-run.jar] moet **niet** worden uitgevoerd tegen AEM instanties buiten het onderhoudsvenster van AEM instanties.

## TarMK Offline indexeren met eiken-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478?quality=12&learn=on)

* Offlineindexering van [!DNL TarMK] gebruiken [!DNL oak-run.jar] is de eenvoudigste [!DNL oak-run.jar] gebaseerde indexeringsaanpak voor [!DNL TarMK] omdat er maar één nodig is [!DNL oak-run.jar] gebruiken, maar de AEM instantie moet worden afgesloten.

## TarMK out-of-band indexering met eak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480?quality=12&learn=on)

* out-of-band indexering aan [!DNL TarMK] gebruiken [!DNL oak-run.jar] Hiermee minimaliseert u het effect van indexering op gebruikte AEM.
* De out-of-band indexering is de geadviseerde het indexeren benadering voor AEM installaties waar de tijd aan re/index de beschikbare onderhoudstijden overschrijdt.

## MongoMK online indexeren met eikenrun.jar

* Online index met [!DNL oak-run.jar] op [!DNL MongoMK] en [!DNL RDBMK] is de aanbevolen methode voor re/indexering [!DNL MongoMK] (en [!DNL RDBMK]) AEM installaties. **Er mag geen andere methode worden gebruikt [!DNL MongoMK] of [!DNL RDBMK].**
* Deze indexering hoeft alleen te worden uitgevoerd op één AEM in de cluster.
* Online indexeren van [!DNL MongoMK] is veilig om tegen een lopende AEM cluster uit te voeren, aangezien de dataopslag-verplaatsing slechts op één enkele plaats zal plaatsvinden [!DNL MongoDB] knoop, die anderen toestaan om verzoeken zonder significante prestatieseffect te blijven dienen.

De [!DNL oak-run.jar] indexopdracht om een online indexering uit te voeren van [!DNL MongoMK] is de [gelijk aan [!DNL TarMK] Online indexeren met [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) met het verschil dat de segmentopslagparameter naar de [!DNL MongoDB] instantie die de Node-opslag bevat.

```
java -jar oak-run*.jar index
 --reindex
 --index-paths=/oak:index/lucene
 --read-write
 --fds-path=/path/to/datastore mongodb://server:port/aem
```

## Ondersteunende materialen

* [Downloaden [!DNL oak-run.jar]](https://repository.apache.org/#nexus-search;gav~org.apache.jackrabbit~oak-run~~~~kw,versionexpand)
   * *Zorg ervoor dat de gedownloade versie overeenkomt met de versie die op de hierboven beschreven AEM is geïnstalleerd.*
* [Documentatie van de opdracht Apache Jackrabbit Oak-run.jar Index](https://jackrabbit.apache.org/oak/docs/query/oak-run-indexing.html)
