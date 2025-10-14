---
title: Asset Compute-workers integreren met AEM-verwerkingsprofielen
description: AEM as a Cloud Service kan worden geïntegreerd met Asset Compute-workers die via AEM Assets-verwerkingsprofielen naar Adobe I/O Runtime worden gestuurd. Verwerkingsprofielen worden geconfigureerd in de service Auteur om specifieke elementen te verwerken met behulp van aangepaste workers en de bestanden die door de workers worden gegenereerd, op te slaan als elementuitvoeringen.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 126
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# Integreren met AEM-verwerkingsprofielen

Asset Compute-workers kunnen alleen aangepaste uitvoeringen genereren in AEM as a Cloud Service als ze zijn geregistreerd in de AEM as a Cloud Service Author-service via het verwerken van profielen. Op alle elementen die onder dat verwerkingsprofiel vallen, wordt de worker geactiveerd tijdens het uploaden of opnieuw verwerken en wordt de aangepaste uitvoering gegenereerd en beschikbaar gesteld via de uitvoeringen van het element.

## Een verwerkingsprofiel definiëren

Maak eerst een nieuw verwerkingsprofiel dat de worker aanroept met de configureerbare parameters.

![&#x200B; Profiel van de Verwerking &#x200B;](./assets/processing-profiles/new-processing-profile.png)

1. Login aan de dienst van de Auteur van AEM as a Cloud Service als __Beheerder van AEM__. Aangezien dit een leerprogramma is adviseren wij het gebruiken van een ontwikkelomgeving of een milieu in een Sandbox.
1. Navigeer aan __Hulpmiddelen > Assets > de Profielen van de Verwerking__
1. Tik __creeer__ knoop
1. Geef het verwerkingsprofiel een naam, `WKND Asset Renditions`
1. Tik het __Eigen__ lusje, en de Tik __voeg Nieuw__ toe
1. De nieuwe service definiëren
   + __Naam van de Vertoning:__ `Circle`
      + De bestandsnaam van de vertoning waarmee deze vertoning is geïdentificeerd in AEM Assets
   + __Uitbreiding:__ `png`
      + De extensie van de vertoning die wordt gegenereerd. Stel in op `png` omdat dit de ondersteunde uitvoerindeling is die door de webservice van de worker wordt ondersteund. Dit resulteert in een transparante achtergrond achter de uitgesneden cirkel.
   + __Eindpunt:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dit is de URL naar de worker die via `aio app get-url` wordt verkregen. Zorg ervoor dat de URL-punten zich op de juiste werkruimte bevinden, op basis van de AEM as a Cloud Service-omgeving.
      + Controleer of de URL van de worker naar de juiste werkruimte verwijst. AEM as a Cloud Service Stage moet de werkruimte-URL van het werkgebied gebruiken en AEM as a Cloud Service Production moet de URL van de productiewerkruimte gebruiken.
   + __Parameters van de Dienst__
      + Tik __voeg Parameter__ toe
         + Sleutel: `size`
         + Waarde: `1000`
      + Tik __voeg Parameter__ toe
         + Sleutel: `contrast`
         + Waarde: `0.25`
      + Tik __voeg Parameter__ toe
         + Sleutel: `brightness`
         + Waarde: `0.10`
      + Deze sleutel-/waardeparen worden doorgegeven aan de Asset Compute-worker en zijn beschikbaar via het JavaScript-object `rendition.instructions` .
   + __MIME Types__
      + __omvat:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Deze MIME-typen zijn de enige typen van de npm-modules van de worker. Deze lijst bevat limieten die worden verwerkt door de aangepaste worker.
      + __sluit uit:__ `Leave blank`
         + Verwerk nooit activa met deze Types MIME gebruikend deze de dienstconfiguratie. In dit geval gebruiken we alleen een lijst van gewenste personen.
1. Tik __sparen__ in het hoogste recht

## Een verwerkingsprofiel toepassen en aanroepen

1. Selecteer het nieuwe verwerkingsprofiel, `WKND Asset Renditions`
1. Tik __pas Profiel op Omslag(en)__ op de hoogste actiebar toe
1. Selecteer een omslag om het Profiel van de Verwerking op toe te passen, zoals `WKND` en tikken __van toepassing is__
1. Navigeer aan de omslag het Profiel van de Verwerking niet werd toegepast op via __AEM > Assets > Dossiers__ en tik in `WKND`.
1. Upload sommige nieuwe beeldactiva ([&#x200B; steekproef-1.jpg &#x200B;](../assets/samples/sample-1.jpg), [&#x200B; sample-2.jpg &#x200B;](../assets/samples/sample-2.jpg), en [&#x200B; sample-3.jpg &#x200B;](../assets/samples/sample-3.jpg)) in om het even welke omslag onder de omslag met het toegepaste Profiel van de Verwerking, en wacht op de geuploade activa om worden verwerkt.
1. Tik op het element om de details te openen
   + Standaarduitvoeringen worden mogelijk sneller gegenereerd en weergegeven in AEM dan aangepaste uitvoeringen.
1. Open de __mening van Vertoningen__ van de linkerzijbalk
1. Tik op het element met de naam `Circle.png` en bekijk de gegenereerde uitvoering

   ![&#x200B; Gegenereerde vertoning &#x200B;](./assets/processing-profiles/rendition.png)

## Voltooid!

Gefeliciteerd! U hebt het [&#x200B; leerprogramma &#x200B;](../overview.md) geëindigd op hoe te om de microdiensten van AEM as a Cloud Service Asset Compute uit te breiden! U moet nu de mogelijkheid hebben om aangepaste Asset Compute-workers in te stellen, te ontwikkelen, te testen, te debuggen en te implementeren voor gebruik door uw AEM as a Cloud Service Author-service.

### Bekijk de volledige broncode van het project op Github

Het uiteindelijke Asset Compute-project is beschikbaar op Github op:

+ [&#x200B; aem-guides-wknd-asset-compute &#x200B;](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github bevat is de definitieve staat van het project, volledig bevolkt met de arbeider en testgevallen, maar bevat geen geloofsbrieven, dat wil zeggen. `.env` , `.config.json` of `.aio` ._

## Problemen oplossen

+ [Aangepaste uitvoering ontbreekt in element in AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Verwerking van middelen mislukt in AEM](../troubleshooting.md#asset-processing-fails)
