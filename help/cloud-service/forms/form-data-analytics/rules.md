---
title: Ingevulde formuliergegevensvelden rapporteren met Adobe Analytics
description: AEM Forms CS met Adobe Analytics integreren om formuliergegevensvelden te rapporteren
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# De regel definiëren

In het bezit van Markeringen hebben wij 2 nieuw gecreeerd [regels](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**Veldvalidatiefout en FormSubmit**).

![adaptief](assets/rules.png)


## Veldvalidatiefout

De **Veldvalidatiefout** regel wordt geactiveerd telkens wanneer zich een validatiefout voordoet in het adaptieve formulierveld. Als het telefoonnummer of de e-mail bijvoorbeeld in ons formulier niet de verwachte indeling heeft, wordt een foutbericht voor validatie weergegeven.

De regel van de Fout van de Bevestiging van het Gebied wordt gevormd door de gebeurtenis te plaatsen aan _**Adobe Experience Manager Forms-Error**_ zoals getoond in het het schermschot



![ingezetene van de verzoekende staat](assets/field_validation_error_rule.png)

De Adobe Analytics - Vastgestelde Variabelen worden gevormd als volgt

![handeling instellen](assets/field_validation_action_rule.png)

## Regel voor verzenden van formulier

De regel Formulier verzenden wordt geactiveerd telkens wanneer een adaptief formulier is verzonden.

De regel Formulier verzenden is geconfigureerd met de _**Adobe Experience Manager Forms - Verzenden**_ event

![form-submit-rule](assets/form-submit-rule.png)

In de regel Formulier verzenden, de waarde van het gegevenselement _**ApplicantsStateOfResidence**_ wordt toegewezen aan prop5 en de waarde van het gegevenselement FormTitle wordt toegewezen aan prop8.

De variabelen Adobe Analytics - Set zijn als volgt geconfigureerd.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Wanneer u klaar bent om uw code van Markeringen te testen,[de aangebrachte wijzigingen in de labels publiceren](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) de publicatiestroom gebruiken.