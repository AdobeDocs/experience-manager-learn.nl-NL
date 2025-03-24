---
title: Asset Compute Development Tool
description: Het Asset Compute Development Tool is een lokaal webkanaal waarmee ontwikkelaars Asset Computer-workers lokaal kunnen configureren en uitvoeren, buiten de context van de AEM SDK en tegen de Asset Compute-bronnen in Adobe I/O Runtime.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Asset Compute Development Tool

Het Asset Compute Development Tool is een lokaal webkanaal waarmee ontwikkelaars Asset Computer-workers lokaal kunnen configureren en uitvoeren, buiten de context van de AEM SDK en tegen de Asset Compute-bronnen in Adobe I/O Runtime.

## Asset Compute Development Tool uitvoeren

Het Asset Compute Development Tool kan vanaf de basis van het Asset Compute-project worden uitgevoerd via de terminalopdracht:

```
$ aio app run
```

Dit zal het Hulpmiddel van de Ontwikkeling in __http://localhost:9000__ beginnen, en zal het automatisch in een browser venster openen. Voor het Te lopen Hulpmiddel van de Ontwikkeling, [ een geldig, auto-geproduceerde devToolToken moet via een vraagparameter ](#troubleshooting__devtooltoken) worden verstrekt.

## De interface Asset Compute Development Tools begrijpen{#interface}

![ Asset Compute Development Tool ](./assets/development-tool/asset-compute-dev-tool.png)

1. __het dossier van Source:__ de selectie van het brondossier wordt gebruikt om:
   + Geselecteerd het element binair dat fungeert als het `source` binaire getal dat aan de Asset Compute-worker wordt doorgegeven
   + Bronbestanden uploaden
1. __Asset Compute profiel(en) definitie:__ bepaalt de worker van Asset Compute om met inbegrip van parameters in werking te stellen: met inbegrip van het eindpunt URL van de worker, de resulterende vertoningsnaam, en om het even welke parameters
1. __Looppas:__ de knoop van de Looppas voert het profiel van Asset Compute uit zoals die in de redacteur van het de configuratieprofiel van Asset Compute wordt bepaald
1. __Afbreken:__ de knoop van de Afbreking annuleert een uitvoering die van het tikken van de knoop van de Looppas in werking wordt gesteld
1. __Verzoek/Reactie:__ verstrekt het verzoek van HTTP en antwoord aan/van de worker van Asset Compute die in Adobe I/O Runtime loopt. Dit kan nuttig zijn voor foutopsporing
1. __Logboeken van de Activering:__ De logboeken die de uitvoering van de worker van Asset Compute, samen met om het even welke fouten beschrijven. Deze informatie is ook beschikbaar in de `aio app run` standaard out
1. __Vertoningen:__ toont alle vertoningen die door de uitvoering van de worker van Asset Compute worden geproduceerd
1. __devToolToken vraagparameter:__ het teken van het Hulpmiddel van de Ontwikkeling van Asset Compute vereist een geldige `devToolToken` vraagparameter aanwezig te zijn. Deze token wordt automatisch gegenereerd wanneer een nieuw ontwikkelingsprogramma wordt gemaaid

### Een aangepaste worker uitvoeren

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_klik-door van het runnen van een werk van Asset Compute in het Hulpmiddel van de Ontwikkeling (Geen audio)_

1. Zorg ervoor dat Asset Compute Development Tool is gestart vanuit de hoofdmap van het project met de opdracht `aio app run` .
1. In het Hulpmiddel van de Ontwikkeling van Asset Compute, upload of selecteer het dossier van het a [ steekproefbeeld ](../assets/samples/sample-file.jpg)
   + Zorg ervoor het dossier in het __dossier van Source__ dropdown wordt geselecteerd
1. Herzie het __het profieldefinitie van Asset Compute__ tekstgebied
   + De sleutel `worker` definieert de URL voor de geïmplementeerde Asset Compute-worker
   + De toets `name` definieert de naam van de vertoning die moet worden gegenereerd
   + In dit JSON-object kunnen andere sleutel/waarden worden opgegeven. Deze zijn beschikbaar in de worker onder het `rendition.instructions` -object
      + Voeg desgewenst waarden toe voor `size` , `contrast` en `brightness` :

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

1. Tik __looppas__ knoop
1. De __sectie van Vertoningen__ zal met een bezitter van de vertoningsplaats van de vertoning bevolken
1. Nadat de worker is voltooid, wordt de gegenereerde uitvoering weergegeven in de tijdelijke aanduiding voor de uitvoering

Als u codewijzigingen aanbrengt in de code van de worker terwijl het Development Tool wordt uitgevoerd, worden de wijzigingen &#39;warm&#39; geïmplementeerd. Het &quot;hete opstellen&quot;neemt verscheidene seconden in, zodat staat opstellen toe om te voltooien alvorens de arbeider van het Hulpmiddel van de Ontwikkeling opnieuw in werking te stellen.

## Problemen oplossen

+ [Onjuiste JAML-inspringing](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize limit is set to low](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [Development Tool kan niet worden gestart omdat private.key ontbreekt](../troubleshooting.md#missing-private-key)
+ [Vervolgkeuzelijst Source-bestanden is onjuist](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Ontbrekende of ongeldige devToolToken-queryparameter](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Kan bronbestanden niet verwijderen](../troubleshooting.md#unable-to-remove-source-files)
+ [Gedeeltelijk getekende/beschadigde vertoning geretourneerd](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
