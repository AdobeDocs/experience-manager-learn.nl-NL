---
title: AEM Assets Microservices en overstappen op AEM as a Cloud Service
description: Leer hoe u met de asset compute-microservices van AEM Assets as a Cloud Service automatisch en efficiënt elke uitvoering voor uw middelen kunt genereren en deze rol van de traditionele AEM-workflow kunt vervangen.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
duration: 973
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# AEM Assets Microservices - Verplaatsen naar AEM as a Cloud Service

Leer hoe u met de asset compute-microservices van AEM Assets as a Cloud Service automatisch en efficiënt elke uitvoering voor uw middelen kunt genereren en deze rol van de traditionele AEM-workflow kunt vervangen.

>[!VIDEO](https://video.tv.adobe.com/v/336990?quality=12&learn=on)

## Workflow-migratiehulpprogramma

![Hulpprogramma voor migratie van workflows](./assets/asset-workflow-migration.png)

Als deel van het refactoring van uw codebasis, gebruik [Hulpprogramma voor workflowmigratie](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html) bestaande workflows migreren om de Asset compute microservices in AEM as a Cloud Service te gebruiken.

## Belangrijkste activiteiten

+ Gebruik de [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) hulpmiddel om werkstromen voor middelenverwerking te migreren voor het gebruik van de Asset compute microservices.
+ Een [plaatselijke ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) en implementeer de bijgewerkte workflows. Mogelijk is een handmatige aanpassing nodig voor complexe workflows.
+ Doorgaan met herhalen in een lokale ontwikkelomgeving met de AEM SDK totdat de bijgewerkte workflow overeenkomt met de pariteit van de functie.
+ Implementeer de bijgewerkte codebasis in een AEM as a Cloud Service ontwikkelomgeving en blijf valideren.

## Handbeweging

Pas je kennis toe door uit te proberen wat je geleerd hebt met deze praktische oefening.

Voordat u de praktische oefening probeert, moet u controleren of u de bovenstaande video en de volgende materialen hebt bekeken en begrepen:

+ [ anders denken over AEM as a Cloud Service](./introduction.md)
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
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Elementenbeheer uitproberen</span>
            </a>
        </td>
    </tr>
</table>
