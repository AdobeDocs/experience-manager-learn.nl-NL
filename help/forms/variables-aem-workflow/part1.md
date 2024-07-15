---
title: Variabelen in AEM workflow[Deel1]
description: Variabelen van het type XML, JSON, ArrayList en Document gebruiken in een AEM workflow
feature: Adaptive Forms, Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 561
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '404'
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

De adaptieve formuliergegevens worden opgeslagen onder het gegevenselement, zoals hierboven is weergegeven. **_in de bovengenoemde SubitterName van XPath is de naam van het tekstgebied in de Aangepaste Vorm._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - wanneer u een variabele van type XML creeert om de voorgelegde gegevens in uw werkschemamodel te vangen, te associëren gelieve XSD niet met de variabele. Dit komt omdat de verzonden gegevens niet voldoen aan de XSD-standaard wanneer u een adaptief XSD-formulier verzendt. De XSD klachtengegevens zijn ingesloten in /afData/afBoundData/ element.
>
>**AEM Forms 6.5.1** - als u XSD met uw variabele van XML associeert kunt u de schemaelementen doorbladeren om de veranderlijke afbeelding te doen. U hebt geen toegang tot formuliergegevens die niet zijn gebonden aan schema-elementen. Als uw gebruiksgeval tot gegevens moet toegang hebben verbindend aan schemaelementen evenals unbound gegevens, dan bindt niet schema met uw variabele van XML in het werkschema.U zult de aangewezen uitdrukking van XPath moeten gebruiken om aan de gegevens te krijgen die u nodig hebt

## XML-variabelen maken

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### Schema gebruiken met XML-variabele

**in kaart brengend een variabele van XML met schema. Deze mogelijkheid gebruiken met AEM Forms 6.5.1 en hoger**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### De variabele in send email gebruiken

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Voer de volgende stappen uit om de middelen op uw systeem te laten werken:

* [Elementen downloaden en importeren in AEM met pakketbeheer](assets/xmlandstringvariable.zip)
* [ Onderzoek het werkschemamodel ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) om de variabelen te begrijpen die in het werkschema worden gebruikt
* [ vorm de E-maildienst ](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [ open de Aangepaste Vorm ](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Vul de gegevens in en verzend het formulier.
