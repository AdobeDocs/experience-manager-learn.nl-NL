---
title: Variabelen in AEM workflow[Deel2]
seo-title: Variabelen in AEM workflow[Deel2]
description: Variabelen van het type xml,json,arraylist,document in een algemene workflow gebruiken
seo-description: Variabelen van het type xml,json,arraylist,document in een algemene workflow gebruiken
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Variabelen van het type JSON in AEM Workflow

Vanaf AEM Forms 6.5 kunnen we nu variabelen van het type JSON in AEM Workflow maken. Gewoonlijk zult u variabelen van type JSON tot stand brengen als u Adaptief Forms op basis van het schema JSON aan een AEMWerkstroom indient of u de resultaten van een de Invoke verrichting van het Model van de Gegevens van het Vorm wilt opslaan. In de volgende video worden de stappen doorlopen die nodig zijn om een variabele van het type JSON te maken en te gebruiken in AEM workflow
>[!NOTE]

**Als u AEM Forms 6.5.0 gebruikt**

Wanneer u een variabele van het type JSON creeert om de voorgelegde gegevens in uw werkschemamodel te vangen, te associëren gelieve niet het schema JSON met de variabele. Dit komt doordat de verzonden gegevens niet compatibel zijn met het JSON-schema wanneer u een adaptief JSON-schema verzendt. De JSON-schemagegevens zijn ingesloten in het element afData.afBoundData.data.

**Als u AEM Forms 6.5.1 en hoger gebruikt**

U kunt het schema met de variabele van type JSON in uw werkschemamodel in kaart brengen. U kunt de schemabrowser dan gebruiken om de schemaelementen met uw koord/aantalvariabelen in uw werkschemamodel in kaart te brengen

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)

**De capaciteit om schemaelementen neer te boren en het schemaelement aan werkschemavariabele in kaart te brengen is slechts beschikbaar met AEM Forms 6.5.1 vanaf.**

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Voer de volgende stappen uit om de middelen op uw systeem te laten werken:

* [Elementen downloaden en importeren in AEM met pakketbeheer](assets/jsonandstringvariable.zip)
* [Het workflowmodel](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) verkennen om inzicht te krijgen in de variabelen die worden gebruikt in de workflow
* [De e-mailservice configureren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Het adaptieve formulier openen](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* De gegevens invullen en het formulier verzenden
