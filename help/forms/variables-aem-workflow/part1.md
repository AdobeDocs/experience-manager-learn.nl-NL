---
title: Variabelen in AEM workflow[Deel1]
seo-title: Variabelen in AEM workflow[Deel1]
description: Variabelen van het type xml,json,arraylist,document in een algemene workflow gebruiken
seo-description: Variabelen van het type xml,json,arraylist,document in een algemene workflow gebruiken
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 0%

---


# XML-variabelen in AEM workflow

Variabelen van het type XML worden doorgaans gebruikt wanneer u een op XSD gebaseerd adaptief formulier hebt en waarden wilt extraheren uit de verzending van het adaptieve formulier in uw workflow.

De volgende video doorloopt u de stappen nodig om variabelen van het type Koord en XML tot stand te brengen en hen in uw werkschema te gebruiken.

De XML-variabele kan worden gebruikt om het aangepaste formulier vooraf in te vullen of om de verzendingsgegevens van het aangepaste formulier in uw workflow op te slaan.

Tekenreeksvariabele kan door Xpathing in de XML-variabele worden ingevuld. Deze tekenreeksvariabele wordt dan doorgaans gebruikt om de plaatsaanduidingen voor de e-mailsjabloon te vullen in de component E-mail verzenden

>[!NOTE]
>
>Als uw adaptieve formulier niet aan XSD is gekoppeld, ziet het XPath voor het ophalen van de waarde van een element er zo uit
>
>**/afData/afUnboundData/data/submitName**

De adaptieve formuliergegevens worden opgeslagen onder het gegevenselement, zoals hierboven is weergegeven. **_In het bovenstaande XPath SubitterName is de naam van het tekstveld in het Adaptief formulier._**

>[!NOTE]
>
>**AEM Forms 6.5.0**  - Wanneer u een variabele van het type XML creeert om de voorgelegde gegevens in uw werkschemamodel te vangen, te associëren gelieve XSD niet met de variabele. Dit komt omdat de verzonden gegevens niet voldoen aan de XSD-standaard wanneer u een adaptief XSD-formulier verzendt. De XSD klachtengegevens zijn ingesloten in /afData/afBoundData/ element.
>
>**AEM Forms 6.5.1**  - Als u XSD aan uw variabele van XML associeert kunt u de schemaelementen doorbladeren om de veranderlijke afbeelding te doen. U hebt geen toegang tot formuliergegevens die niet zijn gebonden aan schema-elementen. Als uw gebruiksgeval tot gegevens moet toegang hebben verbindend aan schemaelementen evenals unbound gegevens, dan bindt niet schema met uw variabele van XML in het werkschema.U zult de aangewezen uitdrukking van XPath moeten gebruiken om aan de gegevens te krijgen die u nodig hebt

## XML-variabelen maken

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Schema gebruiken met XML-variabele

**Een XML-variabele toewijzen aan een schema. Gebruik deze mogelijkheid met AEM Forms 6.5.1 en hoger**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### De variabele in send email gebruiken

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Voer de volgende stappen uit om de middelen op uw systeem te laten werken:

* [Elementen downloaden en importeren in AEM met pakketbeheer](assets/xmlandstringvariable.zip)
* [Ontdek het ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) workflowmodel om te begrijpen welke variabelen worden gebruikt in de workflow
* [De e-mailservice configureren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Het adaptieve formulier openen](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Vul de gegevens in en verzend het formulier.

