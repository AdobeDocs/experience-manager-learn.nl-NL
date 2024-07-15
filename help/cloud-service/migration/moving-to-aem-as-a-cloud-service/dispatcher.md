---
title: Dispatcher configureren bij overgang naar AEM as a Cloud Service
description: Meer informatie over belangrijke wijzigingen in AEM Dispatcher for AEM as a Cloud Service, het Dispatcher-conversieprogramma en het gebruik van de Dispatcher Tools SDK.
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

Meer informatie over AEM Dispatcher for AEM as a Cloud Service, waarbij u zich richt op opmerkelijke wijzigingen van Dispatcher voor AEM 6, het Dispatcher-conversieprogramma en het gebruik van de SDK van Dispatcher Tools.

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Dispatcher Converter

![Dispatcher Converter](./assets/dispatcher-converter-diagram.png)

Als deel van het refactoring van uw codebasis, gebruik [ AEM de Convertor van Dispatcher ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) om bestaande configuraties op-gebouw of van de Adobe Managed Services Dispatcher aan AEM as a Cloud Service compatibele configuratie van Dispatcher te bekrachtigen.

## Belangrijkste activiteiten

+ Gebruik het [ hulpmiddel van de Convertor van Adobe I/O Dispatcher ](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) om een bestaande configuratie van Dispatcher te migreren.
+ Verwijs de module van Dispatcher van [ AEM Archetype van het Project ](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) als beste praktijken.
+ [ Opstelling lokale Hulpmiddelen van Dispatcher ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) om verzender te bevestigen, alvorens in een milieu van de Cloud Service te testen.

## Handbeweging

Pas je kennis toe door uit te proberen wat je geleerd hebt met deze praktische oefening.

Voordat u de praktische oefening probeert, moet u controleren of u de bovenstaande video en de volgende materialen hebt bekeken en begrepen:

+ [AEM-moderniseringstools](./aem-modernization-tools.md)
+ [Onboarding](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

Zorg er ook voor dat u de vorige hands-on oefening hebt uitgevoerd:

+ [Cloud Manager hands-on oefening](./cloud-manager.md#hands-on-exercise)

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
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> probeert uit Dispatcher Hulpmiddelen </span>
            </a>
        </td>
    </tr>
</table>
