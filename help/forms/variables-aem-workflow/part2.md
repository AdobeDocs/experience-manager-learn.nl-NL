---
title: Variabelen in AEM Workflow[Deel2]
description: Variabelen van het type XML, JSON, ArrayList en Document gebruiken in een AEM-workflow
version: Experience Manager 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 354
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Variabelen van het type JSON in AEM Workflow

Vanaf AEM Forms 6.5 kunnen we nu variabelen van het type JSON maken in AEM Workflow. Gewoonlijk zult u variabelen van type JSON tot stand brengen als u Adaptive Forms die op schema JSON wordt gebaseerd aan een Workflow van AEM indient of u de resultaten van een de aanroepverrichting van het ModelGegevens van het Vorm wilt opslaan. In de volgende video worden de stappen doorlopen die nodig zijn voor het maken en gebruiken van een variabele van het type JSON in de AEM-workflow

**als het gebruiken van AEM Forms 6.5.0**

Wanneer u een variabele van het type JSON creeert om de voorgelegde gegevens in uw werkschemamodel te vangen, te associëren gelieve niet het schema JSON met de variabele. Dit komt doordat de verzonden gegevens niet compatibel zijn met het JSON-schema wanneer u een adaptief JSON-schema verzendt. De JSON-schemagegevens zijn ingesloten in het element afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**als het gebruiken van AEM Forms 6.5.1 en hierboven**

U kunt het schema met de variabele van type JSON in uw werkschemamodel in kaart brengen. U kunt de schemabrowser dan gebruiken om de schemaelementen met uw koord/aantalvariabelen in uw werkschemamodel in kaart te brengen

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Voer de volgende stappen uit om de middelen op uw systeem te laten werken:

* [De middelen downloaden en importeren naar AEM met behulp van pakketbeheer](assets/jsonandstringvariable.zip)
* [&#x200B; Onderzoek het werkschemamodel &#x200B;](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) om de variabelen te begrijpen die in het werkschema worden gebruikt
* [&#x200B; vorm de E-maildienst &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [&#x200B; open de Aangepaste Vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* De gegevens invullen en het formulier verzenden
