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
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# OSGi-service

Een dienst OSGi is een klasse of de dienstinterface van Java, samen met een aantal de diensteigenschappen als naam/waardeparen. De de diensteigenschappen onderscheiden zich onder verschillende dienstverleners die de diensten van de zelfde de dienstinterface verlenen.

De dienst OSGi wordt semantisch bepaald door zijn de dienstinterface en als de dienstvoorwerp uitgevoerd. De functionaliteit van de dienst wordt bepaald door de interfaces het uitvoert. Dientengevolge, kunnen de verschillende toepassingen de zelfde dienst uitvoeren. De interfaces van de dienst staan bundels toe om door bindende interfaces, niet implementaties in wisselwerking te staan. Een de dienstinterface zou met zo weinig implementatiedetails moeten worden gespecificeerd mogelijk.

## De interface definiëren

Een eenvoudige interface met één methode om gegevens samen te voegen met de <span class="x x-first x-last">XDP</span> sjabloon.

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## Implementeer de interface

Een nieuw pakket maken met de naam `com.mysite.samples.impl` om de implementatie van de interface te houden.

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

De annotatie `@Component(...)` op lijn 10 maakt merken deze klasse van Java als Component OSGi evenals registreert het als Dienst OSGi.

De `@Reference` De annotatie maakt deel uit van de verklarende diensten van OSGi, en wordt gebruikt om een verwijzing van te injecteren [Uitvoerservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) in de variabele `outputService`.


## De bundel maken en implementeren

* Openen **opdrachtpromptvenster**
* Ga naar `c:\aemformsbundles\mysite\core`
* De opdracht uitvoeren `mvn clean install -PautoInstallBundle`
* Het bovenstaande bevel zal automatisch de bundel aan uw AEM instantie bouwen en opstellen die op localhost loopt:4502

De bundel is ook beschikbaar op de volgende locatie `C:\AEMFormsBundles\mysite\core\target`. De bundel kan ook in AEM worden opgesteld gebruikend [Felix-webconsole.](http://localhost:4502/system/console/bundles)

## De service gebruiken

U kunt de dienst in uw JSP pagina nu gebruiken. Het volgende codefragment toont hoe te om toegang tot uw dienst te krijgen en de methodes te gebruiken die door de dienst worden uitgevoerd

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

Het voorbeeldpakket met de JSP-pagina kan [hier gedownload](assets/learning_aem_forms.zip)

[De volledige bundel kan worden gedownload](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## De verpakking testen

Importeer en installeer het pakket in AEM met de [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)

Gebruik postman om een POST aan te roepen en de invoerparameters te verstrekken zoals die in het hieronder ontsproten scherm worden getoond
![postbode](assets/test-service-postman.JPG)

## Volgende stappen

[Sling Servlet maken](./create-servlet.md)

