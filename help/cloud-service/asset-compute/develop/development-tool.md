---
title: Element voor computerontwikkeling
description: Het Asset Compute Development Tool is een lokaal webkanaal waarmee ontwikkelaars de workers Asset Computer lokaal kunnen configureren en uitvoeren, buiten de context van de AEM SDK om, tegen de bronnen Asset Compute in Adobe I/O Runtime.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: a71c61304bbc9d54490086b3313c823225fbe2e0
workflow-type: tm+mt
source-wordcount: '700'
ht-degree: 0%

---


# Element voor computerontwikkeling

Het Asset Compute Development Tool is een lokaal webkanaal waarmee ontwikkelaars de workers Asset Computer lokaal kunnen configureren en uitvoeren, buiten de context van de AEM SDK om, tegen de bronnen Asset Compute in Adobe I/O Runtime.

## Het hulpprogramma voor het berekenen van bedrijfsmiddelen uitvoeren

Het hulpmiddel van de Ontwikkeling van de Verkoop van Activa kan van de wortel van het de toepassingsproject van de Verwerking van Activa via het eindbevel worden in werking gesteld:

```
$ aio app run
```

Hiermee start u het hulpprogramma voor ontwikkeling op __http://localhost:9000__ en opent u het automatisch in een browservenster. Om het Hulpmiddel van de Ontwikkeling in werking te stellen, moet [een geldig, auto-geproduceerd devToolToken via een vraagparameter](#troubleshooting__devtooltoken)worden verstrekt.

## De interface Asset Compute Development Tools begrijpen{#interface}

![Element voor computerontwikkeling](./assets/development-tool/asset-compute-dev-tool.png)

1. __Bronbestand:__ De selectie van het bronbestand wordt gebruikt om:
   + Geselecteerde activa binair die het `source` binaire getal zal zijn dat tot de worker van de Verwerking van Activa wordt overgegaan
   + Bronbestanden uploaden
1. __Definitie van profiel voor middelenberekening:__ Definieert de worker Asset Compute die moet worden uitgevoerd inclusief parameters: inclusief het URL-eindpunt van de worker, de naam van de resulterende uitvoering en eventuele parameters
1. __Uitvoeren:__ Met de knop Uitvoeren wordt het profiel Asset Compute uitgevoerd, zoals gedefinieerd in de configuratieprofieleditor voor Asset Compute
1. __Afbreken:__ Met de knop Afbreken annuleert u een uitvoering die is gestart nadat u op de knop Uitvoeren hebt getikt
1. __Verzoek/antwoord:__ Verstrekt de HTTP- verzoek en reactie aan/van de toepassing van de Berekening van Activa die in Runtime van Adobe lopen. Dit kan nuttig zijn voor foutopsporing
1. __Activeringslogbestanden:__ De logboeken die de uitvoering van de toepassing Asset Compute beschrijven, samen met eventuele fouten. Deze informatie is ook beschikbaar in de `aio app run` standaardversie
1. __Uitvoeringen:__ Hiermee worden alle uitvoeringen weergegeven die zijn gegenereerd door de uitvoering van de toepassing Asset Compute
1. __devToolToken, queryparameter:__ Voor het token Asset Compute Development Tool moet een geldige `devToolToken` queryparameter aanwezig zijn. Deze token wordt automatisch gegenereerd wanneer een nieuw ontwikkelingsprogramma wordt gemaaid

### Een aangepaste worker uitvoeren

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)
_Doorklikken voor het uitvoeren van een Asset Compute-werk in Development Tool (geen audio)_

1. Zorg ervoor dat het hulpprogramma voor de ontwikkeling van Asset Compute (Asset Compute Development) is gestart vanuit de hoofdmap van het project met behulp van de `aio app run` opdracht.
1. Upload of selecteer een bestand met een [voorbeeldafbeelding in het Hulpprogramma voor het berekenen van bedrijfsmiddelen](../assets/samples/sample-file.jpg)
   + Controleer of het bestand is geselecteerd in het vervolgkeuzemenu __Bronbestand__ .
