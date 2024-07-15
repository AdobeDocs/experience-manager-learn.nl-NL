---
title: Uw eerste OSGi-service met AEM Forms maken
description: Bouw uw eerste OSGi-service met AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
duration: 87
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# OSGi-service

Een dienst OSGi is een klasse of de dienstinterface van Java, samen met een aantal de diensteigenschappen als naam/waardeparen. De de diensteigenschappen onderscheiden zich onder verschillende dienstverleners die de diensten van de zelfde de dienstinterface verlenen.

De dienst OSGi wordt semantisch bepaald door zijn de dienstinterface en als de dienstvoorwerp uitgevoerd. De functionaliteit van de dienst wordt bepaald door de interfaces het uitvoert. Dientengevolge, kunnen de verschillende toepassingen de zelfde dienst uitvoeren. De interfaces van de dienst staan bundels toe om door bindende interfaces, niet implementaties in wisselwerking te staan. Een de dienstinterface zou met zo weinig implementatiedetails moeten worden gespecificeerd mogelijk.

## De interface definiëren

Een eenvoudige interface met één methode om gegevens met het <span class="x x-first x-last"> XDP </span> malplaatje samen te voegen.

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## Implementeer de interface

Maak een nieuw pakket met de naam `com.mysite.samples.impl` voor de implementatie van de interface.

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

De aantekening `@Component(...)` op regel 10 maakt deze Java-klasse gemarkeerd als een OSGi-component en registreert deze als een OSGi-service.

De `@Reference` aantekening maakt deel uit van de verklarende diensten OSGi, en wordt gebruikt om een verwijzing van [ OutputService ](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) in veranderlijk `outputService` te injecteren.


## De bundel maken en implementeren

* Open **bevel snel venster**
* Navigeren naar `c:\aemformsbundles\mysite\core`
* De opdracht uitvoeren `mvn clean install -PautoInstallBundle`
* Het bovenstaande bevel zal automatisch de bundel aan uw AEM instantie bouwen en opstellen die op localhost loopt:4502

De bundel is ook beschikbaar op de volgende locatie `C:\AEMFormsBundles\mysite\core\target` . De bundel kan ook in AEM worden opgesteld gebruikend de [ het Webconsole van de Felix.](http://localhost:4502/system/console/bundles)

## De service gebruiken

U kunt de dienst in uw JSP pagina nu gebruiken. Het volgende codefragment toont hoe te om toegang tot uw dienst te krijgen en de methodes te gebruiken die door de dienst worden uitgevoerd

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

Het steekproefpakket dat de JSP pagina bevat kan [ van hier worden gedownload ](assets/learning_aem_forms.zip)

[De volledige bundel kan worden gedownload](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## De verpakking testen

De invoer en installeert het pakket in AEM gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)

Gebruik postman om een POST aan te roepen en de invoerparameters te verstrekken zoals hieronder getoond in het scherm ontsproten
![ postman ](assets/test-service-postman.JPG)

## Volgende stappen

[Sling Servlet maken](./create-servlet.md)

