---
title: Creeer een project van de Asset compute voor Asset compute rekbaarheid
description: De projecten van de asset compute zijn projecten Node.js, die gebruikend Adobe I/O CLI worden geproduceerd, die aan een bepaalde structuur houden die hen om aan Adobe I/O Runtime toelaat worden opgesteld en met AEM as a Cloud Service worden geïntegreerd.
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 212
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# Een Asset compute maken

De projecten van de asset compute zijn projecten Node.js, die gebruikend Adobe I/O CLI worden geproduceerd, die aan een bepaalde structuur houden die hen om aan Adobe I/O Runtime toelaat worden opgesteld en met AEM as a Cloud Service worden geïntegreerd. Één enkel project van de Asset compute kan één of meerdere arbeiders van de Asset compute bevatten, met elk die een discrete eindpuntverwijzing van HTTP van een AEM as a Cloud Service Profiel van de Verwerking hebben.

## Een project genereren

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Doorklikken voor het genereren van een Asset compute-project (geen audio)_

Gebruik de [Adobe I/O CLI Asset compute plug-in](../set-up/development-environment.md#aio-cli) om een nieuw, leeg project van de Asset compute te produceren.

1. Navigeer vanaf de opdrachtregel naar de map waarin u het project wilt plaatsen.
1. Vanuit de opdrachtregel uitvoeren `aio app init` om met de interactieve projectgeneratie CLI te beginnen.
   + Dit bevel kan browser van het Web kweken die voor authentificatie aan Adobe I/O ertoe aanzetten. Als dit het geval is, geeft u de verificatiegegevens op die zijn gekoppeld aan de [vereiste Adobe-services en -producten](../set-up/accounts-and-services.md). Als u zich niet kunt aanmelden, volgt u [deze instructies over hoe te om een project te produceren](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Org selecteren__
   + Selecteer de Adobe die AEM as a Cloud Service heeft, App Builder zijn geregistreerd bij
1. __Project selecteren__
   + Zoek en selecteer het project. Dit is het [Projecttitel](../set-up/app-builder.md) gecreeerd van het App Builder projectmalplaatje, in dit geval `WKND AEM Asset Compute`
1. __Werkruimte selecteren__
   + Selecteer de `Development` werkruimte
1. __Welke functies van Adobe I/O App wilt u voor dit project toelaten? Selecteer de onderdelen die u wilt opnemen__
   + Selecteren `Actions: Deploy runtime actions`
   + Gebruik de pijltoetsen om de selectie op te heffen en ruimte om de selectie ongedaan te maken/te selecteren en druk op Enter om de selectie te bevestigen
1. __Selecteer het type acties dat u wilt genereren__
   + Selecteren `DX Asset Compute Worker v1`
   + Gebruik de pijlen om selectie te selecteren, ruimte om selectie ongedaan te maken/te selecteren en Enter om selectie te bevestigen
1. __Hoe wilt u deze handeling een naam geven?__
   + De standaardnaam gebruiken `worker`.
   + Als uw project meerdere workers bevat die verschillende elementberekeningen uitvoeren, geeft u deze een semantische naam

## Console.json genereren

Voor het hulpprogramma voor ontwikkelaars is een bestand met de naam `console.json` dat de vereiste geloofsbrieven bevat om met Adobe I/O te verbinden. Dit bestand wordt gedownload vanaf de Adobe I/O-console.

1. De Asset compute worker openen [Adobe I/O](https://console.adobe.io) project
1. Selecteer de projectwerkruimte om de `console.json` referenties voor, in dit geval selecteert u `Development`
1. Ga naar de hoofdmap van het Adobe I/O-project en tik op __Alles downloaden__ in de rechterbovenhoek.
1. Een bestand wordt gedownload als `.json` het dossier met het project en de werkruimte prefixeerde, bijvoorbeeld: `wkndAemAssetCompute-81368-Development.json`
1. U kunt
   + Naam van bestand wijzigen als `console.json` en verplaatst u het in de hoofdmap van uw Asset compute-arbeidersproject. Dit is de aanpak in deze zelfstudie.
   + Verplaats de map naar een willekeurige map EN verwijs ernaar vanuit uw `.env` bestand met een configuratie-item `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Het bestandspad kan absoluut of relatief zijn ten opzichte van de hoofdmap van uw project. Bijvoorbeeld:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     of
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> OPMERKING
> Het bestand bevat referenties. Als u het bestand opslaat in uw project, moet u het toevoegen aan uw `.gitignore` te voorkomen dat bestanden worden gedeeld. Hetzelfde geldt voor de `.env` bestand — Deze aanmeldingsbestanden mogen niet worden gedeeld of opgeslagen in Git.

## Asset compute project op GitHub

Het definitieve project van de Asset compute is beschikbaar op GitHub bij:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub bevat de definitieve staat van het project, volledig bevolkt met de arbeider en testgevallen, maar bevat geen geloofsbrieven, namelijk: `.env`, `console.json` of `.aio`._
