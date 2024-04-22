---
title: AEM Forms gebruiken met Chatbot
description: Parse ChatBot-gegevens
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 0%

---

# Parse ChatBot-gegevens

A [ChatBot-webhaak](https://www.chatbot.com/help/webhooks/what-are-webhooks/) is gebruikt om de ChatBot-gegevens naar een AEM servlet te verzenden.
De gegevens die in de ChatBot worden vastgelegd, hebben de JSON-indeling, waarbij de gebruiker gegevens in het attributenobject heeft ingevoerd, zoals hieronder wordt weergegeven
![chatbogegevens](assets/chat-bot-data.png)

Als u de gegevens wilt samenvoegen met de XDP-sjabloon, moet u de volgende XML maken. Merk het wortelelement van xml op, dit moet met het wortelelement van het malplaatje XDP voor de gegevens aanpassen om met succes samen te voegen.


```xml
<topmostSubForm>
    <f1_01>David Smith</f1_01>
    <signmethod>ESIGN</signmethod>
    <corporation>1</corporation>
    <f1_08>San Jose, CA, 95110</f1_08>
    <f1_07>345 Park Avenue</f1_07>
    <ssn>123-45-6789</ssn>
    <form_name>W-9</form_name>
</topmostSubForm>
```

![xdp-template](assets/xdp-template.png)

## Volgende stappen

[Gegevens samenvoegen met XDP-sjabloon](./merge-data-with-template.md)



