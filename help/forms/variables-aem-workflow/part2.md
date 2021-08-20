---
title: Variabelen in AEM workflow[Deel2]
description: Variabelen van het type XML, JSON, ArrayList en Document gebruiken in een AEM workflow
version: 6.5
topic: Ontwikkeling
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Variabelen van het type JSON in AEM Workflow

Vanaf AEM Forms 6.5 kunnen we nu variabelen van het type JSON in AEM Workflow maken. Gewoonlijk zult u variabelen van type JSON tot stand brengen als u Adaptief Forms op basis van het schema JSON aan een AEMWerkstroom indient of u de resultaten van een de Invoke verrichting van het Model van de Gegevens van het Vorm wilt opslaan. In de volgende video worden de stappen doorlopen die nodig zijn om een variabele van het type JSON te maken en te gebruiken in AEM workflow

**Als u AEM Forms 6.5.0 gebruikt**

Wanneer u een variabele van het type JSON creeert om de voorgelegde gegevens in uw werkschemamodel te vangen, te associëren gelieve niet het schema JSON met de variabele. Dit komt doordat de verzonden gegevens niet compatibel zijn met het JSON-schema wanneer u een adaptief JSON-schema verzendt. De JSON-schemagegevens zijn ingesloten in het element afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Als u AEM Forms 6.5.1 en hoger gebruikt**

U kunt het schema met de variabele van type JSON in uw werkschemamodel in kaart brengen. U kunt de schemabrowser dan gebruiken om de schemaelementen met uw koord/aantalvariabelen in uw werkschemamodel in kaart te brengen

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Voer de volgende stappen uit om de middelen op uw systeem te laten werken:

* [Elementen downloaden en importeren in AEM met pakketbeheer](assets/jsonandstringvariable.zip)
* [Ontdek het ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) workflowmodel om te begrijpen welke variabelen worden gebruikt in de workflow
* [De e-mailservice configureren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Het adaptieve formulier openen](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* De gegevens invullen en het formulier verzenden
