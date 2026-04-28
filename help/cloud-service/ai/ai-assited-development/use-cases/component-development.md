---
title: Componentontwikkeling met AEM Agent Skills
description: Leer hoe u een AEM-component kunt ontwikkelen met AEM Agent Skills als onderdeel van de AI-ondersteunde ontwikkeling.
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20901
thumbnail: KT-20901.png
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---


# Componentontwikkeling met AEM Agent Skills

Leer hoe te om een component van AEM te ontwikkelen gebruikend de Vaardigheden van de Agent van AEM als deel van [ AI-bijgewoonde ontwikkeling ](../overview.md).

In deze analyse, gebruikt u natuurlijke taal in een AI-Gerichte winde (bijvoorbeeld, Curseur) om a **component van de Banner van de Banner van 0} Promo in het [ Project van de Plaatsen WKND ](https://github.com/adobe/aem-guides-wknd) te ontwikkelen.** De coderingsagent past de `create-component` Vaardigheid van de Agent van AEM toe om de implementatie te produceren.

## Vereisten

Voor het volgen van deze zelfstudie hebt u het volgende nodig:

- Een IDE van AI-Aangedreven zoals Cursor, of Code van Visual Studio met Kopilot GitHub.
- Een lokale kloon van het [ Project van Plaatsen WKND ](https://github.com/adobe/aem-guides-wknd), gebouwd en opgesteld aan a _lokale AEM SDK_ instantie.
- _de Vaardigheden van de Agent van AEM_ die in dat project worden geïnstalleerd. Als u dat nog niet hebt gedaan, voltooi [ de Vaardigheden van de Agent van AEM van de Opstelling ](../setup/agent-skills.md).

## Onderdeelvereiste

Laten we aannemen dat het WKND-team een promotiebanner op de homepage wil weergeven. De ontwerpreferentie ziet er als volgt uit:

![ Verwijzing van het Ontwerp van de Banner van de Bevordering ](../assets/component-development/promo-banner-design-reference.png)

De auteurs moeten _etiket van de Promo_, _CTA etiket_, en _de verbindingsgebieden van CTA_ in de componentendialoog kunnen plaatsen.

De ontwerpverwijzing is een schermafbeelding die is verkregen via draadframe, mockup of statische opmaakcode.

## De component ontwikkelen

1. Open het WKND project in uw winde. Bevestig dat de Vaardigheden van de Agent van AEM aanwezig zijn (bijvoorbeeld, onder `.agents/skills`), dan een nieuw agentenpraatje begint.
   ![ verifieer de Vaardigheden van de Agent van AEM geïnstalleerd zijn ](../assets/component-development/verify-aem-agent-skills-installed.png)

1. Voer een soortgelijke vraag in. Bevestig het het schermschot van het componentenontwerp (dat via draadframe wordt verkregen, mockup of statische prijsverhoging vangt) als uw winde beelden in praatje steunt:

   ```text
   Create a WKND Promo Banner Component. Please see attached screenshot for design reference.
   
   Dialog specification are:
   
   1. Promo Label - Textfield, required
   2. CTA Text - Textfield, required
   3. CTA Link - Pathfield, required
   ```

1. De coderingsagent gebruikt de `create-component` Vaardigheid van de Agent van AEM om de component te produceren. Bekijk de voorgestelde HTML-, Sling-model-, dialoogvenster-XML- en verwante bestanden.
   ![ herzie de geproduceerde code ](../assets/component-development/review-generated-code.png)

>[!TIP]
>
>In plaats van het verstrekken van de ontwerpverwijzing als screenshot, kunt u een ontwerp van het Cijfer via de [ server MCP van het Cijfer ](https://www.figma.com/mcp-catalog/) ook verstrekken om de component te produceren. De `create-component` vaardigheid steunt de [ het ontwerpintegratie van Figma ](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/references/figma-design-rules.md)


1. Implementeer de component in de lokale AEM-instantie/SDK.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Plaats in de ontwerpfase de Banner Promo op de startpagina en valideer het gedrag. Verfijn de implementatie als deze nog steeds afwijkt van de ontwerpreferentie.
   ![ Auteur de component van de Banner van de Bevordering ](../assets/component-development/author-promo-banner-component.png)

1. Controleer de nieuwe component door de pagina of de weergave te publiceren zoals deze is gepubliceerd.
   ![ herzie de pas gecreëerde component ](../assets/component-development/review-newly-created-component.png)

Gefeliciteerd. U hebt met succes een nieuwe component van AEM gecreeerd gebruikend de Vaardigheden van de Agent van AEM als deel van AI-bijgewoonde ontwikkeling.

## Buiten eenvoudige componenten

Deze analyse gebruikt een eenvoudige component. Dezelfde `create-component` vaardigheid ondersteunt ook rijkere gevallen, waaronder:

- Meerdere velden en geneste dialoogvelden
- AEM Core Components-extensies (inclusief Sling Resource Merger-patronen)
- Het dossier of kader URLs van figuren voor lay-out en het stileren, wanneer de server van Figma MCP (bijvoorbeeld `plugin-figma-figma`) in uw winde wordt toegelaten

Lees voor veldtypen, dialoogvensterpatronen, figuurregels en voorbeelden `SKILL.md` in de installatiemap voor vaardigheden, bijvoorbeeld `.agents/skills/create-component/SKILL.md` .

Voor een overzicht, installatiepaden door winde, en het oplossen van problemen, zie {de Agent van de Ontwikkeling van de Component van 0} AEM ](https://github.com/adobe/skills/blob/main/plugins/aem/cloud-service/skills/create-component/README.md) in de bewaarplaats van de Vaardigheden van Adobe.[

## AGENTS.md

Alvorens wij samenvatten, begrijpen wij hoe AGENTS.md als deel van het creëren van de component werd geproduceerd.

Voor de projecten van AEM as a Cloud Service, leidt de `ensure-agents-md` bootstrap vaardigheid (die tijdens [ de Vaardigheden van de Agent van AEM van de Opstelling ](../setup/agent-skills.md) wordt geselecteerd) `AGENTS.md` bij de bewaarplaatswortel wanneer het **** mist. Het gebruikt wat het uit uw projectlay-out leert.

Het **** overschrijft geen bestaand `AGENTS.md` dossier.

![ de verwezenlijking van AGENTS.md ](../assets/component-development/agents-md-creation.png)

## Aanvullende bronnen

- [Lokale ontwikkeling met AI-tools](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [Adobe Skills voor AI-coderingsagents](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [Agent Skills](https://agentskills.io/home)
