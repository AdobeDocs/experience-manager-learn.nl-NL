---
title: Samenvattingscomponent aanpassen
description: Breid de overzichtsstapcomponent uit om de mogelijkheid te omvatten om naar het volgende formulier in het pakket te navigeren.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
duration: 43
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Samenvattingsstap aanpassen

De component Samenvattingsstap wordt gebruikt om de samenvatting van uw formulierverzending weer te geven met een koppeling om het ondertekende formulier te downloaden. De samenvattingsstap wordt doorgaans in het laatste deelvenster van het formulier geplaatst.
Voor dit gebruiksgeval hebben wij een nieuwe component gecreeerd die op uit de uit component van de doosSamenvatting wordt gebaseerd en het vermogen uitgebreid om douaneclib te omvatten.

Deze component wordt geïdentificeerd door het label Meerdere formulieren ondertekenen

Het volgende het schermschot toont de nieuwe component die werd gecreeerd om het bericht na de het ondertekenen ceremonie te tonen

![overzichtscomponent](assets/summary.PNG)

De nieuwe component is gebaseerd op de overzichtscomponent van het vak.
![componentprop](assets/componentprop.PNG)

Er is een knop toegevoegd waarmee u naar het volgende formulier kunt navigeren voor ondertekening
![sjablooncode](assets/template-code.PNG)

De summary.jsp heeft de volgende code. Er wordt verwezen naar de clientbibliotheek die wordt aangeduid door de categorie-id **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

De aangepaste overzichtscomponent kan [hier gedownload](assets/custom-summary-step.zip)

## Volgende stappen

[Het volgende formulier ophalen voor ondertekening](./create-client-lib.md)