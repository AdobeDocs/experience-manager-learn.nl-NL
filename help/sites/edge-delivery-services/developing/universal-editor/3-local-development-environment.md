---
title: Een lokale ontwikkelomgeving instellen
description: Stel een lokale ontwikkelomgeving in voor sites die worden geleverd met Edge Delivery Services en die kunnen worden bewerkt met Universal Editor.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 0%

---

# Een lokale ontwikkelomgeving instellen

Een lokale ontwikkelomgeving is essentieel voor het snel ontwikkelen van websites die door Edge Delivery Services worden geleverd. De omgeving gebruikt lokaal ontwikkelde code terwijl inhoud van Edge Delivery Services wordt opgehaald, zodat ontwikkelaars direct wijzigingen in de code kunnen bekijken. Een dergelijke installatie ondersteunt snelle, iteratieve ontwikkeling en tests.

De ontwikkelingstools en -processen voor een Edge Delivery Services-websiteproject zijn ontworpen om vertrouwd te zijn met webontwikkelaars en een snelle en efficiënte ontwikkelervaring te bieden.

## Ontwikkelingstopologie

Deze video biedt een overzicht van de ontwikkelingstopologie voor een Edge Delivery Services-websiteproject dat kan worden bewerkt met Universal Editor.

>[!VIDEO](https://video.tv.adobe.com/v/3443984/?learn=on&enablevpops&captions=dut)

+++Zie extra details van de ontwikkelingstopologie

- **bewaarplaats GitHub**:
   - **Doel**: Gastheren de code van de website (CSS en JavaScript).
   - **Structuur**: De **belangrijkste tak** bevat productie-klaar code, terwijl andere takken werkende code houden.
   - **Functionaliteit**: De code van om het even welke tak kan tegen de **productie** lopen of **voorproef** milieu&#39;s zonder de levende website te beïnvloeden.

- **de dienst van de Auteur van AEM**:
   - **Doel**: dient als canonieke inhoudsbewaarplaats waar de website inhoud wordt uitgegeven en wordt geleid.
   - **Functionaliteit**: De inhoud wordt gelezen en geschreven door de **Universele Redacteur**. Bewerkte inhoud wordt gepubliceerd aan **Edge Delivery Services** in **productie** of **voorproef** milieu&#39;s.

- **Universele Redacteur**:
   - **Doel**: Verstrekt een interface van WYSIWYG voor het uitgeven van website inhoud.
   - **Functionaliteit**: Leest van en schrijft aan de **dienst van de Auteur van AEM**. Kan worden gevormd om code van om het even welke tak in de **bewaarplaats GitHub** te gebruiken om veranderingen te testen en te bevestigen.

- **Edge Delivery Services**:
   - **het milieu van de Productie**:
      - **Doel**: Levert de levende website inhoud en de code aan eind - gebruikers.
      - **Functionaliteit**: Dient inhoud die van de **dienst van de Auteur van AEM** wordt gepubliceerd gebruikend code van de **belangrijkste tak** van de **bewaarplaats GitHub**.
   - **Milieu van de Voorproef**:
      - **Doel**: Verstrekt een het opvoeren milieu om inhoud en code vóór plaatsing te testen en te voorproef.
      - **Functionaliteit**: Dient inhoud die van de **dienst van de Auteur van AEM** wordt gepubliceerd gebruikend code van om het even welke tak van de **bewaarplaats GitHub**, toelatend grondig het testen zonder de levende website te beïnvloeden.

- **Lokale ontwikkelaarmilieu**:
   - **Doel**: Staat ontwikkelaars toe om code (CSS en JavaScript) plaatselijk te schrijven en te testen.
   - **Structuur**:
      - Een lokale kloon van de **bewaarplaats GitHub** voor op tak-gebaseerde ontwikkeling.
      - **AEM CLI**, die als ontwikkelingsserver dienst doet, past lokale codeveranderingen op het **milieu van de Voorproef** voor een heet-herladenervaring toe.
   - **Werkschema**: De ontwikkelaars schrijven code plaatselijk, begaan veranderingen in een werkende tak, duw de tak aan GitHub, bevestigen het in de **Universele Redacteur** (gebruikend de gespecificeerde tak), en voegen het in de **belangrijkste tak** samen wanneer klaar voor productieplaatsing.

+++

## Vereisten

Installeer het volgende op uw computer voordat u de ontwikkeling start:

1. [&#x200B; Git &#x200B;](https://git-scm.com/)
1. [&#x200B; Node.js &amp; npm &#x200B;](https://nodejs.org)
1. [&#x200B; Code van Microsoft Visual Studio &#x200B;](https://code.visualstudio.com/) (of gelijkaardige coderedacteur)

## Clone the GitHub repository

Kloon de [&#x200B; bewaarplaats GitHub die in het nieuwe hoofdstuk van het codeproject &#x200B;](./1-new-code-project.md) wordt gecreeerd dat het de codeproject van AEM Edge Delivery Services aan uw lokale ontwikkelomgeving bevat.

![&#x200B; GitHub bewaart kloon &#x200B;](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Er wordt een nieuwe map `aem-wknd-eds-ue` gemaakt in de map `Code` , die als hoofdmap van het project fungeert. Hoewel het project kan worden gekloond aan om het even welke plaats op de computer, gebruikt dit leerprogramma `~/Code` als wortelfolder.

## Projectafhankelijkheden installeren

Navigeer naar de projectmap en installeer de vereiste afhankelijkheden met `npm install` . Hoewel Edge Delivery Services-projecten geen gebruik maken van traditionele Node.js-constructiesystemen zoals Webpack of Vite, vereisen ze nog steeds verschillende afhankelijkheden voor lokale ontwikkeling.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## AEM CLI installeren

De AEM CLI is een opdrachtregelprogramma Node.js dat is ontworpen om de ontwikkeling van AEM-websites in Edge Delivery Services te stroomlijnen en een lokale ontwikkelingsserver te bieden voor snelle ontwikkeling en het testen van uw website.

Voer de volgende handelingen uit om de AEM CLI te installeren:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

De AEM CLI kan ook globaal worden geïnstalleerd met `npm install --global @adobe/aem-cli`.

## De lokale AEM-ontwikkelingsserver starten

Met de opdracht `aem up` wordt de lokale ontwikkelingsserver gestart en wordt automatisch een browservenster naar de URL van de server geopend. Deze server fungeert als een reverse-proxy voor de Edge Delivery Services-omgeving, zodat inhoud van die server wordt aangeboden terwijl uw lokale codebasis voor ontwikkeling wordt gebruikt.

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

De AEM CLI opent de website in uw browser op `http://localhost:3000/`. De veranderingen in het project worden automatisch heet-opnieuw geladen in Webbrowser, terwijl de inhoudsveranderingen [&#x200B; het publiceren aan het voorproefmilieu &#x200B;](./6-author-block.md) vereisen en Webbrowser verfrissen.

Als de website met een 404 pagina opent, is het waarschijnlijk [&#x200B; fstab.yaml of paths.json &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) bijgewerkt in [&#x200B; nieuw codeproject &#x200B;](./1-new-code-project.md) verkeerd gevormd, of de veranderingen zijn niet begaan aan aan de `main` tak.

## JSON-fragmenten maken

De projecten van Edge Delivery Services, die gebruikend het [&#x200B; malplaatje van AEM Boilerplate XWalk &#x200B;](https://github.com/adobe-rnd/aem-boilerplate-xwalk) worden gecreeerd, baseren zich op configuraties JSON die blokauthoring in de Universele Redacteur toelaten.

- **JSON fragments**: Opgeslagen met hun bijbehorende blokken en bepalen de blokmodellen, de definities, en de filters.
   - **Modelfragmenten**: Opgeslagen bij `/blocks/example/_example.json`.
   - **de fragmenten van de Definitie**: Opgeslagen bij `/blocks/example/_example.json`.
   - **de fragmenten van de Filter**: Opgeslagen bij `/blocks/example/_example.json`.


Het [&#x200B; AEM Boilerplate XWalk projectmalplaatje &#x200B;](https://github.com/adobe-rnd/aem-boilerplate-xwalk) omvat a [&#x200B; Eigen &#x200B;](https://typicode.github.io/husky/) precommit haak die veranderingen in fragmenten JSON ontdekt en hen compileert in de aangewezen `component-*.json` dossiers op `git commit`.

Hoewel de volgende NPM-scripts handmatig via `npm run` kunnen worden uitgevoerd om de JSON-bestanden samen te stellen, is dit gewoonlijk niet nodig omdat deze automatisch door de husky pre-commit haak wordt afgehandeld.

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

Koppelingen zorgen voor kwaliteit en consistentie van de code. Dit is vereist voor Edge Delivery Services-projecten voordat wijzigingen worden samengevoegd in de `main` -vertakking.

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

Deze scripts zijn niet vooraf geconfigureerd met de AEM Boilerplate XWalk-sjabloon, maar kunnen wel aan het `package.json` -bestand worden toegevoegd:

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
