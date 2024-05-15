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

1. Navigeer in de browser naar de knop [Adobe Experience Cloud Home](https://experience.adobe.com/) pagina en login die u Adobe ID gebruiken.

1. Klik op de knop **Gegevensverzameling** van de _Snelle toegang_ van de startpagina van Adobe Experience Cloud.

1. Klik op de knop **Tags** menu-item in de linkernavigatie en klik vervolgens op **Nieuwe eigenschap** in de rechterbovenhoek.

1. Geef de eigenschap Tag een naam met de opdracht **Naam** vereist veld. Voer bij Domeinen uw domeinnaam in of voer AEM as a Cloud Service omgeving in `adobeaemcloud.com` en klik op **Opslaan**.

   ![Eigenschappen van label](assets/tag-properties.png)

## Een nieuwe regel maken

Open de nieuwe eigenschap Tag door op de naam ervan te klikken in het dialoogvenster **Eigenschappen van label** weergeven. ook onder _Mijn recente activiteiten_ in de kop ziet u dat de Core-extensie eraan is toegevoegd. De extensie van de tag Core is de standaardextensie en biedt basisgebeurtenistypen zoals page-load, browser, formulier en andere gebeurtenistypen, zie [Overzicht van kernextensies](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) voor meer informatie .

Met regels kunt u opgeven wat er moet gebeuren als de bezoeker communiceert met uw AEM site. Om dingen eenvoudig te houden, laten wij twee berichten aan de browser console registreren om aan te tonen hoe de integratie van de Markering van de gegevensverzameling JavaScript code in uw AEM plaats kan injecteren zonder AEM de code van het Project bij te werken.

Voer de volgende stappen uit om een regel te maken.

1. Klikken **Regels** van de _AUTHORING_ van de linkernavigatie en klik dan **Nieuwe regel maken**

1. Noem uw regel gebruikend **Naam** vereist veld.

1. Klikken **Toevoegen** van de _EVENTS_ en vervolgens in de _Gebeurtenisconfiguratie_ in het **Type gebeurtenis** vervolgkeuzelijst selecteren _Bibliotheek geladen (pagina boven)_ en klik op **Wijzigingen behouden**.

1. Klikken **Toevoegen** van de _ACTIES_ en vervolgens in de _Configuratie van handelingen_ in het **Type handeling** vervolgkeuzelijst selecteren _Aangepaste code_ en klik op **Editor openen**.

1. In de _Code bewerken_ modal, voer het volgende JavaScript-codefragment in en klik op **Opslaan** en klik op **Wijzigingen behouden**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Klikken **Opslaan** om het proces voor het maken van regels te voltooien.

   ![Nieuwe regel](assets/new-rule.png)

## Bibliotheek toevoegen en publiceren

De eigenschap Tag _Regels_ worden geactiveerd met een bibliotheek. U kunt de bibliotheek zien als een pakket met JavaScript-code. Activeer de nieuwe regel door de stappen te volgen.

1. Klikken **Publishing Flow** van de _PUBLICEREN_ van de linkernavigatie en klik vervolgens op **Bibliotheek toevoegen**

1. Geef uw bibliotheek een naam met de **Naam** veld en selecteer _Ontwikkeling (ontwikkeling)_ optie voor **Omgeving** vervolgkeuzelijst.

1. Als u alle gewijzigde bronnen wilt selecteren sinds het maken van de eigenschap Tag, klikt u **+ Alle gewijzigde bronnen toevoegen**. Deze actie voegt de pas gecreëerde regel en de bron van de kernuitbreiding aan de bibliotheek toe. Klik op Tot slot **Opslaan en samenstellen tot ontwikkeling**.

1. Nadat de bibliotheek voor de **Ontwikkeling** swim lane, gebruiken _ovalen_ Selecteer de **Ter goedkeuring verzenden**

1. Dan in **Verzonden** zwembaan met _ovalen_ Selecteer de **Goedkeuren voor publicatie**, eveneens **Samenstellen en publiceren naar productie** in de **Goedgekeurd** zwembaan.

![Gepubliceerde bibliotheek](assets/published-library.png)


Met de bovenstaande stap wordt het maken van de eenvoudige eigenschap Tag met een regel voltooid voor het vastleggen van een bericht aan de browserconsole wanneer de pagina wordt geladen. De regel- en kernextensie worden ook gepubliceerd door een bibliotheek te maken.

## Volgende stappen

[AEM verbinden met eigenschap Tag met IMS](connect-aem-tag-property-using-ims.md)


## Aanvullende bronnen {#additional-resources}

* [Een eigenschap voor een tag maken](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
