---
title: AEM-moderniseringsgereedschappen gebruiken voor de overgang naar AEM as a Cloud Service
description: Leer hoe AEM Modernization Tools wordt gebruikt om een bestaand AEM project en inhoud te bevorderen om AEM as a Cloud Service compatibel te zijn.
version: Experience Manager as a Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
duration: 2502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 1%

---


# AEM-moderniseringstools

Leer hoe AEM Modernization Tools wordt gebruikt om een bestaande AEM Sites-inhoud te upgraden zodat deze compatibel is met AEM as a Cloud Service en kan worden uitgelijnd met aanbevolen procedures.

## All-in-One converter

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## Paginaconversie

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## Componentconversie

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## Beleidsimport

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## AEM-moderniseringsgereedschappen gebruiken

![ Levenscyclus van de Hulpmiddelen van de Modernisering van AEM ](./assets/aem-modernization-tools.png)

De hulpmiddelen van de Modernisering van AEM zetten automatisch bestaande Pagina&#39;s van AEM die uit erfenis statische malplaatjes, stichtingscomponenten, en parsys worden samengesteld om moderne benaderingen zoals editable malplaatjes, de Componenten van WCM van de Kern van AEM, en de Containers van de Lay-out te gebruiken.

## Belangrijkste activiteiten

+ Clone AEM 6.x-productie om AEM Moderniseringsgereedschappen uit te voeren tegen
+ Download en installeer de [ recentste hulpmiddelen van de Modernisaties van AEM ](https://github.com/adobe/aem-modernize-tools/releases/latest) op AEM 6.x productiekleon via de Manager van het Pakket

+ [ de Omzetter van de Structuur van de Pagina ](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) werkt bestaande paginainhoud van statisch malplaatje aan een in kaart gebracht editable malplaatje gebruikend lay-outcontainers bij
   + Conversieregels definiëren met OSGi-configuratie
   + Paginastructuurconverter uitvoeren op bestaande pagina&#39;s

+ [&#128279;](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) van de Omzetter van de Component van 0&rbrace; werkt bestaande paginainhoud van statische malplaatje aan een in kaart gebracht editable malplaatje gebruikend lay-outcontainers bij
   + Conversieregels definiëren via definities van JCR-knooppunten/XML
   + Het gereedschap Componentconversie uitvoeren op bestaande pagina&#39;s

+ [ Importeur van het Beleid ](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) leidt tot beleid van de configuratie van het Ontwerp
   + Conversieregels definiëren met behulp van JCR-knooppuntdefinities/XML
   + Beleidsimporteur uitvoeren op basis van bestaande ontwerpdefinities
   + Geïmporteerd beleid toepassen op AEM-componenten en -containers

## Handbeweging

Pas je kennis toe door uit te proberen wat je geleerd hebt met deze praktische oefening.

Voordat u de praktische oefening probeert, moet u controleren of u de bovenstaande video en de volgende materialen hebt bekeken en begrepen:

+ [AEM as a Cloud Service anders denken](./introduction.md)
+ [Modernisering opslagplaats](./repository-modernization.md)
+ [Meerdere en onveranderlijke inhoud](../../developing/basics/mutable-immutable.md)
+ [ het projectstructuur van AEM ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html)

Zorg er ook voor dat u de vorige hands-on oefening hebt uitgevoerd:

+ [BPA en CAM hands-on oefening](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="Hands-on opslagplaats van GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Hands-on met de modernisering van AEM</div>
            <p style="margin:1rem 0">
                Ontdek het gebruik van AEM Modernization Tools om een verouderde WKND-site bij te werken overeenkomstig AEM as a Cloud Service best practices.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> probeert uit de Hulpmiddelen van de Modernisering van AEM </span>
            </a>
        </td>
    </tr>
</table>

## Overige middelen

+ [ de Hulpmiddelen van de Modernisaties van AEM van de Download ](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [ documentatie van Hulpmiddelen van de Modernisering van AEM ](https://opensource.adobe.com/aem-modernize-tools/)
+ [ AEM Gems - Introducerend de Reeks van de Modernisering van AEM ](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. Implementeer de onlangs gemoderniseerde wint-legacy site op de lokale AEM SDK. AEM ASK kan hier worden gedownload:
   + [ Portaal van de Distributie van de Software ](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
