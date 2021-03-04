---
title: XDP weergeven in PDF met gebruiksrechten
seo-title: XDP weergeven in PDF met gebruiksrechten
description: Gebruiksrechten toepassen op pdf
seo-description: Gebruiksrechten toepassen op pdf
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: Forms Service
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---


# Reader-extensies toepassen

Met Reader Extensions kunt u gebruiksrechten voor PDF-documenten manipuleren. Gebruiksrechten hebben betrekking op functionaliteit die wel beschikbaar is in Acrobat, maar niet in Adobe Reader. De functionaliteit die wordt beheerd door Reader Extensions omvat de mogelijkheid om opmerkingen toe te voegen aan een document, formulieren in te vullen en het document op te slaan. PDF-documenten waaraan gebruiksrechten zijn toegevoegd, worden documenten waarvoor rechten zijn ingesteld genoemd. Een gebruiker die een PDF-document met ingeschakelde rechten opent in Adobe Reader, kan de bewerkingen uitvoeren die voor dat document zijn ingeschakeld.
Om dit vermogen te testen, kunt u dit [verbinding](https://forms.enablementadobe.com/content/samples/samples.html?query=0) proberen. De voorbeeldnaam is &quot;Render XDP with RE&quot;

Voor dit gebruiksgeval moeten we het volgende doen:
* Voeg het certificaat van de Uitbreidingen van de Reader aan &quot;fd-dienst&quot;gebruiker toe. De stappen om de credentie van de Uitbreidingen van de Reader toe te voegen zijn vermeld [hier](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)

* Creeer de douaneOSGi dienst die gebruiksrechten op de documenten zal toepassen. De code om dit te bereiken wordt hieronder vermeld

```java
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service=ApplyUsageRights.class,immediate = true)
public class ApplyUsageRights implements ReaderExtendPDF {
@Reference
DocAssuranceService docAssuranceService;
@Reference
GetResolver getResolver;
@Override
public Document applyUsageRights(Document pdfDocument,UsageRights usageRights) {
      ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
      UnlockOptions unlockOptions = null;
      ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
      reOptions.setCredentialAlias("ares");
      reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
      reOptions.setReOptions(reOptionsSpec);
    try {
          return docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
          unlockOptions);
        } catch (Exception e) {
            e.printStackTrace();
        }
    return null;
}

}
```

## Servlet maken om de PDF te stroomden {#create-servlet-to-stream-the-pdf}

De volgende stap bestaat uit het maken van een servlet met een methode van de POST om de lezer uitgebreide PDF aan de gebruiker terug te geven. In dit geval wordt de gebruiker gevraagd de PDF op te slaan in zijn bestandssysteem. De reden hiervoor is dat de PDF wordt gerenderd als dynamische PDF en de PDF-viewers die bij de browsers worden geleverd, geen dynamische PDF&#39;s verwerken.

Hier volgt de code voor de servlet. De servlet wordt aangeroepen vanuit de handeling **customsubmit** van Adaptief formulier.
Servlet maakt een object UsageRights en stelt dit object in op basis van de waarden die de gebruiker heeft ingevoerd in het adaptieve formulier. De servlet roept dan **applyUsageRights** methode van de dienst die voor dit doel wordt gecreeerd.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Enumeration;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.PathNotFoundException;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFormatException;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = Servlet.class, property = {
sling.servlet.methods=get", "sling.servlet.paths=/bin/applyrights"
})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {
  @Reference
  GetResolver getResolver;
  @Reference
  ApplyUsageRights applyRights;
  public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
  try {
        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
        InputSource is = new InputSource();
        is.setCharacterStream(new StringReader(xmlString));
  return db.parse(is);
} catch (ParserConfigurationException e) {
    e.printStackTrace();
} catch (SAXException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
return null;
}
@Override
protected void doGet(SlingHttpServletRequest request,SlingHttpServletResponse response)
{
  doPost(request,response);
}
@Override
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  UsageRights usageRights = new UsageRights();
  String submittedData = request.getParameter("jcr:data");
  org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
  String formFill = submittedXml.getElementsByTagName("formfill").item(0).getTextContent();
  usageRights.setEnabledFormDataImportExport(true);
  usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
  usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
  usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
  usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
  String attachmentPath = submittedXml.getElementsByTagName("attachmentpath").item(0).getTextContent();
  Node pdfNode = getResolver.getFormsServiceResolver().getResource(attachmentPath).adaptTo(Node.class);
  javax.jcr.Node jcrContent = null;
try {
    jcrContent = pdfNode.getNode("jcr:content");
    } catch (RepositoryException e1) {
      e1.printStackTrace();
    }

try {
    InputStream is = jcrContent.getProperty("jcr:data").getBinary().getStream();
    Session jcrSession = getResolver.getFormsServiceResolver().adaptTo(Session.class);
    Document documentToReaderExtend = new Document(is);
    documentToReaderExtend =  applyRights.applyUsageRights(documentToReaderExtend,usageRights);
    documentToReaderExtend.copyToFile(new File("c:\\scrap\\doctore.pdf"));
    InputStream fileInputStream = documentToReaderExtend.getInputStream();
    documentToReaderExtend.close();
    response.setContentType("application/pdf");
    response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
    response.setContentLength((int) fileInputStream.available());
    OutputStream responseOutputStream = response.getOutputStream();
    int bytes;
    while ((bytes = fileInputStream.read()) != -1) {
      responseOutputStream.write(bytes);
    }
    responseOutputStream.flush();
    responseOutputStream.close();
  } catch (ValueFormatException e) {
      e.printStackTrace();
  } catch (PathNotFoundException e) {
      e.printStackTrace();
  } catch (RepositoryException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}

}

}
```

Voer de volgende stappen uit om dit op uw lokale server te testen:
1. [Download en installeer de DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Download en installeer de ares.ares.core-ares Bundle](assets/ares.ares.core-ares.jar). Dit heeft de douanedienst en servlet om gebruiksrechten toe te passen en pdf terug te stromen
1. [Clientbibliotheken importeren en naar Aangepast verzenden](assets/applyaresdemo.zip)
1. [Het adaptieve formulier importeren](assets/applyaresform.zip)
1. Certificaat van extensies voor Readers toevoegen aan gebruiker van het type fd-service
1. [Voorbeeld van adaptief formulier](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Selecteer de juiste rechten en upload het PDF-bestand
1. Klik op Verzenden om Reader Extended PDF op te halen



