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
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---


# Element voor computerontwikkeling

Het Asset Compute Development Tool is een lokaal webkanaal waarmee ontwikkelaars de workers Asset Computer lokaal kunnen configureren en uitvoeren, buiten de context van de AEM SDK om, tegen de bronnen Asset Compute in Adobe I/O Runtime.

## Het hulpprogramma voor het berekenen van bedrijfsmiddelen uitvoeren

Het hulpmiddel van de Ontwikkeling van de Verkoop van Activa kan van de wortel van het project van de Verkoop van Activa via het eindbevel worden in werking gesteld:

```
$ aio app run
```

Hiermee start u het hulpprogramma voor ontwikkeling op __http://localhost:9000__ en opent u het automatisch in een browservenster. Om het Hulpmiddel van de Ontwikkeling in werking te stellen, moet [een geldig, auto-geproduceerd devToolToken via een vraagparameter](#troubleshooting__devtooltoken)worden verstrekt.

## De interface Asset Compute Development Tools begrijpen{#interface}

![Element voor computerontwikkeling](./assets/development-tool/asset-compute-dev-tool.png)

1. __Bronbestand:__ De selectie van het bronbestand wordt gebruikt om:
   + Geselecteerde activa binair die het `source` binaire getal zal zijn dat tot de worker van de Verwerking van Activa wordt overgegaan
   + Bronbestanden uploaden
1. __Definitie van profiel(en) voor middelenberekening:__ Definieert de worker Asset Compute die moet worden uitgevoerd inclusief parameters: inclusief het URL-eindpunt van de worker, de naam van de resulterende uitvoering en eventuele parameters
1. __Uitvoeren:__ Met de knop Uitvoeren wordt het profiel Asset Compute uitgevoerd, zoals gedefinieerd in de configuratieprofieleditor voor Asset Compute
1. __Afbreken:__ Met de knop Afbreken annuleert u een uitvoering die is gestart nadat u op de knop Uitvoeren hebt getikt
1. __Verzoek/antwoord:__ Verstrekt de HTTP- verzoek en reactie aan/van de worker Asset Compute die in Adobe I/O Runtime wordt uitgevoerd. Dit kan nuttig zijn voor foutopsporing
1. __Activeringslogbestanden:__ De logboeken waarin de uitvoering van de worker Asset Compute wordt beschreven, evenals eventuele fouten. Deze informatie is ook beschikbaar in de `aio app run` standaardversie
1. __Uitvoeringen:__ Hiermee worden alle uitvoeringen weergegeven die zijn gegenereerd door de uitvoering van de worker Asset Compute
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

+ [Onjuiste JAML-inspringing](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize limit is set to low](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Development Tool kan niet worden gestart omdat private.key ontbreekt](../troubleshooting.md#missing-private-key)
+ [Vervolgkeuzelijst voor bronbestanden is onjuist](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Ontbrekende of ongeldige devToolToken-queryparameter](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Kan bronbestanden niet verwijderen](../troubleshooting.md#unable-to-remove-source-files)
+ [Gedeeltelijk getekende/beschadigde vertoning geretourneerd](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
