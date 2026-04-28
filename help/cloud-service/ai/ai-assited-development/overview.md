---
title: Ontwikkeling met behulp van AI
description: Leer over AI-bijgewoonde ontwikkeling die een IDE of coderingsagenten van AI samen met AGENTS.md, de Vaardigheden van de Agent, en servers gebruikt MCP helpen kwalitatief hoogstaande, productie-klaar code voor projecten op AEM as a Cloud Service produceren.
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20899
thumbnail: KT-20899.pngKT-20899
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 0%

---


# Ontwikkeling met behulp van AI

Bij de AI-ondersteunde ontwikkeling wordt gebruik gemaakt van een IDE of coderingsagent die op AI werkt, samen met `AGENTS.md` , Agent Skills en MCP-servers, om kwalitatief hoogwaardige code te produceren die klaar is voor productie voor AEM as a Cloud Service-projecten.

De hulpmiddelen zoals [&#128279;](https://www.cursor.com/) Curseur , [&#x200B; Copilot GitHub in de Code van Visual Studio &#x200B;](https://code.visualstudio.com/docs/copilot/overview), [&#x200B; Claude Code &#x200B;](https://code.claude.com/docs/en/overview), en gelijkaardige AI-Aangedreven IDEs en coderende agenten helpen op een paar zeer belangrijke manieren:

- **Snellere herhaling**: produceer of refactorcode van natuurlijke taalherinneringen die de gewenste eigenschap of de verandering beschrijven.
- **het Leren hulp**: verklaar onbekende codewegen, configuratie, concepten, of beste praktijken wanneer ertoe aangezet.

Nochtans, hangen deze voordelen sterk van de _context beschikbaar aan de coderende agent_ af. De generieke opleidingsgegevens en één enkele opnamemomentopname zijn vaak _niet voldoende_ om productie-klaar AEM code betrouwbaar te produceren.

## Waarom AI alleen onvoldoende is

Zonder de juiste context kunnen AI-modellen (via een door AI aangedreven IDE of coderingsagent):

- **Hallucineer APIs of levenscycli**: stel code of configuraties voor die niet met de beste praktijken van AEM as a Cloud Service of recentste eigenschappen richten.
- **juf procedurestappen**: laat vereiste stappen weg niet zichtbaar in codebewaarplaats of opleidingsgegevens.
- **Drijving van projectnormen**: negeer gevestigde patronen voor componenten, diensten OSGi, werkschema&#39;s, of de configuratie van Dispatcher.

Dit hiaat is waar _gestructureerde context_ (de Vaardigheden van de Agent en AGENTS.md) en _runtime zicht_ (servers MCP) essentieel worden om AI-bijgewoonde ontwikkeling _productief_ en _betrouwbaar_ te maken.

## Hoe Adobe helpt bij de ontwikkeling van AI

Voor AEM as a Cloud Service-projecten biedt Adobe:

- De Vaardigheden van de agent en AGENTS.md via [&#x200B; Vaardigheden van Adobe voor AI Coding Agenten &#x200B;](https://github.com/adobe/skills)
- Lokale MCP servers voor AEM SDK en lokale Dispatcher via het [&#128279;](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3) portaal van de Distributie van de Software 0&rbrace;
- Adobe-ontvangen servers van AEM MCP voor inhoud en de werkschema&#39;s van Cloud Manager van uw winde of praatjetoepassing - zie [&#x200B; Servers MCP in AEM &#x200B;](../mcp/overview.md)

In de volgende secties wordt elk item samengevat. Gebruik de **secties van de Opstelling** en **Gevallen van het Gebruik** aan het eind van deze pagina voor installatie en analyses voor AI-bijgewoonde ontwikkeling.

## Wat de Vaardigheden van de Agent zijn

De Vaardigheden van de agent zijn _procedurekennis of deskundigheid_ om coderingsagenten _te helpen echt werk betrouwbaar uitvoeren_. Voor meer informatie, zie de [&#x200B; Vaardigheden van de Agent &#x200B;](https://agentskills.io).

Voor een project van AEM as a Cloud Service, zijn de Vaardigheden van de Agent beschikbaar in de [&#x200B; Vaardigheden van Adobe voor AI Coding Agents &#x200B;](https://github.com/adobe/skills) bewaarplaats.

## Wat is AGENTS.md

AGENTS.md verstrekt de _context en instructies_ om coderingsagenten _werk op uw project_ te helpen. Voor meer informatie, zie [&#x200B; AGENTS.md &#x200B;](https://agents.md/).

Voor een project van AEM as a Cloud Service, leidt de `ensure-agents-md` bootstrap vaardigheid **AGENTS.md** bij de bewaarplaatswortel **wanneer het** mist. De vaardigheid inspecteert uw project (bijvoorbeeld, de wortel `pom.xml` en modules) en produceert op maat gemaakte begeleiding in plaats van het gebruiken van een statisch dossier. Als **AGENTS.md** reeds bestaat, wordt het **niet** beschreven.

Nadat het bestand bestaat, kunt u het bewerken en zo meer context en instructies toevoegen voor de beste werkwijzen van uw team of organisatie. De vaardigheid kan **CLAUDE.md** ook tot stand brengen die verwijzingen **AGENTS.md** zodat op Claude gebaseerde hulpmiddelen de zelfde begeleiding opvangen.

## Wat zijn MCP-servers

De servers MCP stellen hulpmiddelen en gegevens aan de coderende agent door het [&#x200B; ModelProtocol van de Context &#x200B;](https://modelcontextprotocol.io/) bloot, dat acties zoals het zuiveren, inspectie, uitvoering, en bevestiging van veranderingen steunt. Een server MCP kan op uw werkstation (**lokaal**) of als ontvangen dienst (**ver**) lopen.

Voor **lokale ontwikkeling** tegen AEM SDK en Dispatcher, installeer deze **lokale servers MCP** van het [&#x200B; portaal van de Distributie van de Software &#x200B;](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3):

- **AEM Quickstart Lokale server MCP**: stelt levende runtime gegevens van een lokale instantie van AEM SDK bloot om het oplossen van problemen en ontwikkeling te steunen. Voor meer informatie, zie [&#x200B; de Server van QuickStart MCP van AEM &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#aem-quickstart-mcp-server).
- **de Lokale server MCP van Dispatcher**: Laat runtime bevestiging en inspectie van een lokale instantie van Dispatcher toe. Voor meer informatie, zie {de Server van 0} Dispatcher MCP [&#128279;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#dispatcher-mcp-server).

Voor Adobe-ontvangen servers van AEM MCP (bijvoorbeeld, inhoud, read-only inhoud, en Cloud Manager), zie [&#x200B; Servers MCP in AEM &#x200B;](../mcp/overview.md).

## Instellen

<!-- 
CARDS
{target = _self}

* ./setup/agent-skills.md
    {title = Set up AEM Agent Skills}
    {description = Learn how to set up AEM Agent Skills for AI-assisted development.}
    {image = ./assets/agent-skills/select-aem-agent-skills-to-install.png}
    {cta = Install AEM Agent Skills}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up AEM Agent Skills">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/agent-skills.md" title="AEM Agent-vaardigheden instellen" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/agent-skills/select-aem-agent-skills-to-install.png" alt="AEM Agent-vaardigheden instellen"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/agent-skills.md" target="_self" rel="referrer" title="AEM Agent-vaardigheden instellen"> de Vaardigheden van de Agent van AEM van de opstelling </a>
                    </p>
                    <p class="is-size-6">Leer hoe u AEM Agent Skills instelt voor AI-ondersteunde ontwikkeling.</p>
                </div>
                <a href="./setup/agent-skills.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> installeer de Vaardigheden van de Agent van AEM </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Gevallen gebruiken

<!-- 
CARDS
{target = _self}

* ./use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ./assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/component-development.md" title="AEM-component maken met ondersteuning voor AI" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/component-development/review-generated-code.png" alt="AEM-component maken met ondersteuning voor AI"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/component-development.md" target="_self" rel="referrer" title="AEM-component maken met ondersteuning voor AI"> creeer de Component van AEM met AI-bijgewoonde ontwikkeling </a>
                    </p>
                    <p class="is-size-6">Leer hoe u AEM-componenten kunt ontwikkelen met behulp van AIR-ondersteunde ontwikkeling.</p>
                </div>
                <a href="./use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> creeer de Component van AEM </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Aanvullende bronnen

- [Lokale ontwikkeling met AI-gereedschappen](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [Adobe Skills voor AI-coderingsagents](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [Agent Skills](https://agentskills.io/home)