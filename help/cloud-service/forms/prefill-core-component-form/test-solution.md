---
title: Adaptief formulier met kerncomponent vooraf invullen
description: Leer hoe u het adaptieve formulier vooraf instelt met gegevens
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14675
duration: 23
exl-id: a94deebd-e86e-4360-b0ed-193f13197ee2
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# De oplossing testen

Nadat de code is geïmplementeerd, maakt u een adaptief formulier op basis van kerncomponenten. Koppel het adaptieve formulier aan de Prefill-service, zoals in de onderstaande schermafbeelding wordt getoond.
![ prefill-service ](assets/pre-fill-service.png)

Telkens wanneer het formulier wordt gegenereerd, wordt de bijbehorende Prefill-service uitgevoerd en wordt het formulier gevuld met de gegevens die door de Prefill-service worden geretourneerd.

Bijvoorbeeld om de vorm met de gegevens vooraf in te vullen verbonden aan guid **d815a2b3-5f4c-4422-8197-d0b73479bf0e**, wordt volgende url gebruikt.
De code in de prefill dienst zal de waarde van guid parameter halen en zal de gegevens verbonden aan guid uit de gegevensbron halen.

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
