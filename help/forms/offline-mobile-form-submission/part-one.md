---
title: Trigger AEM workflow voor HTML5-formulierverzending - Aangepast profiel maken
description: Mobiel formulier blijven invullen in offline modus en mobiel formulier verzenden om AEM workflow te activeren
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Aangepast profiel maken

In dit deel zullen wij a [ douaneprofiel tot stand brengen.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Een profiel is verantwoordelijk voor het renderen van de XDP als HTML. Er wordt een standaardprofiel opgegeven in het vak voor het renderen van XDP&#39;s als HTML. Deze vertegenwoordigt een aangepaste versie van de Mobile Forms Rendition-service. Met de service Mobiele formulieruitvoering kunt u de weergave, het gedrag en de interacties van de Mobile Forms aanpassen. In ons aangepaste profiel leggen we de gegevens die in het mobiele formulier zijn ingevuld, vast met de API voor hulplijnen. Deze gegevens worden vervolgens naar een aangepaste servlet verzonden die vervolgens een interactieve PDF genereert en deze terugstuurt naar de aanroepende toepassing.

Haal de formuliergegevens op met de `formBridge` JavaScript API. We maken gebruik van de methode `getDataXML()` :

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

In de methode van de succesmanager maken wij een vraag aan douaneserlet die in AEM loopt. Deze servlet rendert en retourneert interactieve PDF met de gegevens van het mobiele formulier

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Interactieve PDF genereren

Hier volgt de servletcode die verantwoordelijk is voor het renderen van interactieve pdf en het retourneren van de pdf naar de aanroepende toepassing. servlet haalt `mobileFormToInteractivePdf` methode van de douaneDocumentServices OSGi dienst aan.

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
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
        // TODO Add proper error logging
      }
    }
}
```

### Interactieve PDF renderen

De volgende code maakt gebruik van [ de Dienst API van Forms ](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) om interactieve PDF met de gegevens van de mobiele vorm terug te geven.

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

Om de capaciteit te bekijken om interactieve PDF van gedeeltelijk voltooide mobiele vorm te downloaden, [ gelieve te klikken hier ](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Wanneer de PDF is gedownload, moet u de PDF indienen om een AEM workflow te activeren. Deze workflow voegt de gegevens van de verzonden PDF samen en genereert niet-interactieve PDF voor revisie.

Het aangepaste profiel dat voor dit gebruiksgeval is gemaakt, is beschikbaar als onderdeel van deze zelfstudie-elementen.
