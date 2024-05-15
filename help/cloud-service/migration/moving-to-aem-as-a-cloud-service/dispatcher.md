---
title: Dispatcher configureren bij overgang naar AEM as a Cloud Service
description: Meer informatie over opmerkelijke wijzigingen in AEM Dispatcher voor AEM as a Cloud Service, het Dispatcher-conversieprogramma en het gebruik van de Dispatcher Tools SDK.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
duration: 1618
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 2%

---


# Dispatcher

Leer over AEM Dispatcher voor AEM as a Cloud Service, zich op opmerkelijke veranderingen van Dispatcher voor AEM 6, het hulpmiddel van de Omzetting van de Verzender en hoe te om de Dispatcher Tools SDK te gebruiken.

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Als deel van het refactoring van uw codebasis, gebruik [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) bestaande Managed Services Dispatcher-configuraties op locatie of Adobe te vernieuwen om de as a Cloud Service compatibele Dispatcher-configuratie te AEM.

## Belangrijkste activiteiten

+ Gebruik de [Adobe I/O Dispatcher Converter, gereedschap](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) om een bestaande Dispatcher-configuratie te migreren.
+ Verwijs naar de module Dispatcher van de [Projectarchetype AEM](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) als beste praktijk.
+ [Lokale gereedschappen voor Dispatcher instellen](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) om de verzender te valideren, voordat u gaat testen in een Cloud Service-omgeving.

## Handbeweging

Pas je kennis toe door uit te proberen wat je geleerd hebt met deze praktische oefening.

Voordat u de praktische oefening probeert, moet u controleren of u de bovenstaande video en de volgende materialen hebt bekeken en begrepen:

+ [AEM-moderniseringstools](./aem-modernization-tools.md)
+ [Onboarding](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

Zorg er ook voor dat u de vorige hands-on oefening hebt uitgevoerd:

+ [Praktische oefening van Cloud Manager](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="Hands-on opslagplaats van GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Handmatig aan met Dispatcher Tools</div>
            <p style="margin:1rem 0">
                Ontdek het gebruik van de Dispatcher Tools van de AEM SDK om Dispatcher-configuraties te valideren en AEM Dispatcher lokaal uit te voeren met Docker.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Verzendprogramma's uitproberen</span>
            </a>
        </td>
    </tr>
</table>
