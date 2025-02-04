---
title: Een lokale ontwikkelomgeving instellen
description: Stel een lokale ontwikkelomgeving in voor sites die worden geleverd met Edge Delivery Services en die kunnen worden bewerkt met Universal Editor.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 66bc4cb6f992c64b1a7e32310ce3e26515f3d380
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 0%

---

# Een lokale ontwikkelomgeving instellen

Een lokale ontwikkelomgeving is essentieel voor het snel ontwikkelen van websites die door Edge Delivery Services worden geleverd. De omgeving gebruikt lokaal ontwikkelde code terwijl inhoud wordt opgehaald van Edge Delivery Services, zodat ontwikkelaars direct wijzigingen in de code kunnen bekijken. Een dergelijke installatie ondersteunt snelle, iteratieve ontwikkeling en tests.

De ontwikkelingstools en -processen voor een website-project voor Edge Delivery Services zijn ontworpen om vertrouwd te zijn met webontwikkelaars en een snelle en efficiënte ontwikkelervaring te bieden.

## Ontwikkelingstopologie

Deze video verstrekt een overzicht van de ontwikkelingstopologie voor een Edge Delivery Services websiteproject dat met Universele Redacteur editable is.

>[!VIDEO](https://video.tv.adobe.com/v/3443978/?learn=on&enablevpops)

+++Zie extra details van de ontwikkelingstopologie

- **bewaarplaats GitHub**:
   - **Doel**: Gastheren de code van de website (CSS en JavaScript).
   - **Structuur**: De **belangrijkste tak** bevat productie-klaar code, terwijl andere takken werkende code houden.
   - **Functionaliteit**: De code van om het even welke tak kan tegen de **productie** lopen of **voorproef** milieu&#39;s zonder de levende website te beïnvloeden.

- **AEM de dienst van de Auteur**:
   - **Doel**: dient als canonieke inhoudsbewaarplaats waar de website inhoud wordt uitgegeven en wordt geleid.
   - **Functionaliteit**: De inhoud wordt gelezen en geschreven door de **Universele Redacteur**. Bewerkte inhoud wordt gepubliceerd aan **Edge Delivery Services** in **productie** of **voorproef** milieu&#39;s.

- **Universele Redacteur**:
   - **Doel**: Verstrekt een interface van WYSIWYG voor het uitgeven van website inhoud.
   - **Functionaliteit**: Leest van en schrijft aan de **AEM dienst van de Auteur**. Kan worden gevormd om code van om het even welke tak in de **bewaarplaats GitHub** te gebruiken om veranderingen te testen en te bevestigen.

- **Edge Delivery Services**:
   - **het milieu van de Productie**:
      - **Doel**: Levert de levende website inhoud en de code aan eind - gebruikers.
      - **Functionaliteit**: Dient inhoud die van de **AEM dienst van de Auteur** wordt gepubliceerd gebruikend code van de **belangrijkste tak** van de **bewaarplaats GitHub**.
   - **Milieu van de Voorproef**:
      - **Doel**: Verstrekt een het opvoeren milieu om inhoud en code vóór plaatsing te testen en te voorproef.
      - **Functionaliteit**: Dient inhoud die van de **AEM dienst van de Auteur** wordt gepubliceerd gebruikend code van om het even welke tak van de **bewaarplaats GitHub**, toelatend grondig het testen zonder de levende website te beïnvloeden.

- **Lokale ontwikkelaarmilieu**:
   - **Doel**: Staat ontwikkelaars toe om code (CSS en JavaScript) plaatselijk te schrijven en te testen.
   - **Structuur**:
      - Een lokale kloon van de **bewaarplaats GitHub** voor op tak-gebaseerde ontwikkeling.
      - **AEM CLI**, die als ontwikkelingsserver dienst doet, past lokale codeveranderingen op het **milieu van de Voorproef** voor een heet-herladenervaring toe.
   - **Werkschema**: De ontwikkelaars schrijven code plaatselijk, begaan veranderingen in een werkende tak, duw de tak aan GitHub, bevestigen het in de **Universele Redacteur** (gebruikend de gespecificeerde tak), en voegen het in de **belangrijkste tak** samen wanneer klaar voor productieplaatsing.

+++

## Vereisten

Installeer het volgende op uw computer voordat u de ontwikkeling start:

1. [ Git ](https://git-scm.com/)
1. [ Node.js &amp; npm ](https://nodejs.org)
1. [ Code van Microsoft Visual Studio ](https://code.visualstudio.com/) (of gelijkaardige coderedacteur)

## Clone the GitHub repository

Kloon de [ bewaarplaats GitHub die in het nieuwe hoofdstuk van het codeproject ](./1-new-code-project.md) wordt gecreeerd dat het AEM codeproject van Edge Delivery Services aan uw lokale ontwikkelomgeving bevat.

![ GitHub bewaart kloon ](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Er wordt een nieuwe map `aem-wknd-eds-ue` gemaakt in de map `Code` , die als hoofdmap van het project fungeert. Hoewel het project kan worden gekloond aan om het even welke plaats op de computer, gebruikt dit leerprogramma `~/Code` als wortelfolder.

## Projectafhankelijkheden installeren

Navigeer naar de projectmap en installeer de vereiste afhankelijkheden met `npm install` . Hoewel de projecten van Edge Delivery Services geen traditionele Node.js bouwt systemen zoals Webpack of Vite gebruiken, vereisen zij nog verscheidene gebiedsdelen voor lokale ontwikkeling.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## De AEM CLI installeren

AEM CLI is een Node.js bevel-lijn hulpmiddel dat wordt ontworpen om de ontwikkeling van op Edge Delivery Services-gebaseerde AEM websites te stroomlijnen, die een lokale ontwikkelingsserver voor snelle ontwikkeling en het testen van uw website verstrekken.

Voer de volgende handelingen uit om de AEM CLI te installeren:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

De AEM CLI kan ook globaal worden geïnstalleerd gebruikend `npm install --global @adobe/aem-cli`.

## De lokale AEM ontwikkelingsserver starten

Met de opdracht `aem up` wordt de lokale ontwikkelingsserver gestart en wordt automatisch een browservenster naar de URL van de server geopend. Deze server fungeert als een reverse-proxy voor de omgeving van de Edge Delivery Services, waarbij inhoud van die server wordt aangeboden terwijl uw lokale codebasis voor ontwikkeling wordt gebruikt.

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

De AEM CLI opent de website in uw browser op `http://localhost:3000/`. De veranderingen in het project worden automatisch heet-opnieuw geladen in Webbrowser, terwijl de inhoudsveranderingen [ het publiceren aan het voorproefmilieu ](./6-author-block.md) vereisen en Webbrowser verfrissen.

Als de website met een 404 pagina opent, is het waarschijnlijk [ fstab.yaml of paths.json ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) bijgewerkt in [ nieuw codeproject ](./1-new-code-project.md) verkeerd gevormd, of de veranderingen zijn niet begaan aan aan de `main` tak.

## JSON-fragmenten maken

De projecten van Edge Delivery Services, die gebruikend het [ AEM malplaatje Boilerplate XWalk ](https://github.com/adobe-rnd/aem-boilerplate-xwalk) worden gecreeerd, baseren zich op configuraties JSON die blokcreatie in de Universele Redacteur toelaten.

- **JSON fragments**: Opgeslagen met hun bijbehorende blokken en bepalen de blokmodellen, de definities, en de filters.
   - **Modelfragmenten**: Opgeslagen bij `/blocks/example/_example.json`.
   - **de fragmenten van de Definitie**: Opgeslagen bij `/blocks/example/_example.json`.
   - **de fragmenten van de Filter**: Opgeslagen bij `/blocks/example/_example.json`.

De manuscripten NPM compileren deze fragmenten JSON en plaatsen hen in de aangewezen plaatsen in de projectwortel. Gebruik de meegeleverde NPM-scripts om JSON-bestanden te maken. Als u bijvoorbeeld alle fragmenten wilt compileren, voert u de volgende handelingen uit:

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| NPM-script | Beschrijving |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | Stelt alle JSON-bestanden samen uit fragmenten en voegt deze toe aan de juiste `component-*.json` -bestanden. |
| `build:json:models` | Bouwt blok JSON fragmenten en compileert hen in `/component-models.json`. |
| `build:json:definitions` | Stelt pagina JSON-fragmenten samen en compileert deze in `/component-definitions.json` . |
| `build:json:filters` | Stelt pagina JSON-fragmenten samen en compileert deze in `/component-filters.json` . |

>[!TIP]
>
> Voer `npm run build:json` uit na eventuele wijzigingen in fragmentbestanden om de JSON-hoofdbestanden opnieuw te genereren.

## Koppeling

De verbinding verzekert codekwaliteit en consistentie, die voor Edge Delivery Services projecten alvorens veranderingen in de `main` tak samen te voegen wordt vereist.

De NPM-scripts kunnen bijvoorbeeld via `npm run` worden uitgevoerd:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| NPM-script | Beschrijving |
|------------------|--------------------------------------------------|
| `lint:js` | Hiermee worden JavaScript-lijncontroles uitgevoerd. |
| `lint:css` | Hiermee worden CSS-lijncontroles uitgevoerd. |
| `lint` | Hiermee voert u zowel JavaScript- als CSS-lijncontroles uit. |

### Verbindingsproblemen automatisch corrigeren

U kunt problemen met koppelingen automatisch oplossen door de volgende `scripts` aan het project `package.json` toe te voegen en u kunt deze via `npm run` uitvoeren:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

Deze scripts zijn niet vooraf geconfigureerd met de AEM sjabloon Boilerplate XWalk, maar kunnen wel aan het `package.json` -bestand worden toegevoegd:

>[!BEGINTABS]

>[!TAB  Extra manuscripten ]

| NPM-script | Opdracht | Beschrijving |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | Hiermee worden problemen met JavaScript-koppelingen automatisch opgelost. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | Hiermee worden problemen met CSS-koppelingen automatisch opgelost. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | Hiermee worden zowel JS- als CSS-correctiescripts uitgevoerd voor snelle opschoning. |

>[!TAB  package.json voorbeeld ]

De volgende scriptitems kunnen worden toegevoegd aan de array `package.json` `scripts` .

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
