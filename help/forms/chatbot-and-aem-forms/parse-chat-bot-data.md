---
title: AEM Forms gebruiken met Chatbot
description: Parse ChatBot-gegevens
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 3c304b0a-33f8-49ed-a576-883df4759076
duration: 22
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 0%

---

# Parse ChatBot-gegevens

A [&#x200B; Web-haak ChatBot &#x200B;](https://www.chatbot.com/help/webhooks/what-are-webhooks/) werd gebruikt om de gegevens ChatBot naar een servlet van AEM te verzenden.
De gegevens die in de ChatBot worden vastgelegd, hebben de JSON-indeling, waarbij de gebruiker gegevens in het attributenobject heeft ingevoerd, zoals hieronder wordt weergegeven
![&#x200B; chatbot-data &#x200B;](assets/chat-bot-data.png)

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

![&#x200B; xdp-malplaatje &#x200B;](assets/xdp-template.png)

## Volgende stappen

[Gegevens samenvoegen met XDP-sjabloon](./merge-data-with-template.md)
