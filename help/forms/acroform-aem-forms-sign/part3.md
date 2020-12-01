---
title: Transformaties met AEM Forms
seo-title: Adaptieve formuliergegevens samenvoegen met Acrobat
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 1%

---


# Deze mogelijkheid testen op uw systeem

[Download en importeer dit pakket naar ](assets/acro-form-aem-form.zip)
AEMTit pakket bevat de voorbeeldworkflow en de HTML-pagina waarmee u het schema van de geüploade Acrobat kunt maken.

## Workflow configureren

1. [Open het workflowmodel in de bewerkingsmodus](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Open de configuratie-eigenschappen van de stap MergeAcroformData.
3. Klik op het tabblad Proces.
4. Zorg ervoor dat de argumenten die u doorgeeft een geldige map op de server zijn.
5. Sla de wijzigingen op.

## Adaptief formulier maken

1. Maak een adaptief formulier met het schema dat u in de vorige stap hebt gemaakt.
2. Sleep een aantal schemaelementen naar het adaptieve formulier.
3. Configureer de verzendactie van het adaptieve formulier voor verzending naar AEM workflow (MergeAcroformData).
4. **Zorg ervoor dat u het pad naar het gegevensbestand opgeeft als &quot;Data.xml&quot;. Dit is zeer belangrijk aangezien de steekproefcode naar een dossier genoemd Data.xml in de werkschemalading zoekt.**
5. Geef een voorbeeld van een adaptief formulier weer en vul het formulier in en verzend het.
6. U ziet PDF met de gegevens samengevoegd bewaard aan de omslag die in stap 4 onder vormt werkschema wordt gespecificeerd

>[!NOTE]
>
>Het PDF-bestand dat wordt gegenereerd door het samenvoegen van gegevens met het acroform, wordt opgeslagen als pdfdocument.pdf in de payload-map van de workflow. Dit document kan vervolgens worden gebruikt voor verdere verwerking als onderdeel van de workflow
