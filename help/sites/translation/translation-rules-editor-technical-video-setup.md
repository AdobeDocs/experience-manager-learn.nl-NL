---
title: Vertaalregels instellen in AEM
description: Met de gebruikersinterface van de vertaalconfiguratie kan een gebruiker regels voor het vertalen van inhoud in AEM Sites beheren. In deze video wordt gedetailleerd ingegaan op het maken van een nieuwe vertaalregel voor een aangepaste component.
feature: Language Copy
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
duration: 542
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---

# Vertaalregels instellen {#set-up-translation-rules-in-aem}

Met de gebruikersinterface van de vertaalconfiguratie kan een gebruiker regels voor het vertalen van inhoud in AEM Sites beheren. In deze video wordt gedetailleerd ingegaan op het maken van een nieuwe vertaalregel voor een aangepaste component.

>[!NOTE]
>
> De video hieronder is opgenomen op AEM 6.3. AEM 6.4+ introduceert een nieuwe opslagplaats voor het opslaan van het dossier van XML van vertaalregels. Wanneer het gebruiken van de Configuratie UI van de Vertaling in AEM 6.4+ worden de regels bewaard aan de plaats `/conf/global/settings/translation/rules/translation_rules.xml`. Zie [Te vertalen inhoud identificeren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) voor meer informatie .

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

De vertaalregels identificeren inhoud in AEM die voor vertaling moet worden geëxtraheerd. De vertaalregels in het vak gelden voor veelvoorkomende gebruiksgevallen, zoals tekstcomponenten en alt-tekst voor afbeeldingscomponenten. Afhankelijk van een project kunnen er aanvullende regels voor vertaling nodig zijn. In het algemeen kunnen gebruikers door middel van vertaalregels aangeven:

1. Eigenschappen die op weg en/of middeltype moeten worden vertaald
2. Filters voor eigenschappen die NIET moeten worden vertaald
3. Inhoud waarnaar wordt verwezen en die moet worden vertaald (bijvoorbeeld afbeeldingen of inhoudsfragmenten)

De redacteur van vertaalregels die het vertaling xml- dossier zal bijwerken. De UI van de Configuratie van de Vertaling maakt het gemakkelijker om diverse vertaalregels te beheren en beschermt tegen typos wanneer het uitgeven van XML direct.

Toegang tot de gebruikersinterface van de vertaalconfiguratie:

* **[!UICONTROL AEM Start Menu]> [!UICONTROL Tools] > [!UICONTROL General] > [[!UICONTROL Translation Configuration]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Vóór AEM 6.3 {#prior-to-aem}

In vorige AEM versies werden de vertaalregels handmatig bijgewerkt door een XML-bestand te bewerken dat zich onder de vertaalworkflow bevindt: `/etc/workflow/models/translation/translation_rules.xml`.

## Aanvullende bronnen {#additional-resources}

* [Te vertalen inhoud identificeren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Inhoud vertalen voor meertalige sites](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Aanbevolen werkwijzen voor vertaling](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
