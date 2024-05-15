---
title: Asset compute ontwikkelen
description: Het Hulpmiddel van de Ontwikkeling van de Asset compute is een lokaal Webkanaal dat ontwikkelaars toestaat om de arbeiders van de Computer van Activa plaatselijk, buiten de context van AEM SDK tegen de middelen van de Asset compute in Adobe I/O Runtime te vormen en uit te voeren.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Asset compute ontwikkelen

Het Hulpmiddel van de Ontwikkeling van de Asset compute is een lokaal Webkanaal dat ontwikkelaars toestaat om de arbeiders van de Computer van Activa plaatselijk, buiten de context van AEM SDK tegen de middelen van de Asset compute in Adobe I/O Runtime te vormen en uit te voeren.

## Het Asset compute Development Tool uitvoeren

Het Hulpmiddel van de Ontwikkeling van de Asset compute kan van de wortel van het project van de Asset compute via het eindbevel worden in werking gesteld:

```
$ aio app run
```

Hiermee wordt het Ontwikkelingsinstrument gestart op __http://localhost:9000__ en opent deze automatisch in een browservenster. Voor het uitvoeren van het Development Tool, [een geldige, auto-geproduceerde devToolToken moet via een vraagparameter worden verstrekt](#troubleshooting__devtooltoken).

## Begrijp de interface van de Hulpmiddelen van de Ontwikkeling van de Asset compute{#interface}

![Asset compute ontwikkelen](./assets/development-tool/asset-compute-dev-tool.png)

1. __Bronbestand:__ De selectie van het bronbestand wordt gebruikt om:
   + Geselecteerd element dat als de `source` binair doorgegeven aan de Asset compute worker
   + Bronbestanden uploaden
1. __Definitie(s) van profiel(en) van asset compute:__ Definieert de Asset compute worker die moet worden uitgevoerd inclusief parameters: inclusief het URL-eindpunt van de worker, de resulterende renditienaam en eventuele parameters
1. __Uitvoeren:__ De knoop van de Looppas voert het profiel van de Asset compute uit zoals die in de redacteur van het de configuratieprofiel van de Asset compute wordt bepaald
1. __Afbreken:__ Met de knop Afbreken annuleert u een uitvoering die is gestart nadat u op de knop Uitvoeren hebt getikt
1. __Verzoek/antwoord:__ Verstrekt de HTTP- verzoek en reactie aan/van de Asset compute worker die in Adobe I/O Runtime loopt. Dit kan nuttig zijn voor foutopsporing
1. __Activeringslogbestanden:__ De logboeken die de uitvoering van de Asset compute worker beschrijven, samen met eventuele fouten. Deze informatie is ook beschikbaar in de `aio app run` standaard out
1. __Uitvoeringen:__ Hiermee worden alle uitvoeringen weergegeven die zijn gegenereerd door de uitvoering van de Asset compute-worker
1. __devToolToken, queryparameter:__ Voor het ontwikkelingstoken van Asset compute is een geldige waarde vereist `devToolToken` de vraagparameter om aanwezig te zijn. Deze token wordt automatisch gegenereerd wanneer een nieuw ontwikkelingsprogramma wordt gemaaid

### Een aangepaste worker uitvoeren

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Doorklikken voor het uitvoeren van een Asset compute in Development Tool (Geen audio)_

1. Zorg ervoor dat het hulpprogramma voor de ontwikkeling van Asset computen is gestart vanuit de hoofdmap van het project met behulp van de `aio app run` gebruiken.
1. Upload of selecteer een [voorbeeldafbeeldingsbestand](../assets/samples/sample-file.jpg)
   + Zorg ervoor dat het bestand is geselecteerd in het dialoogvenster __Bronbestand__ druppel
1. Controleer de __Definitie van asset compute-profiel__ tekstgebied
   + De `worker` De sleutel bepaalt URL aan de opgestelde worker van de Asset compute
   + De `name` key definieert de naam van de vertoning die moet worden gegenereerd
   + Andere sleutel/waarden kunnen in dit JSON-object worden opgegeven en zijn beschikbaar in de worker onder de `rendition.instructions` object
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

1. Tik op de knop __Uitvoeren__ knop
1. De __Sectie Uitvoeringen__ wordt gevuld met een weergaveplaatsaanduiding
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
