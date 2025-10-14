---
title: QR-code in adaptieve vorm weergeven
description: QR-code weergeven in een adaptief formulier
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
exl-id: 0c6079f4-601e-4a82-976c-71dbb2faa671
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Voorbeeld QR-codecomponent

Het insluiten van een QR-code in een adaptief formulier kan het voor gebruikers veel gemakkelijker en efficiënter maken om toegang te krijgen tot aanvullende informatie over het formulier.

De steekproefcomponent maakt gebruik van [&#x200B; QRCode.js &#x200B;](https://davidshimjs.github.io/qrcodejs/).

QRCode.js is een javascript bibliotheek voor het maken van QRCode, steunt het dwars-browser met HTML5 Canvas en lijstmarkering in DOM.

De component produceert de code QR die op de waarde wordt gebaseerd in het configuratiebezit van de component wordt gespecificeerd.
![afbeelding](assets/qr-code-url.png)

De volgende code is gebruikt in body.jsp van de qr-code-generator component.

De &quot;url&quot; is de URL die moet worden ingesloten in de Qr-code. Deze url wordt gespecificeerd in de configuratieeigenschappen van de QR codecomponent.

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



De volgende code maakt gebruik van de makeCode methode van de bibliotheek QRCode.js in de cliëntbibliotheek van de qr-code-generator component.De geproduceerde QR code wordt toegevoegd aan div die door identiteitskaart **&quot;qrcode&quot;** wordt geïdentificeerd.

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## De elementen op uw lokale server implementeren

* [Download en installeer de component QR-code met Package Manager.](assets/qrcode.zip)
* [Download en installeer het voorbeeldadaptieve formulier met Package Manager.](assets/form-with-qr-code.zip)
* [&#x200B; voorproef de vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled). De Help-sectie van het formulier heeft de QR-code.
