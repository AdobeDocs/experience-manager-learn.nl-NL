---
title: AEM Forms met Marketo (Deel 3)
description: Zelfstudie voor de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 78
source-git-commit: 7e0d7e87d72aa1e4450649afa6a962099ceb2db4
workflow-type: tm+mt
source-wordcount: '217'
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

## Volgende stappen

[Alles samenvoegen voor testen](./part4.md)
