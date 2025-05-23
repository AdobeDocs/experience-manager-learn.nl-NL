---
title: Gebruikersrechten toepassen op geüploade pdf
description: Gebruiksrechten toepassen op pdf
version: Experience Manager 6.4, Experience Manager 6.5
feature: Reader Extensions
topic: Development
role: Developer
level: Experienced
exl-id: ea433667-81db-40f7-870d-b16630128871
last-substantial-update: 2020-07-07T00:00:00Z
duration: 129
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Reader-extensies toepassen

Met Reader Extensions kunt u gebruiksrechten op PDF-documenten manipuleren. Gebruiksrechten hebben betrekking op functionaliteit die wel beschikbaar is in Acrobat, maar niet in Adobe Reader. De functionaliteit die wordt beheerd door Reader Extensions omvat de mogelijkheid om opmerkingen toe te voegen aan een document, formulieren in te vullen en het document op te slaan. PDF-documenten waaraan gebruiksrechten zijn toegevoegd, worden documenten met ingeschakelde rechten genoemd. Een gebruiker die een PDF-document met ingeschakelde rechten opent in Adobe Reader, kan de bewerkingen uitvoeren die voor dat document zijn ingeschakeld.

Voor dit gebruiksgeval moeten we het volgende doen:
* [ voeg het certificaat van de Uitbreidingen van Reader ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=nl-NL) aan `fd-service` gebruiker toe.

## Aangepaste OSGi-service maken

Creeer de douaneOSGi dienst die gebruiksrechten op de documenten zal toepassen. De code om dit te bereiken wordt hieronder vermeld

```java
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = ApplyUsageRights.class)
public class ApplyUsageRights implements ReaderExtendPDF {
        @Reference
        DocAssuranceService docAssuranceService;
        @Reference
        GetResolver getResolver;
        Logger logger = LoggerFactory.getLogger(ApplyUsageRights.class);
        @Override
        public Document applyUsageRights(Document pdfDocument, UsageRights usageRights) {

                ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
                UnlockOptions unlockOptions = null;
                ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
                reOptions.setCredentialAlias("ares");

                reOptions.setResourceResolver(getResolver.getFormsServiceResolver());

                reOptions.setReOptions(reOptionsSpec);
                System.out.println("Applying Usage Rights");

                try {
                        Document readerExtended = docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
                                unlockOptions);
                        reOptions.getResourceResolver().close();
                        return readerExtended;
                } catch (Exception e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                return null;
        }

}
```

## servlet maken om de uitgebreide PDF van de lezer te streamen

De volgende stap bestaat uit het maken van een servlet met een POST-methode om de lezer die PDF voor de gebruiker heeft uitgebreid, terug te sturen. In dit geval wordt de gebruiker gevraagd de PDF op te slaan in zijn bestandssysteem. De reden hiervoor is dat de PDF wordt weergegeven als dynamisch PDF en dat de PDF-viewers die bij de browsers worden geleverd, geen dynamische PDF&#39;s verwerken.

Hier volgt de code voor de servlet. Het servlet-object wordt aangeroepen vanuit de aangepaste verzendactie van het adaptieve formulier.
Servlet maakt een object UsageRights en stelt dit object in op basis van de waarden die de gebruiker heeft ingevoerd in het adaptieve formulier. servlet roept dan de applyUsageRights methode van de dienst die voor dit doel wordt gecreeerd.

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

        private static final long serialVersionUID = -883724052368090823 L;
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
                                        System.out.println("Got form attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);
                                        if (logger.isDebugEnabled()) {
                                                documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        }

                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();

                                        response.setContentType("application/pdf");
                                        response.addHeader("Content-Disposition", "attachment; filename=" + param.getFileName().split("/")[1]);
                                        response.setContentLength((int) fileInputStream.available());
                                        OutputStream responseOutputStream = response.getOutputStream();
                                        documentToReaderExtend.close();
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

1. Voeg de volgende vermelding toe aan Apache Sling User Mapper Service met behulp van de configMgr console zoals hieronder wordt getoond

```
       DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

![ gebruiker-mapper ](assets/user-mapper-service.PNG)
1. [ Download en installeer de bundel ares.ares.core-ares ](assets/ares.ares.core-ares.jar). Dit heeft de douanedienst en servlet om gebruiksrechten toe te passen en pdf terug te stromen.
1. [Clientbibliotheken importeren en naar Aangepast verzenden](assets/applyaresdemo.zip)
1. [Het adaptieve formulier importeren](assets/applyaresform.zip)
1. Voeg het Reader Extensions-certificaat toe aan de gebruiker van de &quot;fd-service&quot;. Zorg ervoor alias is &quot;**aren**&quot;.
1. [ Voorproef Aangepaste Vorm ](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Selecteer de juiste rechten en upload het PDF-bestand
1. Klik op Verzenden om Reader Extended PDF op te halen
