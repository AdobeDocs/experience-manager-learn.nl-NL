---
title: Een configuratie voor Cloud Servicen starten in AEM Sites maken
description: Leer hoe te om een configuratie van de Cloud Service van de Lancering in AEM tot stand te brengen. De configuratie van de Cloud Service Launch kan dan worden toegepast op een bestaande Site en de tagbibliotheken kunnen worden waargenomen tijdens het laden in zowel de auteur- als de publicatieomgeving.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 0%

---

# Een configuratie van de Cloud Service Launch maken in AEM {#create-launch-cloud-service}

>[!NOTE]
>
>Het proces om Adobe Experience Platform Launch als reeks technologieën van de gegevensinzameling anders te noemen wordt uitgevoerd in AEM product UI, inhoud, en documentatie, zodat wordt de term Lanceren nog gebruikt hier.

Leer hoe u een configuratie voor een Cloud Service starten maakt in Adobe Experience Manager. AEM de configuratie van de Cloud Service van de Lancering kan dan op een bestaande Plaats worden toegepast en de bibliotheken van Markeringen kunnen ladend in zowel auteur als Publish milieu&#39;s worden waargenomen.

## Launch Cloud Service maken

Maak de configuratie van de cloudservice starten met onderstaande stappen.

1. Van de **Gereedschappen** menu, selecteert u **Cloud Services** en klik op **Adobe-startconfiguraties**

1. Selecteer de configuratiemap van uw site of selecteer **WKND-site** (als het gebruiken van het WKND gids project) en klik **Maken**

1. Van de _Algemeen_ tab, noem uw configuratie gebruikend **Titel** en selecteer **Adobe starten** van de _Gekoppelde Adobe IMS-configuratie_ vervolgkeuzelijst. Selecteer vervolgens uw bedrijfsnaam in het menu _Bedrijf_ vervolgkeuzelijst en selecteer eerder gemaakte eigenschap in het menu _Eigenschap_ vervolgkeuzelijst.

1. Van de _Staging_ en _Productie_ de standaardconfiguraties behouden. Nochtans wordt het geadviseerd het herzien en de configuraties voor echte productie opstelling, specifiek te veranderen _Bibliotheek asynchroon laden_ schakelen op basis van uw prestatie- en optimalisatievereisten. Let ook op het volgende: _Bibliotheek-URI_ De waarde is anders voor Staging en Productie.

1. Tot slot klikt u op **Maken** om de Launch Cloud Services te voltooien.

   ![Configuratie van Cloud Services starten](assets/launch-cloud-services-config.png)

## Cloudservice starten toepassen op de site

Als u de eigenschap Tag en de bijbehorende bibliotheken op de AEM site wilt laden, wordt de configuratie van de cloudservice starten toegepast op de site. In de vorige stap wordt de configuratie van de cloudservice gemaakt onder de map met sitenaam (WKND-site), zodat deze automatisch moet worden toegepast. Laten we controleren of de configuratie niet werkt.

1. Van de **Navigatie** menu, selecteert u **Sites** pictogram.

1. Selecteer de hoofdpagina van de AEM Site en klik op **Eigenschappen**. Navigeer vervolgens naar de **Geavanceerd** tab en onder **Configuratie** sectie, controleert u of de waarde voor Cloud Configuration naar uw sitespecifieke `conf` map.

   ![Configuratie van Cloud Services toepassen op de site](assets/apply-cloud-services-config-to-site.png)

## Het laden van de eigenschap Tag op de pagina&#39;s Auteur en Publiceren controleren

Nu is het tijd om te controleren dat de eigenschap Tag en de bijbehorende bibliotheken op de AEM sitepagina worden geladen.

1. Open uw favoriete sitepagina in het dialoogvenster **Weergeven als gepubliceerd** in de browserconsole ziet u het logbericht. Het is hetzelfde bericht uit het JavaScript-codefragment van de tageigenschapregel dat wordt geactiveerd wanneer _Bibliotheek geladen (pagina boven)_ gebeurtenis wordt geactiveerd.

1. Als u wilt controleren bij Publiceren, publiceert u eerst uw **Cloudservice starten** en opent u de sitepagina in de instantie Publiceren.

   ![Eigenschap labelen op auteur- en publicatiepagina&#39;s](assets/tag-property-on-author-publish-pages.png)

Gefeliciteerd! U hebt AEM en gegevensverzameling integratie van de Tag voltooid die JavaScript-code in uw AEM-site injecteert zonder de AEM projectcode bij te werken.

## Uitdaging - regel bijwerken en publiceren in eigenschap Tag

De lessen gebruiken die u hebt geleerd van het vorige [Een eigenschap voor een tag maken](./create-tag-property.md) om de eenvoudige uitdaging te voltooien, werk de bestaande Regel bij om extra consoleverklaring toe te voegen en het gebruiken _Publishing Flow_ implementeren op de AEM.

## Volgende stappen

[Fouten opsporen in een implementatie van tags](debug-tags-implementation.md)
