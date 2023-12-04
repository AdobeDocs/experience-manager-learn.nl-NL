---
title: Opstelling uw project BPA en CAM
description: Leer hoe de Analysator van Beste praktijken en de Manager van de Versnelling van de Wolk een aangepaste gids voor het migreren aan AEM as a Cloud Service verstrekt.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
duration: 717
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 2%

---

# Best Practices Analyzer en Cloud Acceleration Manager

Leer hoe de Analysator van Beste praktijken (BPA) en de Manager van de Versnelling van de Wolk (CAM) een aangepaste gids voor het migreren aan AEM as a Cloud Service verstrekt. 

>[!VIDEO](https://video.tv.adobe.com/v/336957?quality=12&learn=on)

## BPA en CAM gebruiken

![BPA- en CAM-diagram op hoog niveau](assets/bpa-cam-diagram.png)

Het BPA-pakket moet worden geïnstalleerd op een kloon van de AEM 6.x-omgeving. De BPA zal een rapport produceren dat dan in CAM kan worden geupload, dat begeleiding in de belangrijkste activiteiten zal verstrekken die moeten plaatsvinden om zich naar AEM as a Cloud Service te bewegen.

## Belangrijkste activiteiten

+ Maak een kloon van uw productie 6.x milieu. Tijdens het migreren van inhoud en refactorcode is het van belang een kloon van een productieomgeving te hebben om verschillende gereedschappen en wijzigingen te testen.
+ Download het nieuwste BPA-programma van de [Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) en installeer op uw AEM 6.x gekloonde omgeving.
+ Gebruik het hulpmiddel BPA om een rapport te produceren dat aan de Manager van de Versnelling van de Wolk (CAM) kan worden geupload. CAM is toegankelijk via [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ Gebruik CAM om richtlijnen te geven voor de updates die moeten worden uitgevoerd in de huidige codebasis en -omgeving om naar AEM as a Cloud Service te gaan.

## Handbeweging

Pas je kennis toe door uit te proberen wat je geleerd hebt met deze praktische oefening.

Voordat u de praktische oefening probeert, moet u controleren of u de bovenstaande video en de volgende materialen hebt bekeken en begrepen:

+ [ anders denken over AEM as a Cloud Service](./introduction.md)
+ [Wat is AEM as a Cloud Service?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Architectuur van AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [Meerdere en onveranderlijke inhoud](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [Verschillen in ontwikkeling voor AEM as a Cloud Service en AEM 6.x](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="Hands-on opslagplaats van GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Hands-on met de Analyzer van Beste praktijken</div>
            <p style="margin:1rem 0">
                Onderzoek de Analysator van Beste praktijken (BPA) en het herzien van de resultaten door het tegen een erfenisWKND codebasis in werking te stellen die voorbeeldschendingen bevat.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Analysator van best practices uitproberen</span>
            </a>
        </td>
    </tr>
</table>


## Overige middelen

+ [Download de Analyzer van Beste praktijken](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)