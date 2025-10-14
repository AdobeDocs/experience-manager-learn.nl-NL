---
title: Zoeken en indexeren in AEM as a Cloud Service
description: Meer informatie over zoekindexen in AEM as a Cloud Service, hoe u AEM 6-indexdefinities omzet en hoe u indexen kunt implementeren.
version: Experience Manager as a Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# Zoeken en indexeren

Leer hoe u AEM as a Cloud Service-zoekindexen gebruikt, hoe u AEM 6-indexdefinities omzet in AEM as a Cloud Service-compatibel en hoe u indexen implementeert in AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Indexconversie

![&#x200B; het Hulpmiddel van de Convertor van de Index &#x200B;](./assets/index-converter.png)

Als deel van het refactoring van uw codebasis, gebruik het [&#x200B; omzetter van de Index hulpmiddel &#x200B;](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) om de indexdefinities van douaneOak in AEM as a Cloud Service compatibele indexdefinities om te zetten.

Herzie de [&#x200B; documentatie van de indexomzetter &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html?lang=nl-NL) voor de volledige en huidige reeks mogelijkheden van de Convertor van de Index.

## Belangrijkste activiteiten

+ Gebruik het [&#128279;](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) hulpmiddel van de Migrator van het Werkschema van 0&rbrace; Adobe I/O &lbrace;om werkschema&#39;s van de activaverwerking te migreren om de microdiensten van Asset Compute te gebruiken.
+ Opstelling a [&#x200B; lokale ontwikkelomgeving &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL) en stel de aangepaste indexen op. Zorg ervoor dat de bijgewerkte indexen up-to-date zijn.
+ Implementeer de bijgewerkte codebasis in een AEM as a Cloud Service-ontwikkelomgeving en blijf valideren.
+ Als het wijzigen van een uit de doos index **&#x200B;**&#x200B;altijd kopieert de recentste indexdefinitie van een milieu van AEM as a Cloud Service dat op de recentste versie loopt. Pas de gekopieerde indexdefinitie aan uw behoeften aan.

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
