---
title: AEM Forms Cloud Service en Marketo integreren (Deel 3)
description: Leer hoe u AEM Forms en Marketo kunt integreren met het AEM Forms-formuliergegevensmodel.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
source-git-commit: 835e76695824cc1f155720567ca104a50be4bab8
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 0%

---

# Formuliergegevensmodel maken

Na het vormen van de gegevensbron is de volgende stap een Model van de Gegevens van de Vorm te creëren dat op de gegevensbron wordt gebaseerd die in de vroegere stap wordt gevormd. Voer de volgende stappen uit om een formuliergegevensmodel te maken:

Wijs uw browser aan de [ pagina van de gegevensintegratie.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Hier worden alle gegevensintegraties weergegeven die op uw AEM-instantie zijn gemaakt.

1. Klik op Maken | Formuliergegevensmodel
1. Geef een betekenisvolle titel op, zoals FormsAndMarketo, en klik op Volgende
1. Selecteer de gegevensbron die in de vorige stap is geconfigureerd en klik op Maken en bewerken om het formuliergegevensmodel te openen in de bewerkingsmodus
1. Vouw het knooppunt FormsAndMarketo uit. Vouw het knooppunt Services uit
1. Selecteer de eerste bewerking Ophalen
1. Klik op Geselecteerde toevoegen
1. Klik op Alles selecteren in het dialoogvenster &quot;Gekoppelde modelobjecten toevoegen&quot; en klik vervolgens op Toevoegen
1. Sla het formuliergegevensmodel op door op Opslaan te klikken
1. Tab naar het tabblad Services
1. Selecteer de enige dienst die wordt vermeld en klik op de Dienst van de Test
1. Geef een geldige leadId op en klik op Testen. Als alles goed gaat, moet u de hoofddetails terugkrijgen, zoals hieronder in de schermafbeelding wordt getoond
   ![ testresultaten ](assets/testresults.png)

U kunt nu een adaptief formulier maken op basis van dit formuliergegevensmodel en Marketo-objecten invoegen en ophalen.