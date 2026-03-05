---
title: MCP-servers in AEM
description: Leer hoe u MCP-servers (AEM Model Context Protocol) van uw voorkeurstoepassingen voor IDE of Chat-gebaseerde toepassingen kunt gebruiken om uw AEM-inhoudswerk te stroomlijnen en te versnellen.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20473
source-git-commit: c5f1c7f57181b1e9de6dd91aa2428f2fe1a04893
workflow-type: tm+mt
source-wordcount: '881'
ht-degree: 0%

---

# MCP-servers in AEM

Leer hoe te om de Servers van de Context van het ModelProtocol van de AEM _(MCP)_ van uw aangewezen IDE of op Chat-Gebaseerde toepassingen van AI te gebruiken om uw de inhoudswerk van AEM te stroomlijnen en te versnellen. U beschrijft wat u in een natuurlijke taal wilt in plaats van laag-vlakke API code te schrijven of door AEM UI te navigeren.

## Lijst met AEM MCP-servers

Alle AEM MCP-servers zijn beschikbaar onder `https://mcp.adobeaemcloud.com/adobe/mcp/` . Zie [ Gebruikend MCP met AEM as a Cloud Service ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service) voor meer informatie.

- **Inhoud** (`/content`) — Volledige toegang tot creeer, lees, update, en schrap pagina&#39;s, fragmenten, en activa.
- **Inhoud (read-only)** (`/content-readonly`) — read-only om pagina&#39;s, fragmenten, en activa (geen veranderingen) te maken en te krijgen.
- **Cloud Manager** (`/cloudmanager`) - om de programma&#39;s, milieu&#39;s, bewaarplaatsen en pijpleidingen van Adobe Cloud Manager te beheren.

>[!TIP]
>
>De gereedschappen die elke server beschikbaar stelt, kunnen in de loop der tijd veranderen. Als u wilt zien wat er nu beschikbaar is, vraagt u uw AI om alle AEM MCP-gereedschappen weer te geven (bijvoorbeeld `List all AEM MCP tools available from this server and describe what they do` ) of typt u de aanwijzing `tools/list` in uw IDE.

## Gebruikspatronen voor de MCP-server

Voordat u de AEM MCP-servers gaat gebruiken, moet u de twee belangrijkste gebruikspatronen voor de MCP-servers begrijpen:

- **mens-centric** — U bent in de bestuurderszitplaats. U vraagt, AI stelt of stelt hulpmiddelen voor u in winde voor.
- **Agentic** — Een agentic toepassing (agent of sub-agent) roept de server op zijn eigen, het kiezen van hulpmiddelen en het werken naar een doel met weinig menselijke input.

Hieronder wordt beschreven hoe deze twee gebruikspatronen worden vergeleken:

| Verhouding | Menselijk-centrisch | Agentic |
| ------ | ------------- | ------- |
| **die acties** aandrijft | Jij. <br> De AI stelt gereedschappen voor of voert deze voor u uit in de IDE- of chattoepassing. | De AI. <br> Hierbij wordt bepaald welke gereedschappen moeten worden gebruikt en blijven zo weinig mogelijk instructies aan de slag. |
| **het gezag van het Besluit** | Je blijft de controle houden. U keurt elke stap goed of activeert deze. | De AI heeft meer vrijheid. Voor de acties met een hoog effect kunnen voorzorgsmaatregelen of goedkeuringen nodig zijn. |
| **Typisch gebruikspatroon** | **per-ontwikkelaar**, gebruikt u het van uw eigen winde of op Chat-Gebaseerde toepassing, één ontwikkelaar per zitting, goed voor dagelijks dev werk. | **Gedeeld** via een agentische toepassing, als gedeelde diensten en gateways voor vele gebruikers of agenten. |
| **het meest geschikt voor** | Inhoud reviseren, begeleidende updates uitvoeren, taken verkennen of herhalen terwijl u in de lus blijft. | Agentische workflows, batchtaken, pijpleidingen en doelen waar het systeem met minimale tussenkomst moet werken. |

### Bij gebruik van MCP in systemen voor arbeidsbureaus

MCP de Servers worden ontworpen voor **mens-in werking gestelde Cliënten MCP** met interactieve UX en menselijk toezicht. De specificatie van Hulpmiddelen MCP adviseert _een mens in de lijn_ die hulpmiddelaanroepen kan goedkeuren of ontkennen.

Als u de Servers MCP in een agentic of autonoom systeem gebruikt, behandelt dat als afzonderlijke verenigbaarheidsrij. Doet **geen hardcode** hulpmiddelnamen in _herinneringen_, _lijsten van gewenste personen_, of _verpletterend logica_. In MCP, is de _hulpmiddelnaam_ een programmatic herkenningsteken, is de _beschrijving_ de model-onder ogen ziende wenk voor LLM. Voorkeur voor mogelijkheid of beschrijving op basis van vragen en selectie.

Voer runtime ontdekking via `tools/list` uit, handvat hulpmiddel-lijst veranderingen (`notifications/tools/list_changed`), en richt zich met de leverancier MCP van de Server op het aan boord gaan en het versioning als u stabiliteitsgaranties voorbij de protocolbasislijn nodig hebt.

