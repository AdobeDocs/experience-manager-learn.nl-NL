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
feature: forms-service
discoiquuid: aefb4124-91a0-4548-94a3-86785ea04549
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---


# XDP weergeven in PDF met gebruiksrechten{#rendering-xdp-into-pdf-with-usage-rights}

Een veelvoorkomend geval is het renderen van xdp naar PDF en het toepassen van Reader Extensions op de gerenderde PDF.

Als een gebruiker bijvoorbeeld op XDP klikt in de portal Formulieren van AEM Forms, kunnen we XDP weergeven als PDF en de PDF door Reader uitbreiden.

U kunt deze mogelijkheid testen door deze [koppeling](https://forms.enablementadobe.com/content/samples/samples.html?query=0)te proberen. De voorbeeldnaam is &quot;Render XDP with RE&quot;

Voor dit gebruiksgeval moeten we het volgende doen.

* Voeg het certificaat van de Uitbreidingen van de Reader aan &quot;fd-dienst&quot;gebruiker toe. De stappen om de credentie van de Uitbreidingen van de Reader toe te voegen zijn [hier vermeld](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)

* Creeer de douanedienst OSGi die gebruiksrechten zal teruggeven en toepassen. De code om dit te bereiken wordt hieronder vermeld

## XDP renderen en gebruiksrechten toepassen {#render-xdp-and-apply-usage-rights}

* Regel 7: Met de renderPDFForm van FormsService genereren we PDF van XDP.

* Lijnen 8-14: De juiste gebruiksrechten worden ingesteld. Deze gebruiksrechten worden opgehaald van de OSGi-configuratiemontages.

* Regel 20: Gebruik resourceresolver verbonden aan de dienst van de de dienstgebruiker

* Regel 24: De methode secureDocument van DocumentAssuranceService wordt gebruikt om de gebruiksrechten toe te passen

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

Het volgende schermschot toont u de configuratieeigenschappen blootgesteld. De meeste algemene gebruiksrechten worden via deze configuratie weergegeven.

![](assets/configurationproperties.gif)

De volgende code toont u de code die wordt gebruikt om de OSGi configuratiemontages te bouwen

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## Servlet maken om de PDF te streamen {#create-servlet-to-stream-the-pdf}

De volgende stap bestaat uit het maken van een servlet met een methode GET om de voor lezer uitgebreide PDF naar de gebruiker te retourneren. In dit geval wordt de gebruiker gevraagd de PDF op te slaan in zijn bestandssysteem. De reden hiervoor is dat de PDF wordt gerenderd als dynamische PDF en de PDF-viewers die bij de browsers worden geleverd, geen dynamische PDF&#39;s verwerken.

Hier volgt de code voor de servlet. We geven het pad van de XDP in de CRX-opslagruimte door aan deze servlet.

Vervolgens roept u de methode renderAndExtendXdp van com.aemformssamples.documentservices.core.DocumentServices aan.

Het uitgebreide PDF-bestand van de lezer wordt vervolgens gestreamd naar de opvragende toepassing

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

Voer de volgende stappen uit om dit op uw lokale server te testen
1. [Download en installeer de DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [De AEMFormsDocumentServices-bundel downloaden en installeren](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [De aan dit artikel gerelateerde elementen downloaden en importeren in AEM met behulp van pakketbeheer](assets/renderandextendxdp.zip)
   * Dit pakket bevat voorbeeldportal en xdp-bestand
1. Certificaat van extensies voor Readers toevoegen aan gebruiker van het type fd-service
1. Webpagina&#39;s [porteren naar uw browser](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. Klik op het pdf-pictogram om de xdp te renderen en pdf te krijgen die Reader Extended is



