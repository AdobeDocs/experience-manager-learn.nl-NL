---
title: asset compute-ontwikkelingsinstrument
description: Het Hulpmiddel van de Ontwikkeling van de Asset compute is een lokaal Webkanaal dat ontwikkelaars toestaat om de arbeiders van de Computer van Activa plaatselijk, buiten de context van AEM SDK tegen de middelen van de Asset compute in Adobe I/O Runtime te vormen en uit te voeren.
feature: asset compute microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integratie, ontwikkeling
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%

---


# asset compute-ontwikkelingsinstrument

Het Hulpmiddel van de Ontwikkeling van de Asset compute is een lokaal Webkanaal dat ontwikkelaars toestaat om de arbeiders van de Computer van Activa plaatselijk, buiten de context van AEM SDK tegen de middelen van de Asset compute in Adobe I/O Runtime te vormen en uit te voeren.

## Het Asset compute Development Tool uitvoeren

Het Hulpmiddel van de Ontwikkeling van de Asset compute kan van de wortel van het project van de Asset compute via het eindbevel worden in werking gesteld:

```
$ aio app run
```

Dit zal het Hulpmiddel van de Ontwikkeling bij __http://localhost:9000__ beginnen, en zal het automatisch in een browser venster openen. Om het Hulpmiddel van de Ontwikkeling in werking te stellen, [moet een geldig, auto-geproduceerd devToolToken via een vraagparameter worden verstrekt](#troubleshooting__devtooltoken).

## Begrijp de interface van de Hulpmiddelen van de Ontwikkeling van de Asset compute{#interface}

![asset compute-ontwikkelingsinstrument](./assets/development-tool/asset-compute-dev-tool.png)

1. __Bronbestand:__ De selectie van het bronbestand wordt gebruikt om:
   + Geselecteerd element binair dat `source` binair zal zijn die tot de Asset compute worker wordt overgegaan
   + Bronbestanden uploaden
1. __Definitie profiel(en) van asset compute:__ definieert de Asset compute-worker die moet worden uitgevoerd, inclusief parameters: inclusief het URL-eindpunt van de worker, de naam van de resulterende uitvoering en eventuele parameters
1. __Uitvoeren:__ de knoop van de Looppas voert het profiel van de Asset compute zoals die in de redacteur van het de configuratieprofiel van de Asset compute wordt bepaald uit
1. __Afbreken:Met__ de knop Afbreken annuleert u een uitvoering die is gestart nadat u op de knop Uitvoeren hebt getikt
1. __Request/Response:__ Verstrekt de HTTP- verzoek en reactie aan/van de Asset compute worker die in Adobe I/O Runtime loopt. Dit kan nuttig zijn voor foutopsporing
1. __Activatielogboeken:__ de logboeken waarin de uitvoering van de Asset compute worker wordt beschreven, samen met eventuele fouten. Deze informatie is ook beschikbaar in de `aio app run`-standaard
1. __Uitvoeringen:__ geeft alle uitvoeringen weer die zijn gegenereerd door de uitvoering van de Asset compute worker
1. __devToolToken queryparameter:__ Voor het token van het hulpprogramma voor Asset compute-ontwikkeling moet een geldige  `devToolToken` queryparameter aanwezig zijn. Deze token wordt automatisch gegenereerd wanneer een nieuw ontwikkelingsprogramma wordt gemaaid

### Een aangepaste worker uitvoeren

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Doorklikken voor het uitvoeren van een Asset compute in Development Tool (Geen audio)_

1. Verzeker het Hulpmiddel van de Ontwikkeling van de Asset compute van uw projectwortel gebruikend het `aio app run` bevel is begonnen.
1. Upload of selecteer een [voorbeeldafbeeldingsbestand](../assets/samples/sample-file.jpg) in het Asset compute Development Tool
   + Zorg ervoor dat het bestand is geselecteerd in het vervolgkeuzemenu __Bronbestand__
1. Bekijk de __definitie van het profiel van de Asset compute__ tekstgebied
   + Met de `worker`-toets wordt de URL voor de geïmplementeerde Asset compute-worker gedefinieerd
   + De `name`-toets definieert de naam van de vertoning die moet worden gegenereerd
   + Andere sleutel/waarden kunnen in dit JSON-object worden opgegeven en zijn beschikbaar in de worker onder het `rendition.instructions`-object
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
1. In de sectie __Uitvoeringen__ wordt een tijdelijke aanduiding voor een vertoning ingevuld
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
