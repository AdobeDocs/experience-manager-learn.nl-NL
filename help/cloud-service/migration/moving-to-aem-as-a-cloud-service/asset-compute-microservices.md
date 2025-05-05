---
title: AEM Assets Microservices en overstappen naar AEM as a Cloud Service
description: Leer hoe u met AEM Assets as a Cloud Service Asset Compute-microservices automatisch en efficiënt uitvoering voor uw middelen kunt genereren en deze rol van de traditionele AEM Workflow kunt vervangen.
version: Experience Manager as a Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 973
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# AEM Assets Microservices - Naar AEM as a Cloud Service

Leer hoe u met AEM Assets as a Cloud Service Asset Compute-microservices automatisch en efficiënt uitvoering voor uw middelen kunt genereren en deze rol van de traditionele AEM Workflow kunt vervangen.

>[!VIDEO](https://video.tv.adobe.com/v/3454290?quality=12&learn=on&captions=dut)

## Workflow-migratiehulpprogramma

![ Hulpmiddel van de Migratie van het Werkschema van Activa ](./assets/asset-workflow-migration.png)

Als deel van het refactoring van uw codebasis, gebruik het [ hulpmiddel van de Migratie van het Werkschema van Activa ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=nl-NL) om bestaande werkschema&#39;s te migreren om de microdiensten van Asset Compute in AEM as a Cloud Service te gebruiken.

## Belangrijkste activiteiten

+ Gebruik het [&#128279;](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) hulpmiddel van de Migrator van het Werkschema van 0&rbrace; Adobe I/O &lbrace;om werkschema&#39;s van de activaverwerking te migreren om de microdiensten van Asset Compute te gebruiken.
+ Opstelling a [ lokale ontwikkelomgeving ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=nl-NL) en stel de bijgewerkte werkschema&#39;s op. Mogelijk is een handmatige aanpassing nodig voor complexe workflows.
+ Doorgaan met herhalen in een lokale ontwikkelomgeving met de AEM SDK totdat de bijgewerkte workflow overeenkomt met de pariteit van de functie.
+ Implementeer de bijgewerkte codebasis in een AEM as a Cloud Service-ontwikkelomgeving en blijf valideren.

## Handbeweging

Pas je kennis toe door uit te proberen wat je geleerd hebt met deze praktische oefening.

Voordat u de praktische oefening probeert, moet u controleren of u de bovenstaande video en de volgende materialen hebt bekeken en begrepen:

+ [AEM as a Cloud Service anders denken](./introduction.md)
+ [Onboarding](./onboarding.md)

Zorg er ook voor dat u de vorige hands-on oefening hebt uitgevoerd:

+ [Praktijkoefening zoeken en indexeren](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="Hands-on opslagplaats van GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Hands-on met het uploaden van activa</div>
            <p style="margin:1rem 0">
                Ontdek hoe u AEM Assets-verwerkingsprofielen definieert en toewijst aan mappen en elementen uploadt naar AEM met de Npm CLI-module 'aem-upload'.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> probeert uit activa beheer </span>
            </a>
        </td>
    </tr>
</table>