## MCP-entiteiten en hun toewijzing

MCP wordt gebouwd rond drie entiteiten, **gastheer**, **cliënt**, en **server**. De [ specificatie MCP ](https://modelcontextprotocol.io/docs/getting-started/intro) bepaalt formeel hen. In de onderstaande tabel worden de afzonderlijke tabellen echter in duidelijke termen uitgelegd en in kaart gebracht bij gebruik van AEM MCP-servers.

| Component | Standaarddefinitie | Bij gebruik van AEM MCP-servers |
| --------- | ------------------- | ---------------- |
| **Gastheer** | De app die alles uitvoert, verzamelt context, praat met de AI, verwerkt machtigingen en maakt clients. | Uw **winde** (Cursor) of op Chat-Gebaseerde toepassing is de gastheer. Het stelt de cliënt MCP in werking en beslist welke hulpmiddelen en servers uw zitting kan gebruiken. |
| **Cliënt** | Eén verbinding van de host naar één server. Het geeft berichten heen en weer en zorgt ervoor dat de toegang van die server gescheiden blijft van die van anderen. | De **cliënt MCP** levens in uw winde of op Chat-Gebaseerde toepassing. Wanneer u de AEM Content MCP Server in montages toevoegt, leidt de IDE of op Chat-Gebaseerde toepassing tot een cliënt die met die server spreekt. Uw vragen en hulpmiddelvraag gaan door deze cliënt. |
| **Server** | De dienst die hulpmiddelen, gegevens, en herinneringen over MCP blootstelt. Het kan op uw computer of ver lopen. | De Adobe ontvangen **servers MCP van AEM** biedt hulpmiddelen aan om pagina&#39;s, inhoudsfragmenten, en activa tot stand te brengen, te lezen bij te werken en te schrappen zodat AI in uw IDE of op Chat-Gebaseerde toepassing met uw milieu van AEM kan werken. |

Eenvoudig gezet, **Gastheer** is uw winde of op Chat-Gebaseerde toepassing, **de Cliënt** is de verbinding van winde of op Chat-Gebaseerde toepassing aan AEM, **Server** is de Adobe ontvangen Servers van AEM MCP die het werk doen.

## Instellen

AEM MCP-servers zijn ontworpen voor gebruik met een gedefinieerde set MCP-compatibele toepassingen. De volgende toepassingen worden officieel ondersteund:

- [ Anthropic Claude ](https://claude.com/product/overview)
- [ Cursor ](https://www.cursor.com/)
- [ OpenAI ChatGPT ](https://chatgpt.com/)
- [ Microsoft Copilot Studio ](https://www.microsoft.com/en-us/microsoft-365-copilot/microsoft-copilot-studio)

Zie [ Overzicht van de Opstelling ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service#setup-overview) voor meer informatie.

## Gevallen gebruiken

<!-- CARDS
{target = _self}

* ./accelerate-content-operations-with-aem-mcp-server.md    
  {title = Accelerate Content Operations with AEM MCP Server}
  {description = Learn how to use the AEM Content MCP Server from Cursor IDE to streamline and accelerate your AEM content work.}
  {image = ../assets/content-mcp-server/update-adventure-price-prompt-response.png}
  {cta = Learn Content MCP Server}

* ./cloud-manager.md
  {title = Cloud Manager MCP Server}
  {description = Learn how to use the AEM Cloud Manager MCP Server from Cursor IDE to streamline and accelerate your AEM cloud manager work.}
  {image = ../assets/cm-mcp-server/start-pipeline.png}
  {cta = Learn Cloud Manager MCP Server}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Accelerate Content Operations with AEM MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./accelerate-content-operations-with-aem-mcp-server.md" title="Inhoudsbewerkingen versnellen met AEM MCP Server" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/content-mcp-server/update-adventure-price-prompt-response.png" alt="Inhoudsbewerkingen versnellen met AEM MCP Server"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" title="Inhoudsbewerkingen versnellen met AEM MCP Server"> versnelt de Verrichtingen van de Inhoud met de Server van AEM MCP </a>
                    </p>
                    <p class="is-size-6">Leer hoe u de AEM Content MCP Server van Cursor IDE gebruikt om uw AEM-inhoudswerk te stroomlijnen en te versnellen.</p>
                </div>
                <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer Inhoud MCP Server </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud Manager MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./cloud-manager.md" title="Cloud Manager MCP Server" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/cm-mcp-server/start-pipeline.png" alt="Cloud Manager MCP Server"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./cloud-manager.md" target="_self" rel="referrer" title="Cloud Manager MCP Server"> Cloud Manager MCP Server </a>
                    </p>
                    <p class="is-size-6">Leer hoe u de AEM Cloud Manager MCP-server van de Cursor-IDE kunt gebruiken om uw AEM Cloud Manager-werk te stroomlijnen en te versnellen.</p>
                </div>
                <a href="./cloud-manager.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> Leer de Server van Cloud Manager MCP </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
