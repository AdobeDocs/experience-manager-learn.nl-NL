---
title: Opgeslagen adaptief formulier ophalen
description: Servlet om het adaptieve formulier te genereren met opgeslagen gegevens
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6553
thumbnail: 6553.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d722cb9c-6c8a-44de-aaea-fc07a555b864
duration: 66
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Opgeslagen formulier ophalen

De volgende stap bestaat uit het maken van een servlet waarmee het adaptieve formulier wordt weergegeven met de opgeslagen gegevens en de bijbehorende bijlagen.
De volgende servletcode wordt uitgevoerd nadat de code OTP is geverifieerd. De adaptieve formuliergegevens en de bijbehorende bestandsbijlagen die aan de unieke toepassings-id zijn gekoppeld, worden opgehaald uit de database. Het aanvraagobject wordt gevuld met de opgeslagen adaptieve formuliergegevens en de bestandsbijlagen die zijn toegewezen. Het verzoek wordt vervolgens doorgestuurd om het formulier &quot;storeafwithattachments&quot; te genereren met de oorspronkelijke gegevens en de bijlagen.

```java
import java.io.IOException;
import java.io.StringReader;
import java.util.Enumeration;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.xml.sax.InputSource;

import com.google.gson.JsonObject;
import com.saveAndResume.core.SaveAndFetchDataFromDB;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/renderaf"
})

public class RenderFormWithDataAndAttachments extends SlingAllMethodsServlet {
  @Reference
  SaveAndFetchDataFromDB saveAndFetchFromDB;
  Logger log = LoggerFactory.getLogger(this.getClass());

  public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
    try {
      log.debug("Inside w3cDocumentFromString");
      DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
      InputSource is = new InputSource();
      is.setCharacterStream(new StringReader(xmlString));
      return db.parse(is);
    } catch (Exception e) {
      log.error(e.getMessage());
      return null;
    }
  }
  protected void doPost(final SlingHttpServletRequest request, final SlingHttpServletResponse response) throws ServletException {
    log.debug("In do POST ######");
    String applicationNo = "/afData/afUnboundData/data/ApplicationNumber";
    String submittedData = request.getParameter("jcr:data");
    Document submittedXml = this.w3cDocumentFromStrng(submittedData);
    XPath xPath = XPathFactory.newInstance().newXPath();
    Enumeration < String > params = request.getParameterNames();
    while (params.hasMoreElements()) {
      String paramName = params.nextElement();
      log.debug("The param Name is " + paramName);
      String data = request.getParameter(paramName);
      log.debug("The data  is " + data);
    }

    SyntheticSlingHttpServletGetRequest syntheticRequest = new SyntheticSlingHttpServletGetRequest(request);
    try {
      Node applicationNode = (Node) xPath.compile(applicationNo).evaluate(submittedXml, XPathConstants.NODE);
      log.debug("The application number we got was " + applicationNode.getTextContent());
      JsonObject afDataObject = saveAndFetchFromDB.getAFFormDataWithAttachments(applicationNode.getTextContent());
      log.debug("$$$$ The data that is set in request object is " + afDataObject.get("afData").getAsString());
      request.setAttribute("data", afDataObject.get("afData").getAsString());
      JsonObject customMap = new JsonObject();
      customMap.addProperty("fileAttachmentMap", afDataObject.get("afAttachments").getAsString());
      request.setAttribute("customContextProperty", customMap.toString());
      request.getRequestDispatcher("/content/forms/af/storeafwithattachments.html").forward(syntheticRequest, response);
    } catch (ServletException | IOException | XPathExpressionException exception) {

       log.error(exception.getMessage());
    }
  }

}
```

## SyntheticSlingHttpServletGetRequest

```java
package saveandresume.core.servlets;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.wrappers.SlingHttpServletRequestWrapper;

public class SyntheticSlingHttpServletGetRequest extends SlingHttpServletRequestWrapper {
    private static final String METHOD_GET = "GET";

    public SyntheticSlingHttpServletGetRequest(final SlingHttpServletRequest request) {
        super(request);
    }

    @Override
    public String getMethod() {
        return METHOD_GET;
    }
}
```


## Volgende stappen

[Client lib maken om de servlet aan te roepen om formuliergegevens op te slaan](./create-client-lib.md)
