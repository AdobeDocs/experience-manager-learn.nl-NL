---
title: Zoeken en indexeren in AEM as a Cloud Service
description: Leer over AEM as a Cloud Service onderzoeksindexen, hoe te om AEM 6 indexdefinities om te zetten, en hoe te om indexen op te stellen.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# Zoeken en indexeren

Leer over AEM as a Cloud Service onderzoeksindexen, hoe te om AEM 6 indexdefinities om te zetten om compatibel met AEM as a Cloud Service, en hoe te om indexen aan AEM as a Cloud Service op te stellen.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Indexconversie

![ het Hulpmiddel van de Convertor van de Index ](./assets/index-converter.png)

Als deel van het refactoring van uw codebasis, gebruik het [ omzetter van de Index hulpmiddel ](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) om de indexdefinities van douaneOak in AEM as a Cloud Service compatibele indexdefinities om te zetten.

Herzie de [ documentatie van de indexomzetter ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html) voor de volledige en huidige reeks mogelijkheden van de Convertor van de Index.

## Belangrijkste activiteiten

+ Gebruik het ](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) hulpmiddel van de Migrator van het Werkschema 0} Adobe I/O om werkschema&#39;s van de activaverwerking te migreren om de microdiensten van de Asset compute te gebruiken.[
+ Opstelling a [ lokale ontwikkelomgeving ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) en stel de aangepaste indexen op. Zorg ervoor dat de bijgewerkte indexen up-to-date zijn.
+ Implementeer de bijgewerkte codebasis in een AEM as a Cloud Service-ontwikkelomgeving en blijf valideren.
+ Als het wijzigen van een uit de doos index **** altijd kopieert de recentste indexdefinitie van een milieu van AEM as a Cloud Service dat op de recentste versie loopt. Pas de gekopieerde indexdefinitie aan uw behoeften aan.

## Handbeweging

Pas je kennis toe door uit te proberen wat je geleerd hebt met deze praktische oefening.

Voordat u de praktische oefening probeert, moet u controleren of u de bovenstaande video en de volgende materialen hebt bekeken en begrepen:

+ [AEM as a Cloud Service anders denken](./introduction.md)
+ [Modernisering opslagplaats](./repository-modernization.md)

Zorg er ook voor dat u de vorige hands-on oefening hebt uitgevoerd:

+ [Praktische oefening van het gereedschap Inhoudsoverdracht](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="Hands-on opslagplaats van GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Hands-on met indexen</div>
            <p style="margin:1rem 0">
                Ontdek het definiëren en implementeren van Oak-indexen voor AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> probeert uit het indexeren </span>
            </a>
        </td>
    </tr>
</table>
