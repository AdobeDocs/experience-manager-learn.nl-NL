---
title: Transformaties met AEM Forms
description: Deel 3 van een zelfstudie waarin Acroforms worden geïntegreerd met AEM Forms. Test de workflow en het adaptieve formulier op uw systeem.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 45
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 1%

---


# Deze mogelijkheid testen op uw systeem

[&#x200B; Download en voer dit pakket in AEM &#x200B;](assets/acro-form-aem-form.zip) in
Dit pakket bevat de voorbeeldworkflow en de HTML-pagina waarmee u het schema kunt maken van het geüploade Acrobat-formulier.

## Workflow configureren

1. [&#x200B; open het Model van het Werkschema op geef wijze &#x200B;](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html) uit.
2. Open de configuratie-eigenschappen van de stap MergeAcroformData.
3. Klik op het tabblad Proces.
4. Zorg ervoor dat de argumenten die u doorgeeft een geldige map op de server zijn.
5. Sla de wijzigingen op.

## Adaptief formulier maken

1. Maak een adaptief formulier met het schema dat u in de vorige stap hebt gemaakt.
2. Sleep een aantal schemaelementen naar het adaptieve formulier.
3. Configureer de verzendactie van het adaptieve formulier voor verzending naar de AEM-workflow (MergeAcroformData).
4. **zorg ervoor u de het dossierweg van Gegevens als &quot;Data.xml&quot;specificeert. Dit is zeer belangrijk aangezien de steekproefcode naar een dossier genoemd Data.xml in het werkschemalading zoekt.**
5. Geef een voorbeeld van een adaptief formulier weer en vul het formulier in en verzend het.
6. PDF wordt weergegeven met de gegevens die zijn samengevoegd en die zijn opgeslagen in de map die is opgegeven in stap 4 onder de configuratieworkflow

>[!NOTE]
>
>Het PDF-bestand dat wordt gegenereerd door het samenvoegen van gegevens met het acroform, wordt opgeslagen als pdfdocument.pdf in de payload-map van de workflow. Dit document kan vervolgens worden gebruikt voor verdere verwerking als onderdeel van de workflow
