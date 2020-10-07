---
title: Workers voor middelenboekhouding integreren met AEM verwerkingsprofielen
description: AEM als Cloud Service integreert met Asset Compute-workers die via AEM Assets Processing Profiles naar Adobe I/O Runtime worden geïmplementeerd. Verwerkingsprofielen worden geconfigureerd in de service Auteur om specifieke elementen te verwerken met behulp van aangepaste workers en de bestanden die door de workers worden gegenereerd, op te slaan als elementuitvoeringen.
feature: asset-compute, processing-profiles
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---


# Integreren met AEM verwerkingsprofielen

Workers van Asset Compute kunnen alleen aangepaste uitvoeringen in AEM als Cloud Service genereren als deze in AEM zijn geregistreerd als service Auteur van Cloud Service via het verwerken van profielen. Op alle elementen die onder dat verwerkingsprofiel vallen, wordt de worker geactiveerd tijdens het uploaden of opnieuw verwerken en wordt de aangepaste uitvoering gegenereerd en beschikbaar gesteld via de uitvoeringen van het element.

## Een verwerkingsprofiel definiëren

Maak eerst een nieuw verwerkingsprofiel dat de worker aanroept met de configureerbare parameters.

![Profiel verwerken](./assets/processing-profiles/new-processing-profile.png)

1. Meld u aan bij AEM als Cloud Service Author-service als __AEM beheerder__. Aangezien dit een leerprogramma is adviseren wij het gebruiken van een ontwikkelomgeving of een milieu in een Sandbox.
1. Ga naar __Gereedschappen > Middelen > Profielen verwerken__
1. Tik op __Maken__ , knop
1. Geef het verwerkingsprofiel een naam, `WKND Asset Renditions`
1. Tik op het tabblad __Aangepast__ en tik op Nieuw __toevoegen__
1. De nieuwe service definiëren
   + __Naam van vertoning:__ `Circle`
      + De bestandsnaamuitvoering die wordt gebruikt om deze vertoning in AEM Assets te identificeren
   + __Extensie:__ `png`
      + De extensie van de vertoning die wordt gegenereerd. Stel dit in op de ondersteunde uitvoerindeling die door de webservice van de worker wordt ondersteund. Dit resulteert in een transparante achtergrond achter de uitgesneden cirkel. `png`
   + __Eindpunt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dit is de URL naar de worker die via `aio app get-url`deze URL is verkregen. Zorg ervoor dat de URL-punten zich op de juiste werkruimte bevinden, op basis van de AEM als een Cloud Service-omgeving waarin het verwerkingsprofiel is geconfigureerd. Dit subdomein komt overeen met de `development` werkruimte.
      + Controleer of de URL van de worker naar de juiste werkruimte verwijst. AEM als werkgebied van de Cloud Service zou werkruimte URL van het Stadium moeten gebruiken, en AEM als Productie zou de werkruimte URL van de Productie moeten gebruiken.
   + __Serviceparameters__
      + Tik op parameter __toevoegen__
         + Sleutel: `size`
         + Waarde: `1000`
      + Tik op parameter __toevoegen__
         + Sleutel: `contrast`
         + Waarde: `0.25`
      + Tik op parameter __toevoegen__
         + Sleutel: `brightness`
         + Waarde: `0.10`
      + Deze sleutel-/waardeparen worden doorgegeven aan de worker Asset Compute en zijn beschikbaar via het `rendition.instructions` JavaScript-object.
   + __MIME-typen__
      + __Omvat:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Deze MIME-typen zijn de enige die door de webservice van de worker worden ondersteund. Hierdoor wordt beperkt welke elementen door de aangepaste worker kunnen worden verwerkt.
      + __Omvat niet:__ `Leave blank`
         + Verwerk nooit activa met deze Types MIME gebruikend deze de dienstconfiguratie. In dit geval gebruiken we alleen een lijst van gewenste personen.
1. Tik op __Opslaan__ rechtsboven

## Een verwerkingsprofiel toepassen en aanroepen

1. Selecteer het nieuwe verwerkingsprofiel, `WKND Asset Renditions`
1. Tik op Profiel __toepassen op map(pen)__ in de bovenste actiebalk
1. Selecteer een map waarop u het verwerkingsprofiel wilt toepassen, bijvoorbeeld `WKND` en tik op __Toepassen__
1. Navigeer naar de map waarop het verwerkingsprofiel niet is toegepast via __AEM > Middelen > Bestanden__ en tik in `WKND`.
1. Upload enkele nieuwe afbeeldingselementen ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg)en [sample-3.jpg](../assets/samples/sample-3.jpg)) naar een willekeurige map in de map met het verwerkingsprofiel toegepast en wacht tot het geüploade element is verwerkt.
1. Tik op het element om de details te openen
   + Standaarduitvoeringen worden mogelijk sneller gegenereerd en weergegeven in AEM dan aangepaste uitvoeringen.
1. De weergave __Uitvoeringen__ openen vanuit de linkerzijbalk
1. Tik op het element met de naam `Circle.png` en bekijk de gegenereerde vertoning

   ![Gegenereerde uitvoering](./assets/processing-profiles/rendition.png)

## Voltooid!

Gefeliciteerd! U hebt de [zelfstudie](../overview.md) over het uitbreiden van AEM als Cloud Service Asset Compute microservices voltooid! U zou nu de capaciteit moeten hebben om de arbeiders van de Compute van de douane van Activa voor gebruik door uw AEM als dienst van de Auteur van de Cloud Service te vestigen, te ontwikkelen, te testen, te zuiveren en op te stellen.

### Bekijk de volledige broncode van het project op Github

Het definitieve project Asset Compute is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github bevat de definitieve staat van het project, volledig bevolkt met de arbeider en testgevallen, maar bevat geen geloofsbrieven, d.w.z.`.env`,`.config.json`of`.aio`._

## Problemen oplossen

### Aangepaste uitvoering ontbreekt in element

+ __Fout:__ Nieuwe en opnieuw verwerkte elementen worden verwerkt, maar de aangepaste uitvoering ontbreekt

#### Profiel verwerken dat niet is toegepast op de bovenliggende map

+ __Oorzaak:__ Het element bestaat niet in een map met het verwerkingsprofiel dat de aangepaste worker gebruikt
+ __Resolutie:__ Pas het verwerkingsprofiel toe op een bovenliggende map van het element

#### Bezig met verwerken van profiel vervangen door lager verwerkingsprofiel

+ __Oorzaak:__ Het middel bestaat onder een omslag met het toegepaste Profiel van de de arbeidersverwerking van de douane, nochtans een verschillend Profiel van de Verwerking dat niet de klantenarbeider gebruikt is toegepast tussen die omslag en het middel.
+ __Resolutie:__ De twee verwerkingsprofielen combineren of op een andere manier afstemmen en het tussentijdse verwerkingsprofiel verwijderen

### Verwerking van element mislukt

+ __Fout:__ Asset Processing Failed badge displayed on asset
+ __Oorzaak:__ Er is een fout opgetreden bij de uitvoering van de aangepaste worker
+ __Resolutie:__ Volg de instructies op het [zuiveren van de activering](../test-debug/debug.md#aio-app-logs) van Adobe I/O Runtime gebruikend `aio app logs`.
