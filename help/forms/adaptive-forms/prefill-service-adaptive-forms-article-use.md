---
title: Prefill-service in adaptieve Forms
description: Aangepaste formulieren vooraf invullen door gegevens op te halen uit achterwaartse gegevensbronnen.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
duration: 129
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# Prefill-service gebruiken in Adaptive Forms

U kunt de velden van een adaptief formulier vooraf invullen met bestaande gegevens. Wanneer een gebruiker een formulier opent, worden de waarden voor die velden vooraf ingevuld. Er zijn meerdere manieren om aangepaste formuliervelden vooraf in te vullen. In dit artikel bekijken we het vooraf ingevulde adaptieve formulier met de AEM Forms Prefill-service.

Om meer over diverse methodes te leren om adaptieve vormen vooraf in te vullen, [&#x200B; te volgen gelieve deze documentatie &#x200B;](https://helpx.adobe.com/nl/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Als u een adaptief formulier wilt vooraf invullen met de Prefill-service, moet u een klasse maken die de interface `com.adobe.forms.common.service.DataXMLProvider` implementeert. De methode `getDataXMLForDataRef` bevat de logica voor het samenstellen en retourneren van gegevens die het aangepaste formulier gebruikt om de velden vooraf in te vullen. Met deze methode kunt u de gegevens van elke bron ophalen en de invoerstream van het gegevensdocument retourneren. De volgende voorbeeldcode haalt de gebruikersprofielinformatie van de aangemelde gebruiker op en maakt een XML-document waarvan de invoerstream wordt geretourneerd voor gebruik door de adaptieve formulieren.

In het codefragment hieronder hebben we een klasse die de DataXMLProvider-interface implementeert. We krijgen toegang tot de aangemelde gebruiker en halen vervolgens de profielgegevens van de aangemelde gebruiker op. Vervolgens maken we een XML-document met het basisknooppuntelement &#39;data&#39; en voegen we de juiste elementen toe aan dit gegevensknooppunt. Wanneer het XML-document is samengesteld, wordt de invoerstream van het XML-document geretourneerd.

Deze klasse wordt dan gemaakt in bundel OSGi en opgesteld in AEM. Zodra de bundel wordt opgesteld, is deze prefill dienst beschikbaar om als prefill dienst van uw Aangepast Vorm te worden gebruikt.

>[!NOTE]
>
>U kunt het formulier vooraf invullen met XML- of JSON-gegevens op de manier die in dit artikel wordt beschreven.

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

Voer het volgende uit om deze mogelijkheid op uw server te testen

* Zorg ervoor het het programma geopende [&#128279;](http://localhost:4502/security/users.html) het profiel van de gebruiker  informatie wordt ingevuld. Het voorbeeld zoekt naar de eigenschappen FirstName, LastName en Email van de aangemelde gebruiker.
* [Download en extraheer de inhoud van het ZIP-bestand naar uw computer](assets/prefillservice.zip)
* Stel de prefill.core-1.0.0-SNAPSHOT bundel op gebruikend de [&#x200B; het Webconsole van AEM &#x200B;](http://localhost:4502/system/console/bundles)
* Het adaptieve formulier importeren met het dialoogvenster Maken | Het dossier uploadt van de [&#x200B; sectie FormsAndDocuments &#x200B;](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Zorg ervoor de [&#x200B; vorm &#x200B;](http://localhost:4502/editor.html/content/forms/af/prefill.html) **&quot;De Dienst van de Prevulling van AEM Forms van de Douane&quot;** als prefill dienst gebruikt. Dit kan van de configuratieeigenschappen van de **sectie van de Container van de Vorm** worden geverifieerd.
* [&#x200B; voorproef de vorm &#x200B;](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). Het formulier moet de juiste waarden bevatten.

>[!NOTE]
>
>Als u foutopsporing hebt ingeschakeld voor com.aem.prefill.core.PrefillAdaptiveForm, wordt het gegenereerde XML-gegevensbestand geschreven naar de installatiemap van de AEM-server.

