---
title: Transformaties met AEM Forms
description: Deel 3 van een zelfstudie waarin Acroforms worden geïntegreerd met AEM Forms. Test de workflow en het adaptieve formulier op uw systeem.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 49
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 1%

---


# Deze mogelijkheid testen op uw systeem

[Download en importeer dit pakket in AEM](assets/acro-form-aem-form.zip)
Dit pakket bevat de voorbeeldworkflow en de HTML-pagina waarmee u het schema kunt maken van het geüploade Acrobat-formulier.

## Workflow configureren

1. [Het workflowmodel openen in de bewerkingsmodus](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Open de configuratie-eigenschappen van de stap MergeAcroformData.
3. Klik op het tabblad Proces.
4. Zorg ervoor dat de argumenten die u doorgeeft een geldige map op de server zijn.
5. Sla de wijzigingen op.

## Adaptief formulier maken

1. Maak een adaptief formulier met het schema dat u in de vorige stap hebt gemaakt.
2. Sleep een aantal schemaelementen naar het adaptieve formulier.
3. Configureer de verzendactie van het adaptieve formulier voor verzending naar AEM workflow (MergeAcroformData).
4. **Zorg ervoor dat u het pad naar het gegevensbestand opgeeft als &quot;Data.xml&quot;. Dit is zeer belangrijk aangezien de steekproefcode naar een dossier genoemd Data.xml in het werkschemalading zoekt.**
5. Geef een voorbeeld van een adaptief formulier weer en vul het formulier in en verzend het.
6. U moet PDF zien met de samengevoegde gegevens die zijn opgeslagen in de map die is opgegeven in stap 4 onder de configuratieworkflow

>[!NOTE]
>
>Het PDF-bestand dat wordt gegenereerd door het samenvoegen van gegevens met het acroform, wordt opgeslagen als pdfdocument.pdf in de payload-map van de workflow. Dit document kan vervolgens worden gebruikt voor verdere verwerking als onderdeel van de workflow
