---
title: Een eigenschap voor een tag maken
description: Leer hoe te om een bezit van de Markering met de kale-minimumconfiguratie tot stand te brengen om met AEM te integreren. Gebruikers worden nu toegevoegd aan de interface van de tag en leren meer over extensies, regels en publicatieworkflows.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 606
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# Een eigenschap voor een tag maken {#create-tag-property}

Leer hoe te om een bezit van de Markering met de kale-minimumconfiguratie tot stand te brengen om met Adobe Experience Manager te integreren. Gebruikers worden nu toegevoegd aan de interface van de tag en leren meer over extensies, regels en publicatieworkflows.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Tageigenschap maken

Voer de volgende stappen uit om een eigenschap Tag te maken.

1. In browser, navigeer aan het [&#x200B; Huis van Adobe Experience Cloud &#x200B;](https://experience.adobe.com/) pagina en login gebruikend u Adobe ID.

1. Klik de **toepassing van de Inzameling van 0&rbrace; Gegevens van de _Snelle toegang_ sectie van de Homepage van Adobe Experience Cloud.**

1. Klik het **het menupunt van Markeringen** van de linkernavigatie, dan klik **Nieuw Bezit** van top-right hoek.

1. Noem uw bezit van de Markering gebruikend het **Naam** vereiste gebied. Voor het gebied van Domeinen, ga uw domeinnaam in of als het gebruiken van het milieu van AEM as a Cloud Service `adobeaemcloud.com` ingaat en **sparen** klikt.

   ![&#x200B; Eigenschappen van de Markering &#x200B;](assets/tag-properties.png)

## Een nieuwe regel maken

Open het onlangs gecreeerde bezit van de Markering door zijn naam in de **mening van de Eigenschappen van de Markering** te klikken. Ook onder _Mijn Recente rubriek van de Activiteit_ zou u moeten zien dat de uitbreiding van de Kern aan het werd toegevoegd. De de markeringsuitbreiding van de Kern is de standaarduitbreiding en het verstrekt fundamentele gebeurtenistypen zoals pagina-lading, browser, vorm, en andere gebeurtenistypen, zie [&#x200B; de uitbreidingsoverzicht van de Kern &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html?lang=nl-NL) voor meer informatie.

Met regels kunt u opgeven wat er moet gebeuren als de bezoeker communiceert met uw AEM site. Om dingen eenvoudig te houden, laten wij twee berichten aan de browser console registreren om aan te tonen hoe de integratie van de Markering van de gegevensverzameling JavaScript code in uw AEM plaats kan injecteren zonder AEM de code van het Project bij te werken.

Voer de volgende stappen uit om een regel te maken.

1. Klik **Regels** van de _AUTHORING_ sectie van de linkernavigatie en klik dan **creeer Nieuwe Regel**

1. Noem uw regel gebruikend het **Vereiste gebied van de Naam**.

1. Klik **toevoegen** van de _GEBEURTENISSEN_ sectie, dan in de _vorm van de Configuratie van de Gebeurtenis_, in het **drop-down uitgezochte Type van Gebeurtenis** Bibliotheek 8&rbrace; (de Boven van de Pagina) _optie en klik **houden Veranderingen**._

1. Klik **toevoegen** van de _sectie van Acties_, dan in de _Vorm van de Configuratie van de Actie_, in de **drop-down uitgezochte _optie van het Type van Actie_ en klik** Open Redacteur **.**

1. In _geef Code_ modaal uit, ga na het codefragment van JavaScript in, dan klik **sparen**, en klik definitief **houden Veranderingen**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Klik **sparen** om het proces van de regelverwezenlijking te beëindigen.

   ![&#x200B; Nieuwe Regel &#x200B;](assets/new-rule.png)

## Bibliotheek toevoegen en publiceren

Het bezit van de Markering _Regels_ wordt geactiveerd gebruikend een bibliotheek, denk aan de bibliotheek als pakket dat de code van JavaScript bevat. Activeer de nieuwe regel door de stappen te volgen.

1. Klik **het Publiceren Stroom** van _PUBLICEREN_ sectie van de linkernavigatie, dan klik **toevoegen Bibliotheek**

1. Noem uw bibliotheek gebruikend het **gebied van de Naam 0&rbrace; en selecteer _Ontwikkeling (ontwikkeling)_ optie voor** Milieu **dropdown.**

1. Als u alle gewijzigde bronnen wilt selecteren sinds het maken van de eigenschap Tag, klikt u op **+ Alle gewijzigde bronnen toevoegen** . Deze actie voegt de pas gecreëerde regel en de bron van de kernuitbreiding aan de bibliotheek toe. Tot slot klik **sparen &amp; bouwen aan Ontwikkeling**.

1. Zodra de bibliotheek voor de **ontwikkeling** wordt gebouwd zwemweg, gebruikend _ellipsen_ selecteer **voorleggen voor Goedkeuring**

1. Dan in **Voorgelegde** zwemweg gebruikend _ellipsen_ selecteer **goedkeuren voor het Publiceren**, eveneens **Bouwstijl &amp; Publish aan Productie** in **Goedgekeurde** zwemweg.

![&#x200B; Gepubliceerde bibliotheek &#x200B;](assets/published-library.png)


Met de bovenstaande stap wordt het maken van de eenvoudige eigenschap Tag met een regel voltooid voor het vastleggen van een bericht aan de browserconsole wanneer de pagina wordt geladen. De regel- en kernextensie worden ook gepubliceerd door een bibliotheek te maken.

## Volgende stappen

[AEM verbinden met eigenschap Tag met IMS](connect-aem-tag-property-using-ims.md)


## Aanvullende bronnen {#additional-resources}

* [&#x200B; creeer een Bezit van de Markering &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=nl-NL)
