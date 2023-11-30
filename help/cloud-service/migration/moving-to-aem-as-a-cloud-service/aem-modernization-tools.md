---
title: Het gebruiken van AEM Moderniseringshulpmiddelen om zich aan AEM as a Cloud Service te bewegen
description: Leer hoe AEM Moderniseringshulpmiddelen worden gebruikt om een bestaand AEM project en inhoud te bevorderen om as a Cloud Service compatibel AEM te zijn.
version: Cloud Service
topic: Migration, Upgrade
feature: Migration
role: Developer
level: Experienced
jira: KT-8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 1%

---


# AEM-moderniseringstools

Leer hoe AEM Moderniseringshulpmiddelen worden gebruikt om een bestaande inhoud van AEM Sites te bevorderen om as a Cloud Service compatibel AEM te zijn en zich aan beste praktijken te richten.

## All-in-One converter

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## Paginaconversie

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## Componentconversie

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## Beleidsimport

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## AEM moderniseringsgereedschappen gebruiken

![Levenscyclus AEM moderniseringsgereedschappen](./assets/aem-modernization-tools.png)

AEM de hulpmiddelen van de Modernisering zetten automatisch bestaande AEM Pagina&#39;s om die uit erfenis statische malplaatjes, stichtingscomponenten, en parsys worden samengesteld - om moderne benaderingen zoals editable malplaatjes, AEM de Componenten van de Kern WCM, en de Containers van de Lay-out te gebruiken.

## Belangrijkste activiteiten

+ Klonen AEM 6.x productie om AEM Moderniseringshulpmiddelen tegen te werken
+ Download en installeer de [nieuwste AEM moderniseringsgereedschappen](https://github.com/adobe/aem-modernize-tools/releases/latest) op de AEM 6.x productiekleon via Package Manager

+ [Paginastructuurconverter](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) Hiermee wordt bestaande pagina-inhoud van een statische sjabloon bijgewerkt naar een toegewezen bewerkbare sjabloon met behulp van lay-outcontainers
   + Conversieregels definiëren met OSGi-configuratie
   + Paginastructuurconverter uitvoeren op bestaande pagina&#39;s

+ [Componentconversie](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) Hiermee wordt bestaande pagina-inhoud van een statische sjabloon bijgewerkt naar een toegewezen bewerkbare sjabloon met behulp van lay-outcontainers
   + Conversieregels definiëren via definities van JCR-knooppunten/XML
   + Het gereedschap Componentconversie uitvoeren op bestaande pagina&#39;s

+ [Beleidsimporteur](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) creeert beleid van de configuratie van het Ontwerp
   + Conversieregels definiëren met behulp van JCR-knooppuntdefinities/XML
   + Beleidsimporteur uitvoeren op basis van bestaande ontwerpdefinities
   + Geïmporteerd beleid toepassen op AEM componenten en containers

## Handbeweging

Pas je kennis toe door uit te proberen wat je geleerd hebt met deze praktische oefening.

Voordat u de praktische oefening probeert, moet u controleren of u de bovenstaande video en de volgende materialen hebt bekeken en begrepen:

+ [ anders denken over AEM as a Cloud Service](./introduction.md)
+ [Modernisering opslagplaats](./repository-modernization.md)
+ [Meerdere en onveranderlijke inhoud](../../developing/basics/mutable-immutable.md)
+ [AEM projectstructuur](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html)

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
            <div style="font-size:1.25rem;font-weight:400;">Hands-on met AEM modernisering</div>
            <p style="margin:1rem 0">
                Ontdek het gebruiken AEM Moderniseringshulpmiddelen om een erfenisWKND plaats bij te werken om met AEM as a Cloud Service beste praktijken in overeenstemming te brengen.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Probeer de AEM moderniseringsgereedschappen uit</span>
            </a>
        </td>
    </tr>
</table>

## Overige middelen

+ [Gereedschappen voor AEM downloaden](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM documentatie over moderniseringsgereedschappen](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - Introductie van de AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. Implementeer de net gemoderniseerde wknd-erfenissite op de lokale AEM SDK. AEM ASK is hier beschikbaar voor downloaden:
   + [Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
