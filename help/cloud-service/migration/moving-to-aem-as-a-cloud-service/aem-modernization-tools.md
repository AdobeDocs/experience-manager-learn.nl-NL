---
title: Het gebruiken van AEM Moderniseringshulpmiddelen om zich aan AEM as a Cloud Service te bewegen
description: Leer hoe AEM Moderniseringshulpmiddelen worden gebruikt om een bestaand AEM project en inhoud te bevorderen om as a Cloud Service compatibel AEM te zijn.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 1%

---


# AEM-moderniseringstools

Leer hoe AEM Moderniseringshulpmiddelen worden gebruikt om een bestaande inhoud van AEM Sites te bevorderen om as a Cloud Service compatibel AEM te zijn en zich aan beste praktijken te richten.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## AEM moderniseringsgereedschappen gebruiken

![Levenscyclus AEM moderniseringsgereedschappen](./assets/aem-modernization-tools.png)

AEM de hulpmiddelen van de Modernisering zetten automatisch bestaande AEM Pagina&#39;s om die uit erfenis statische malplaatjes, stichtingscomponenten, en parsys worden samengesteld - om moderne benaderingen zoals editable malplaatjes, AEM de Componenten van de Kern WCM, en de Containers van de Lay-out te gebruiken.

### Belangrijkste activiteiten

+ Klonen AEM 6.x productie om AEM Moderniseringshulpmiddelen tegen te werken
+ Download en installeer de [nieuwste AEM moderniseringsgereedschappen](https://github.com/adobe/aem-modernize-tools/releases/latest) op de AEM 6.x productiekleon via Package Manager

+ [Paginastructuurconverter](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) Hiermee wordt bestaande pagina-inhoud van een statische sjabloon bijgewerkt naar een toegewezen bewerkbare sjabloon met behulp van lay-outcontainers
   + Conversieregels definiëren met OSGi-configuratie
   + Paginastructuurconverter uitvoeren op bestaande pagina&#39;s

+ [Component Converter](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) Hiermee wordt bestaande pagina-inhoud van een statische sjabloon bijgewerkt naar een toegewezen bewerkbare sjabloon met behulp van lay-outcontainers
   + Conversieregels definiëren via JCR-knoopdefinities/XML
   + Het gereedschap Componentconversie uitvoeren op bestaande pagina&#39;s

+ [Beleidsimporteur](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) creeert beleid van de configuratie van het Ontwerp
   + Conversieregels definiëren met behulp van JCR-knooppuntdefinities/XML
   + Beleidsimporteur uitvoeren op basis van bestaande ontwerpdefinities
   + Geïmporteerd beleid toepassen op AEM componenten en containers

+ [Dialoogconversie](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) converteert op Classic (ExtJS) en CoralUI 2 gebaseerde componentdialoogvensters naar op CoralUI 3 TouchUI gebaseerde dialoogvensters.
   + Het gereedschap Dialoogconverter uitvoeren op basis van bestaande dialoogvensters die zijn gebaseerd op ExtJS of Coral2
   + Omgezette dialoogvensters weer synchroniseren naar Git-opslagplaats

### Overige bronnen

+ [Gereedschappen voor AEM downloaden](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM documentatie over moderniseringsgereedschappen](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - Introductie van de AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
