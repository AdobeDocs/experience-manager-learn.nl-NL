---
title: Asset compute-workers integreren met AEM verwerkingsprofielen
description: AEM as a Cloud Service integratie met Asset compute werknemers die via AEM Assets Processing Profiles naar Adobe I/O Runtime worden gestuurd. Verwerkingsprofielen worden geconfigureerd in de service Auteur om specifieke elementen te verwerken met behulp van aangepaste workers en de bestanden die door de workers worden gegenereerd, op te slaan als elementuitvoeringen.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# Integreren met AEM verwerkingsprofielen

Workers van Asset computen kunnen alleen aangepaste uitvoeringen genereren in AEM as a Cloud Service als ze zijn geregistreerd in AEM as a Cloud Service auteur-service via het verwerken van profielen. Op alle elementen die onder dat verwerkingsprofiel vallen, wordt de worker geactiveerd tijdens het uploaden of opnieuw verwerken en wordt de aangepaste uitvoering gegenereerd en beschikbaar gesteld via de uitvoeringen van het element.

## Een verwerkingsprofiel definiëren

Maak eerst een nieuw verwerkingsprofiel dat de worker aanroept met de configureerbare parameters.

![Profiel verwerken](./assets/processing-profiles/new-processing-profile.png)

1. Aanmelden bij AEM as a Cloud Service auteur als een __AEM__. Aangezien dit een leerprogramma is adviseren wij het gebruiken van een ontwikkelomgeving of een milieu in een Sandbox.
1. Navigeren naar __Gereedschappen > Middelen > Profielen verwerken__
1. Tikken __Maken__ knop
1. Geef het verwerkingsprofiel een naam, `WKND Asset Renditions`
1. Tik op de knop __Aangepast__ en tikken __Nieuwe toevoegen__
1. De nieuwe service definiëren
   + __Naam van vertoning:__ `Circle`
      + De bestandsnaam van de vertoning waarmee deze vertoning is geïdentificeerd in AEM Assets
   + __Extensie:__ `png`
      + De extensie van de vertoning die wordt gegenereerd. Instellen op `png` aangezien dit de ondersteunde uitvoerindeling is, wordt deze door de webservice van de worker ondersteund en wordt een transparante achtergrond achter de uitgesneden cirkel weergegeven.
   + __Eindpunt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dit is de URL naar de worker die via `aio app get-url`. Zorg ervoor dat de URL-punten zich op de juiste werkruimte bevinden, op basis van de AEM as a Cloud Service omgeving.
      + Controleer of de URL van de worker naar de juiste werkruimte verwijst. AEM as a Cloud Service werkgebied moet de werkruimte-URL van het werkgebied gebruiken en AEM as a Cloud Service productie moet de werkruimte-URL van de productie gebruiken.
   + __Serviceparameters__
      + Tikken __Parameter toevoegen__
         + Sleutel: `size`
         + Waarde: `1000`
      + Tikken __Parameter toevoegen__
         + Sleutel: `contrast`
         + Waarde: `0.25`
      + Tikken __Parameter toevoegen__
         + Sleutel: `brightness`
         + Waarde: `0.10`
      + Deze sleutel/waardeparen die in de worker van de Asset compute worden overgegaan en beschikbaar via `rendition.instructions` JavaScript-object.
   + __MIME-typen__
      + __Omvat:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Deze MIME-typen zijn de enige typen van de npm-modules van de worker. Deze lijst bevat limieten die worden verwerkt door de aangepaste worker.
      + __Omvat niet:__ `Leave blank`
         + Verwerk nooit activa met deze Types MIME gebruikend deze de dienstconfiguratie. In dit geval gebruiken we alleen een lijst van gewenste personen.
1. Tikken __Opslaan__ in de rechterbovenhoek

## Een verwerkingsprofiel toepassen en aanroepen

1. Selecteer het nieuwe verwerkingsprofiel, `WKND Asset Renditions`
1. Tikken __Profiel toepassen op map(pen)__ in de bovenste actiebalk
1. Selecteer een map waarop u het verwerkingsprofiel wilt toepassen, bijvoorbeeld `WKND` en tikken __Toepassen__
1. Navigeren naar de map waarop het verwerkingsprofiel niet is toegepast via __AEM > Middelen > Bestanden__ en tikken in `WKND`.
1. Enkele nieuwe afbeeldingselementen uploaden ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg), en [sample-3.jpg](../assets/samples/sample-3.jpg)) in een map in de map met het verwerkingsprofiel toegepast en wacht tot het geüploade element is verwerkt.
1. Tik op het element om de details te openen
   + Standaarduitvoeringen worden mogelijk sneller gegenereerd en weergegeven in AEM dan aangepaste uitvoeringen.
1. Open de __Uitvoeringen__ weergave in de linkerzijbalk
1. Tik op het element met de naam `Circle.png` en bekijk de gegenereerde uitvoering

   ![Generatie](./assets/processing-profiles/rendition.png)

## Voltooid!

Gefeliciteerd! U hebt de [zelfstudie](../overview.md) over de uitbreiding van AEM microdiensten voor as a Cloud Service Asset compute! U zou nu de capaciteit moeten hebben om de arbeiders van de douaneAsset compute voor gebruik door uw AEM as a Cloud Service dienst van de Auteur op te zetten, te ontwikkelen, te testen, te zuiveren en op te stellen.

### Bekijk de volledige broncode van het project op Github

Het definitieve project voor de Asset compute is beschikbaar op Github op:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github bevat de definitieve staat van het project, volledig bevolkt met de arbeider en testgevallen, maar bevat geen geloofsbrieven, namelijk. `.env`, `.config.json` of `.aio`._

## Problemen oplossen

+ [Aangepaste uitvoering ontbreekt in element in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Verwerking van middelen mislukt in AEM](../troubleshooting.md#asset-processing-fails)
