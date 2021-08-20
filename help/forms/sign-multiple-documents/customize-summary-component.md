---
title: Samenvattingscomponent aanpassen
description: Breid de overzichtsstapcomponent uit om de mogelijkheid te omvatten om naar het volgende formulier in het pakket te navigeren.
feature: Adaptieve Forms
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---


# Samenvattingsstap aanpassen

De component Samenvattingsstap wordt gebruikt om de samenvatting van uw formulierverzending weer te geven met een koppeling om het ondertekende formulier te downloaden. De samenvattingsstap wordt doorgaans in het laatste deelvenster van het formulier geplaatst.
Voor dit gebruiksgeval hebben wij een nieuwe component gecreeerd die op uit de uit component van de doosSamenvatting wordt gebaseerd en het vermogen uitgebreid om douaneclib te omvatten.

Deze component wordt geïdentificeerd door het label Meerdere formulieren ondertekenen

Het volgende het schermschot toont de nieuwe component die werd gecreeerd om het bericht na de het ondertekenen ceremonie te tonen

![summary-component](assets/summary.PNG)

De nieuwe component is gebaseerd op de overzichtscomponent van het vak.
![componentprop](assets/componentprop.PNG)

Er is een knop toegevoegd waarmee u naar het volgende formulier kunt navigeren voor ondertekening
![sjablooncode](assets/template-code.PNG)

summary.jsp heeft de volgende code. Het heeft verwijzing naar de cliëntbibliotheek die door categorie id **getnextform** wordt geïdentificeerd

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

De aangepaste summiere component kan [hier worden gedownload](assets/custom-summary-step.zip)


