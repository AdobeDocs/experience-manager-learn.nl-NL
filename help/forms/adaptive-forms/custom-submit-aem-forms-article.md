---
title: Een aangepaste verzending schrijven in AEM Forms
seo-title: Een aangepaste verzending schrijven in AEM Forms
description: Snelle en eenvoudige manier om uw eigen aangepaste verzendactie te maken voor adaptief formulier
seo-description: Snelle en eenvoudige manier om uw eigen aangepaste verzendactie te maken voor adaptief formulier
feature: Adaptieve Forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---


# Een aangepaste verzending schrijven in AEM Forms {#writing-a-custom-submit-in-aem-forms}

Snelle en eenvoudige manier om uw eigen aangepaste verzendactie te maken voor adaptief formulier

In dit artikel worden de stappen doorlopen die nodig zijn om een aangepaste verzendactie te maken voor de verwerking van Adaptive Forms-verzendingen.

* Aanmelden bij crx
* Maak een knooppunt van het type &quot;sling:folder&quot; onder apps. Laten we dit knooppunt CustomSubmitHelpx noemen.
* Sla het nieuwe knooppunt op.
* Voeg de volgende twee eigenschappen toe aan het nieuwe knooppunt
* PropertyName       | Waarde van eigendom
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:beschrijving   | CustomSubmitHelpx
* Sla de wijzigingen op
* Maak een nieuw bestand met de naam post.POST.jsp onder het knooppunt CustomSubmitHelpx. Wanneer een adaptief formulier wordt verzonden, wordt dit JSP aangeroepen. U kunt de JSP-code naar wens in dit bestand schrijven. De volgende code stuurt het verzoek door naar de servlet.

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* Maak het bestand addfields.jsp onder het knooppunt CustomSubmitHelpx. Met dit bestand hebt u toegang tot het ondertekende document.
* De volgende code toevoegen aan dit bestand

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Uw wijzigingen opslaan

Nu ziet u &quot;CustomSubmitHelpx&quot; in de verzendacties van uw Adaptief formulier, zoals in deze afbeelding wordt getoond.

![Adaptief formulier met Aangepast verzenden](assets/capture-2.gif)

