---
title: Een configuratie met een Cloud Service voor tags maken in AEM Sites
description: Leer hoe te om een configuratie van de Cloud Service van markeringen in AEM tot stand te brengen.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# Een configuratie voor de Cloud Service van tags maken in AEM {#create-launch-cloud-service}

Leer hoe u een configuratie van een Cloud Service voor tags maakt in Adobe Experience Manager. AEM configuratie van de Cloud Service van labels kan dan worden toegepast op een bestaande site en de tagbibliotheken kunnen worden waargenomen tijdens het laden in zowel de auteur- als de Publish-omgeving.

## Cloud-service voor tags maken

Maak de configuratie van de cloudservice met onderstaande stappen.

1. Van het **menu van Hulpmiddelen**, selecteer **Cloud Servicen** sectie en klik **de Configuraties van de Lancering van de Adobe**
1. Selecteer de configuratiemap van uw plaats of selecteer **Plaats WKND** (als het gebruiken van het WKND gids project) en klik **creeer**
1. Van het _Algemene_ lusje, noem uw configuratie gebruikend het **gebied van de Titel**, en selecteer **Lancering van de Adobe** van _Bijbehorende de Configuratie van Adobe IMS_ dropdown. Dan, selecteer uw bedrijfsnaam van _bedrijf_ dropdown en selecteer eerder gecreeerd bezit van het _Bezit_ dropdown.
1. Van het _Staging_ en _Productie_ lusje houden de standaardconfiguraties. Nochtans wordt het geadviseerd het herzien en de configuraties voor echte productie opstelling, specifiek de _wisselaar van de Lading Asynchroon_ schakelen die op uw prestaties en optimaliseringsvereisten wordt gebaseerd. Merk ook op dat de _waarde van 0} Bibliotheek URI {voor het Opvoeren en de Productie verschillend is._
1. Tot slot klik **creeer** om de diensten van de etiketwolk te voltooien.

   ![ de Configuratie van Cloud Servicen van markeringen ](assets/launch-cloud-services-config.png)

## Cloudservice voor tags toepassen op de site

Als u de eigenschap Tag en de bijbehorende bibliotheken op de AEM site wilt laden, wordt de configuratie van de cloudservice op de site toegepast. In de vorige stap wordt de configuratie van de cloudservice gemaakt onder de map met sitenaam (WKND-site), zodat deze automatisch moet worden toegepast. Laten we controleren of de configuratie niet werkt.

1. Van het **menu van de Navigatie**, uitgezochte **het pictogram van Plaatsen**.

1. Selecteer de wortelpagina van de AEM Plaats, en klik **Eigenschappen**. Dan, navigeer aan het **Geavanceerde** lusje en onder **de sectie van de Configuratie**, verifieer dat de waarde van de Configuratie van de Wolk aan uw plaats-specifieke `conf` omslag richt.

   ![ pas Configuratie van Cloud Servicen op Plaats ](assets/apply-cloud-services-config-to-site.png) toe

## Laden van eigenschap Tag op pagina&#39;s van Auteur en Publish controleren

Nu is het tijd om te controleren dat de eigenschap Tag en de bijbehorende bibliotheken op de AEM sitepagina worden geladen.

1. Open uw favoriete plaatspagina in de **Mening zoals Gepubliceerde** wijze, in de browser console u het logboekbericht zou moeten zien. Het is het zelfde bericht van het de codefragment van JavaScript van de het bezitsregel van de Markering die in brand wordt gestoken wanneer _Geladen Bibliotheek (de Boven van de Pagina)_ gebeurtenis wordt teweeggebracht.

1. Om op Publish te verifiëren, publiceer eerst uw **configuratie van de de wolkendienst van markeringen** en open de plaatspagina op de instantie van Publish.

   ![ Bezit van de Markering op de Pagina&#39;s van de Auteur en van Publish ](assets/tag-property-on-author-publish-pages.png)

Gefeliciteerd! U hebt AEM en gegevensverzameling integratie van de Tag voltooid die de code van JavaScript in uw AEM plaats injecteert zonder de code van het AEM Project bij te werken.

## Uitdaging - regel bijwerken en publiceren in eigenschap Tag

De lessen van het gebruik die van vorige [ worden geleerd leiden tot een Bezit van de Markering ](./create-tag-property.md) om de eenvoudige uitdaging te voltooien, werk de bestaande Regel bij om extra consoleverklaring toe te voegen en het gebruiken van _het Publiceren Stroom_ stelt het op de AEM plaats op.

## Volgende stappen

[Fouten opsporen in een implementatie van tags](debug-tags-implementation.md)
