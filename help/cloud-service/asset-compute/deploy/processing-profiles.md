---
title: Asset compute-workers integreren met AEM verwerkingsprofielen
description: AEM als Cloud Service integreert met de arbeiders van de Asset compute die aan Adobe I/O Runtime via AEM Assets-verwerkingsprofielen worden opgesteld. Verwerkingsprofielen worden geconfigureerd in de service Auteur om specifieke elementen te verwerken met behulp van aangepaste workers en de bestanden die door de workers worden gegenereerd, op te slaan als elementuitvoeringen.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 1%

---


# Integreren met AEM verwerkingsprofielen

Workers van Asset computen kunnen alleen aangepaste uitvoeringen in AEM als Cloud Service genereren als deze in AEM zijn geregistreerd als service Auteur van Cloud Service via het verwerken van profielen. Op alle elementen die onder dat verwerkingsprofiel vallen, wordt de worker geactiveerd tijdens het uploaden of opnieuw verwerken en wordt de aangepaste uitvoering gegenereerd en beschikbaar gesteld via de uitvoeringen van het element.

## Een verwerkingsprofiel definiëren

Maak eerst een nieuw verwerkingsprofiel dat de worker aanroept met de configureerbare parameters.

![Profiel verwerken](./assets/processing-profiles/new-processing-profile.png)

1. Meld u aan bij AEM als een Cloud Service Auteur-service als een __AEM Beheerder__. Aangezien dit een leerprogramma is adviseren wij het gebruiken van een ontwikkelomgeving of een milieu in een Sandbox.
1. Navigeer naar __Gereedschappen > Middelen > Profielen verwerken__
1. Tik __Maken__
1. Geef het verwerkingsprofiel een naam, `WKND Asset Renditions`
1. Tik op de tab __Aangepast__ en tik __Nieuw__ toevoegen
1. De nieuwe service definiëren
   + __Naam van vertoning:__ `Circle`
      + De bestandsnaamuitvoering die wordt gebruikt om deze vertoning in AEM Assets te identificeren
   + __Extensie:__ `png`
      + De extensie van de vertoning die wordt gegenereerd. Stel dit in op `png` omdat dit de ondersteunde uitvoerindeling is die door de webservice van de worker wordt ondersteund. Dit resulteert in een transparante achtergrond achter de uitgesneden cirkel.
   + __Eindpunt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dit is de URL naar de worker die via `aio app get-url` is verkregen. Zorg ervoor dat de URL-punten zich in de juiste werkruimte bevinden, op basis van de AEM als een Cloud Service-omgeving.
      + Controleer of de URL van de worker naar de juiste werkruimte verwijst. AEM als werkgebied van de Cloud Service zou werkruimte URL van het Stadium moeten gebruiken, en AEM als Productie zou de werkruimte URL van de Productie moeten gebruiken.
   + __Serviceparameters__
      + Tik __Parameter toevoegen__
         + Sleutel: `size`
         + Waarde: `1000`
      + Tik __Parameter toevoegen__
         + Sleutel: `contrast`
         + Waarde: `0.25`
      + Tik __Parameter toevoegen__
         + Sleutel: `brightness`
         + Waarde: `0.10`
      + Deze sleutel-/waardeparen die worden doorgegeven aan de Asset compute-worker en beschikbaar via JavaScript-object `rendition.instructions`.
   + __MIME-typen__
      + __Omvat:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Deze MIME-typen zijn de enige typen van de npm-modules van de worker. In deze lijst wordt beperkt welke elementen door de aangepaste worker worden verwerkt.
      + __Omvat niet:__ `Leave blank`
         + Verwerk nooit activa met deze Types MIME gebruikend deze de dienstconfiguratie. In dit geval gebruiken we alleen een lijst van gewenste personen.
1. Tik __Opslaan__ rechtsboven

## Een verwerkingsprofiel toepassen en aanroepen

1. Selecteer het nieuwe verwerkingsprofiel `WKND Asset Renditions`
1. Tik __Profiel toepassen op map(pen)__ in de bovenste actiebalk
1. Selecteer een map waarop u het verwerkingsprofiel wilt toepassen, zoals `WKND` en tik __Toepassen__
1. Navigeer naar de map waarop het verwerkingsprofiel niet is toegepast via __AEM > Middelen > Bestanden__ en tik op `WKND`.
1. Upload enkele nieuwe afbeeldingselementen ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) en [sample-3.jpg](../assets/samples/sample-3.jpg)) naar een willekeurige map in de map met het verwerkingsprofiel toegepast en wacht tot het geüploade element wordt verwerkt.
1. Tik op het element om de details te openen
   + Standaarduitvoeringen worden mogelijk sneller gegenereerd en weergegeven in AEM dan aangepaste uitvoeringen.
1. Open de weergave __Uitvoeringen__ vanuit de linkerzijbalk
1. Tik op het element met de naam `Circle.png` en bekijk de gegenereerde vertoning

   ![Gegenereerde uitvoering](./assets/processing-profiles/rendition.png)

## Voltooid!

Gefeliciteerd! U hebt [tutorial](../overview.md) over hoe te om AEM als de microservices van de Asset compute van de Cloud Service uit te breiden gebeëindigd! U zou nu de capaciteit moeten hebben om de arbeiders van de douaneAsset compute voor gebruik door uw AEM als dienst van de Auteur van de Cloud Service op te zetten, te ontwikkelen, te testen, te zuiveren en op te stellen.

### Bekijk de volledige broncode van het project op Github

Het definitieve project voor de Asset compute is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github bevat de definitieve staat van het project, volledig bevolkt met de arbeider en testgevallen, maar bevat geen geloofsbrieven, d.w.z. `.env`,  `.config.json` of  `.aio`._

## Problemen oplossen

+ [Aangepaste uitvoering ontbreekt in element in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Verwerking van middelen mislukt in AEM](../troubleshooting.md#asset-processing-fails)
