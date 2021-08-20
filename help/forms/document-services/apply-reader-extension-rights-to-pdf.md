---
title: XDP weergeven in PDF met gebruiksrechten
description: Gebruiksrechten toepassen op pdf
version: 6.4,6.5
feature: Reader-extensies
topic: Ontwikkeling
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Reader-extensies toepassen

Met Reader Extensions kunt u gebruiksrechten voor PDF-documenten manipuleren. Gebruiksrechten hebben betrekking op functionaliteit die wel beschikbaar is in Acrobat, maar niet in Adobe Reader. De functionaliteit die wordt beheerd door Reader Extensions omvat de mogelijkheid om opmerkingen toe te voegen aan een document, formulieren in te vullen en het document op te slaan. PDF-documenten waaraan gebruiksrechten zijn toegevoegd, worden documenten waarvoor rechten zijn ingesteld genoemd. Een gebruiker die een PDF-document met ingeschakelde rechten opent in Adobe Reader, kan de bewerkingen uitvoeren die voor dat document zijn ingeschakeld.
Om dit vermogen te testen, kunt u dit [verbinding](https://forms.enablementadobe.com/content/forms/af/applyreaderextensions.html) proberen.

Voor dit gebruiksgeval moeten we het volgende doen:
* [Voeg het certificaat van de Uitbreidingen van de Reader ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) aan  `fd-service` gebruiker toe.

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

## Servlet maken om de PDF te streamen {#create-servlet-to-stream-the-pdf}

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
import java.util.Map;

import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.request.RequestParameterMap;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;

@Component(service = Servlet.class, property = {

        "sling.servlet.methods=post",

        "sling.servlet.paths=/bin/applyrights"

})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {

        @Reference
        ApplyUsageRights applyRights;
        Logger logger = LoggerFactory.getLogger(GetReaderExtendedPDF.class);

        public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
                try {
                        System.out.println("the submitted data is " + xmlString);
                        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                        InputSource is = new InputSource();
                        is.setCharacterStream(new StringReader(xmlString));

                        return db.parse(is);
                } catch (Exception e) {
                        logger.debug(e.getMessage());
                }
                return null;
        }
        @Override
        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }
        @Override
        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                System.out.println("In my do POST");
                UsageRights usageRights = new UsageRights();
                String submittedData = request.getParameter("jcr:data");
                org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
                usageRights.setEnabledDynamicFormFields(true);
                usageRights.setEnabledDynamicFormPages(true);
                usageRights.setEnabledFormDataImportExport(true);
                usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
                usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
                usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
                usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
                usageRights.setEnabledBarcodeDecoding(Boolean.valueOf(submittedXml.getElementsByTagName("barcode").item(0).getTextContent()));

                RequestParameterMap requestParameterMap = request.getRequestParameterMap();

                for (Map.Entry < String, RequestParameter[] > pairs: requestParameterMap.entrySet()) {
                        final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                        final org.apache.sling.api.request.RequestParameter param = pArr[0];
                        if (!param.isFormField()) {
                                try {
                                        System.out.println("Got attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);

                                        documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        documentToReaderExtend.close();
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

                                } catch (IOException e) {
                                        logger.debug("Exception in streaming pdf back to client  " + e.getMessage());
                                }
                        }

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


