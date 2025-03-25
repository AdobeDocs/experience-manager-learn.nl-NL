---
title: Samenvattingscomponent aanpassen
description: Breid de overzichtsstapcomponent uit om de mogelijkheid te omvatten om naar het volgende formulier in het pakket te navigeren.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
duration: 38
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Samenvattingsstap aanpassen

De component Samenvattingsstap wordt gebruikt om de samenvatting van uw formulierverzending weer te geven met een koppeling om het ondertekende formulier te downloaden. De samenvattingsstap wordt doorgaans in het laatste deelvenster van het formulier geplaatst.
Voor dit gebruiksgeval hebben wij een nieuwe component gecreeerd die op uit de uit component van de doosSamenvatting wordt gebaseerd en het vermogen uitgebreid om douaneclib te omvatten.

Deze component wordt geïdentificeerd door het label Meerdere formulieren ondertekenen

Het volgende het schermschot toont de nieuwe component die werd gecreeerd om het bericht na de het ondertekenen ceremonie te tonen

![ summiere component ](assets/summary.PNG)

De nieuwe component is gebaseerd op de overzichtscomponent van het vak.
![ component-prop ](assets/componentprop.PNG)

Er is een knop toegevoegd waarmee u naar het volgende formulier kunt navigeren voor ondertekening
![ malplaatje-code ](assets/template-code.PNG)

De summary.jsp heeft de volgende code. Het heeft verwijzing naar de cliëntbibliotheek die door categorie identiteitskaart **wordt geïdentificeerd getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

De douane summiere component kan [ van hier worden gedownload ](assets/custom-summary-step.zip)

## Volgende stappen

[Het volgende formulier ophalen voor ondertekening](./create-client-lib.md)