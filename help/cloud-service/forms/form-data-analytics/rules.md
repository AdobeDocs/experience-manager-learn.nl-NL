---
title: Ingevulde formuliergegevensvelden rapporteren met Adobe Analytics
description: AEM Forms CS met Adobe Analytics integreren om formuliergegevensvelden te rapporteren
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# De regel definiëren

In het bezit van Markeringen creeerden wij 2 nieuwe [ regels ](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**de Fout van de Bevestiging van het Gebied en FormSubmit**).

![ adaptive-form ](assets/rules.png)


## Veldvalidatiefout

De **regel van de Fout van de Bevestiging van het Gebied** wordt teweeggebracht telkens als er bevestigingsfout op adaptief vormgebied is. Als het telefoonnummer of de e-mail bijvoorbeeld in ons formulier niet de verwachte indeling heeft, wordt een foutbericht voor validatie weergegeven.

De regel van de Fout van de Bevestiging van het Gebied wordt gevormd door de gebeurtenis aan _&#x200B;**Adobe Experience Manager Forms-Fout**&#x200B;_ zoals aangetoond in het het schermschot te plaatsen



![ aanvrager-staat-verblijf ](assets/field_validation_error_rule.png)

De Adobe Analytics - de Vastgestelde Variabelen worden gevormd als volgt

![ vastgestelde actie ](assets/field_validation_action_rule.png)

## Regel voor verzenden van formulier

De regel Formulier verzenden wordt geactiveerd telkens wanneer een adaptief formulier is verzonden.

De Vorm legt regel voor wordt gevormd gebruikend _&#x200B;**Adobe Experience Manager Forms - voorlegt**&#x200B;_ gebeurtenis

![ vorm-voorlegt-regel ](assets/form-submit-rule.png)

In de Vorm legt regel voor, wordt de waarde van het gegevenselement _&#x200B;**ApplicantsStateOfResidence**&#x200B;_ in kaart gebracht aan prop5 en de waarde van het gegevenselement FormTitle wordt in kaart gebracht aan prop8.

De variabelen Adobe Analytics - Set zijn als volgt geconfigureerd.
![ vorm-voorlegt-regel-reeks-variabelen ](assets/form-submit-set-variable.png)

Wanneer u bereid bent om uw code van Markeringen te testen, [ publiceer uw veranderingen die aan de markeringen ](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) worden aangebracht gebruikend de het publiceren stroom.

## Volgende stappen

[De oplossing testen](./test.md)