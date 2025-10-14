---
title: Creeer een project van de Asset compute voor Asset compute rekbaarheid
description: De projecten van de asset compute zijn projecten Node.js, die gebruikend Adobe I/O CLI worden geproduceerd, die aan een bepaalde structuur houden die hen toelaat om aan Adobe I/O Runtime worden opgesteld en met AEM as a Cloud Service worden geïntegreerd.
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# Een Asset compute maken

De projecten van de asset compute zijn projecten Node.js, die gebruikend Adobe I/O CLI worden geproduceerd, die aan een bepaalde structuur houden die hen om aan Adobe I/O Runtime toelaat worden opgesteld en met AEM as a Cloud Service worden geïntegreerd. Een enkel project van de Asset compute kan één of meerdere arbeiders van de Asset compute bevatten, met elk die een discrete eindpuntverwijzing van HTTP van een Profiel van de Verwerking van AEM as a Cloud Service hebben.

## Een project genereren

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_klik-door van het produceren van een project van de Asset compute (Geen audio)_

Gebruik [&#x200B; Adobe I/O CLI Asset compute stop-in &#x200B;](../set-up/development-environment.md#aio-cli) om een nieuw, leeg project van de Asset compute te produceren.

1. Navigeer vanaf de opdrachtregel naar de map waarin u het project wilt plaatsen.
1. Van de bevellijn, voer `aio app init` uit om met de interactieve projectgeneratie CLI te beginnen.
   + Dit bevel kan browser van het Web kweken die voor authentificatie aan Adobe I/O ertoe aanzetten. Als het, uw geloofsbrieven van de Adobe verbonden aan de [&#x200B; vereiste diensten en de producten van Adobe &#x200B;](../set-up/accounts-and-services.md) verstrekt. Als u niet aan login kunt, volg [&#x200B; deze instructies op hoe te om een project &#x200B;](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user) te produceren.
1. __Uitgezochte Org__
   + Selecteer de Adobe die AEM as a Cloud Service heeft, App Builder is geregistreerd bij
1. __Uitgezochte Project__
   + Zoek en selecteer het project. Dit is de [&#x200B; titel van het Project &#x200B;](../set-up/app-builder.md) gecreeerd van het het projectmalplaatje van App Builder, in dit geval `WKND AEM Asset Compute`
1. __Uitgezochte Workspace__
   + De werkruimte `Development` selecteren
1. __Welke eigenschappen van de Adobe I/O App wilt u voor dit project toelaten? Componenten selecteren om op te nemen__
   + Selecteren `Actions: Deploy runtime actions`
   + Gebruik de pijltoetsen om de selectie op te heffen en ruimte om de selectie ongedaan te maken/te selecteren en druk op Enter om de selectie te bevestigen
1. __Uitgezochte type van te produceren acties__
   + Selecteren `DX Asset Compute Worker v1`
   + Gebruik de pijlen om selectie te selecteren, ruimte om selectie ongedaan te maken/te selecteren en Enter om selectie te bevestigen
1. __hoe u deze actie zou willen noemen?__
   + Gebruik de standaardnaam `worker` .
   + Als uw project meerdere workers bevat die verschillende elementberekeningen uitvoeren, geeft u deze een semantische naam

## Console.json genereren

Voor het hulpprogramma voor ontwikkelaars is een bestand met de naam `console.json` vereist dat de gegevens bevat die nodig zijn om verbinding te maken met Adobe I/O. Dit bestand wordt gedownload vanaf de Adobe I/O-console.

1. Open het Adobe I/O [&#128279;](https://console.adobe.io) project van de Asset compute arbeider 
1. Selecteer de projectwerkruimte waar u de `console.json` -referenties voor wilt downloaden. Selecteer in dit geval `Development`
1. Ga naar de wortel van het project van de Adobe I/O en tik __Download allen__ in de hoger-juiste hoek.
1. Een bestand wordt gedownload als een `.json` -bestand dat vooraf is ingesteld met het project en de werkruimte, bijvoorbeeld: `wkndAemAssetCompute-81368-Development.json`
1. U kunt
   + Wijzig de naam van het bestand in `console.json` en verplaats het bestand in de hoofdmap van het Asset compute worker-project. Dit is de aanpak in deze zelfstudie.
   + Verplaats de map naar een willekeurige map EN verwijs ernaar vanuit het `.env` -bestand met een configuratiegegevens `ASSET_COMPUTE_INTEGRATION_FILE_PATH` . Het bestandspad kan absoluut of relatief zijn ten opzichte van de hoofdmap van uw project. Bijvoorbeeld:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     of
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> OPMERKING
> Het bestand bevat referenties. Als u het bestand opslaat in uw project, moet u het toevoegen aan uw `.gitignore` -bestand om te voorkomen dat het wordt gedeeld. Hetzelfde geldt voor het bestand `.env` . Deze aanmeldingsbestanden mogen niet worden gedeeld of opgeslagen in Git.

## Asset compute project op GitHub

Het definitieve project van de Asset compute is beschikbaar op GitHub bij:

+ [&#x200B; aem-guides-wknd-asset-compute &#x200B;](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub bevat de definitieve staat van het project, volledig bevolkt met de arbeider en testgevallen, maar bevat geen geloofsbrieven, namelijk `.env`, `console.json` of `.aio`._