1. Het tekstgebied voor de definitie __van het profiel__ Asset Compute controleren
   + De `worker` sleutel bepaalt URL aan de opgestelde worker van de Compute van Activa
   + De `name` sleutel definieert de naam van de vertoning die moet worden gegenereerd
   + Andere sleutel/waarden kunnen in dit JSON-object worden opgegeven en zijn beschikbaar in de worker onder het `rendition.instructions` object
      + Voeg desgewenst waarden toe voor `size`, `contrast` en `brightness`:

         ```json
         {
             "renditions": [
                 {
                     "worker": "...",
                     "name": "rendition.png",
                     "size":"800",
                     "contrast": "0.30",
                     "brightness": "-0.15"
                 }
             ]
         }
         ```

1. Tik op de knop __Uitvoeren__
1. De sectie ____ Uitvoeringen wordt gevuld met een tijdelijke aanduiding voor een vertoning
1. Nadat de worker is voltooid, wordt de gegenereerde uitvoering weergegeven in de tijdelijke aanduiding voor de uitvoering

Als u codewijzigingen aanbrengt in de code van de worker terwijl het Development Tool wordt uitgevoerd, worden de wijzigingen &#39;warm&#39; geïmplementeerd. Het &quot;hete opstellen&quot;neemt verscheidene seconden in, zodat staat opstellen toe om te voltooien alvorens de arbeider van het Hulpmiddel van de Ontwikkeling opnieuw in werking te stellen.

## Problemen oplossen

### Vervolgkeuzelijst voor bronbestanden is onjuist{#troubleshooting__dev-tool-application-cache}

Het hulpmiddel van de Ontwikkeling van de Compute van activa kan een staat ingaan waar het stapelgegevens ophaalt, en het merkbaarst in het __Brondossier__ dropdown tonend onjuiste punten is.

+ __Fout:__ In het vervolgkeuzemenu Bronbestand worden onjuiste items weergegeven.
+ __Oorzaak:__ De browserstatus van Stale in cache veroorzaakt de
+ __Resolutie:__ In uw browser ontruimen volledig de de toepassingsstaat van het browser lusje, het browser geheime voorgeheugen, lokale opslag en de dienstarbeider.

### Ontbrekende devToolToken-queryparameter{#troubleshooting__devtooltoken}

+ __Fout:__ Melding van &quot;onbevoegd&quot; in Asset Compute Development Tool
+ __Oorzaak:__ `devToolToken` ontbreekt of is ongeldig
+ __Resolutie:__ Sluit het de browser van het Hulpmiddel van de Ontwikkeling van Activa Compute, beëindigt om het even welke lopende processen van het Hulpmiddel van de Ontwikkeling die via het `aio app run` bevel worden in werking gesteld, en herstart het Hulpmiddel van de Ontwikkeling (gebruikend `aio app run`).

### Kan bronbestanden niet verwijderen{#troubleshooting__remove-source-files}

+ __Fout:__ Er is geen manier om toegevoegde brondossiers uit de UI van Hulpmiddelen van de Ontwikkeling te verwijderen
+ __Oorzaak:__ Deze functionaliteit is niet geïmplementeerd
+ __Resolutie:__ Meld u aan bij uw leverancier voor cloudopslag met de referenties die zijn gedefinieerd in `.env`. Zoek de container die wordt gebruikt door de ontwikkelingsprogramma&#39;s (ook opgegeven in `.env`), navigeer naar de __bronmap__ en verwijder bronafbeeldingen. Mogelijk moet u de stappen uitvoeren die worden beschreven in het vervolgkeuzemenu [Bronbestanden onjuist](#troubleshooting__dev-tool-application-cache) als de verwijderde bronbestanden in het vervolgkeuzemenu worden weergegeven, omdat ze mogelijk lokaal in het cachegeheugen zijn opgeslagen in de toepassingsstatus van Ontwikkelingsgereedschappen.

   ![Microsoft Azure Blob-opslag](./assets/development-tool/troubleshooting__remove-source-files.png)
