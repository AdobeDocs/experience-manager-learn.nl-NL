---
title: Gebruik oak-run.jar om indexen te beheren
description: Het indexbevel van oak-run.jar consolideert een aantal eigenschappen om indexen van het Eak in AEM te beheren, van het verzamelen van indexstatistieken, het runnen van indexconsistentiecontroles, en re/indexeert indexen zelf.
version: 6.4, 6.5
feature: oak
topics: search
activity: use
audience: architect, developer, implementer
doc-type: technical video
translation-type: tm+mt
source-git-commit: e19e177589df7ce6a56c0be3f9d590cbca2f8ce7
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---


# Gebruik oak-run.jar om indexen te beheren

[!DNL oak-run.jar]De de indexbevel van s consolideert een aantal eigenschappen om  [!DNL Oak]200 indexen in AEM te beheren, van het verzamelen van indexstatistieken, lopende controles van de indexconsistentie, en re/indexeert indexen zelf.

>[!NOTE]
>
>In dit artikel en in video&#39;s worden de termen indexeren en opnieuw indexeren door elkaar gebruikt en als dezelfde bewerking beschouwd.

## [!DNL oak-run.jar] Basisbeginselen van indexopdrachten

>[!VIDEO](https://video.tv.adobe.com/v/21475/?quality=9&learn=on)

* De gebruikte versie van [[!DNL oak-run.jar]](https://repository.apache.org/service/local/artifact/maven/redirect?r=releases&amp;g=org.apache.jackrabbit&amp;a=oak-run&amp;v=1.8.0) moet de versie van Oak aanpassen die op de AEM instantie wordt gebruikt.
* Het beheren van indexen die [!DNL oak-run.jar] gebruiken hefboomwerkingen **[!DNL index]** bevel met diverse vlaggen om verschillende verrichtingen te steunen.

   * `java -jar oak-run*.jar index ...`

## Indexstatistieken

>[!VIDEO](https://video.tv.adobe.com/v/21477/?quality=12&learn=on)

* `oak-run.jar` Hiermee plaatst u alle indexdefinities, belangrijke indexstatussen en indexinhoud voor offline analyse.
* Het verzamelen van indexstatistieken is veilig om op in-use AEM instanties uit te voeren.

## Consistentiecontrole index

>[!VIDEO](https://video.tv.adobe.com/v/21476/?quality=12&learn=on)

* `oak-run.jar` bepaalt snel of de indexen van de eikenhout van de lucene corrupte zijn.
* De consistentiecontrole kan veilig worden uitgevoerd op AEM instantie voor consistentiecontroleniveaus 1 en 2.

## TarMK Online indexeren met [!DNL oak-run.jar] {#tarmkonlineindexingwithoakrunjar}

>[!VIDEO](https://video.tv.adobe.com/v/21479/?quality=12&learn=on)

* Het online indexeren van [!DNL TarMK] gebruikend [!DNL oak-run.jar] is sneller dan het plaatsen `reindex=true` op `oak:queryIndexDefinition` knoop. Ondanks deze prestatieverhoging, vereist online indexeren gebruikend [!DNL oak-run.jar] nog een onderhoudsvenster om het indexeren uit te voeren.

* Onlineindexering van [!DNL TarMK] met behulp van [!DNL oak-run.jar] moet **not** worden uitgevoerd tegen AEM exemplaren buiten het venster voor onderhoud van AEM instanties.

## TarMK Offline indexeren met eiken-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21478/?quality=12&learn=on)

* Offlineindexering van [!DNL TarMK] door [!DNL oak-run.jar] te gebruiken is de eenvoudigste [!DNL oak-run.jar] gebaseerde indexeringsbenadering voor [!DNL TarMK] aangezien het één enkele [!DNL oak-run.jar] bevel vereist, nochtans vereist het AEM instantie om sluiting te zijn.

## TarMK out-of-band indexering met eak-run.jar

>[!VIDEO](https://video.tv.adobe.com/v/21480/?quality=12&learn=on)

* De out-of-band indexering op [!DNL TarMK] gebruikend [!DNL oak-run.jar] minimaliseert het effect van indexeren op in-use AEM instanties.
* De out-of-band indexering is de geadviseerde het indexeren benadering voor AEM installaties waar de tijd aan re/index de beschikbare onderhoudstijden overschrijdt.

## MongoMK online indexeren met eikenrun.jar

* Online index met [!DNL oak-run.jar] op [!DNL MongoMK] en [!DNL RDBMK] is de geadviseerde methode voor het re/indexeren van [!DNL MongoMK] (en [!DNL RDBMK]) AEM installaties. **Er mag geen andere methode worden gebruikt voor  [!DNL MongoMK] of  [!DNL RDBMK].**
* Deze indexering hoeft alleen te worden uitgevoerd op één AEM in de cluster.
* Het online indexeren van [!DNL MongoMK] is veilig om tegen een lopende AEM cluster uit te voeren, aangezien de repository traversal slechts op één [!DNL MongoDB] knoop zal voorkomen, toelatend anderen om verzoeken zonder significante prestatiesinvloed te blijven dienen.

De [!DNL oak-run.jar] indexbevel om een online indexering van [!DNL MongoMK] uit te voeren is [zelfde als  [!DNL TarMK] Online indexeren met [!DNL oak-run.jar]](#tarmkonlineindexingwithoakrunjar) met het verschil dat de parameter van de segmentopslag aan de [!DNL MongoDB] instantie richt die de opslag van de Knoop bevat.

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
