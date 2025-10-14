---
title: AEM-workflow activeren bij het verzenden van HTML5-formulieren - Aangepast profiel maken
description: Een aangepast profiel maken om een interactieve PDF te downloaden met de gegevens van het gedeeltelijk gevulde HTML5-formulier
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# Aangepast profiel maken

In dit deel zullen wij a [&#x200B; douaneprofiel tot stand brengen.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Een profiel is verantwoordelijk voor het renderen van de XDP als HTML. Er wordt een standaardprofiel opgegeven in het vak voor het renderen van XDP&#39;s als HTML. Deze vertegenwoordigt een aangepaste versie van de Mobile Forms Rendition-service. Met de service Mobiele formulieruitvoering kunt u de weergave, het gedrag en de interacties van de Mobile Forms aanpassen. In ons aangepaste profiel leggen we de gegevens die in het mobiele formulier zijn ingevuld, vast met de API voor hulplijnen. Deze gegevens worden vervolgens verzonden naar een aangepaste servlet die vervolgens een interactieve PDF genereert en deze terugstuurt naar de aanroepende toepassing.

Haal de formuliergegevens op met de `formBridge` JavaScript API. We maken gebruik van de methode `getDataXML()` :

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

In de methode van de succesmanager richten wij een vraag aan douaneserlet die in AEM loopt. Deze servlet rendert en retourneert interactieve PDF met de gegevens van het mobiele formulier

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    let postURL ="/bin/generateinteractivepdf";
    console.log("The data: " + data);
    xhr.open('POST',postURL);
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    let parts = window.location.pathname.split("/");
    let formName = parts[parts.length-2];
    const updatedFilename = formName.replace(/\.xdp$/, '.pdf');

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
                a.download = updatedFilename;
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Interactieve PDF genereren

Hier volgt de servletcode die verantwoordelijk is voor het renderen van interactieve pdf en het retourneren van de pdf naar de aanroepende toepassing. servlet haalt `mobileFormToInteractivePdf` methode van de douaneDocumentServices OSGi dienst aan.

```java
package com.aemforms.mobileforms.core.servlets;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import java.io.*;
import java.nio.charset.StandardCharsets;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/generateInteractivePDF"})
public class GeneratePDFFromMobileFormData extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final transient Logger logger = LoggerFactory.getLogger(this.getClass());
    @Reference
    GeneratePDFFromMobileForm generatePDFFromMobileForm;

    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) throws IOException {
        String dataXml = request.getParameter("formData");
        logger.debug("The data is "+dataXml);
        InputStream inputStream = new ByteArrayInputStream(dataXml.getBytes(StandardCharsets.UTF_8));
        Document submittedXml = new Document(inputStream);
       Document interactivePDF =  generatePDFFromMobileForm.generateInteractivePDF(submittedXml,request.getParameter("xdpPath"));
        interactivePDF.copyToFile(new File("interactive.pdf"));
        try {
            InputStream fileInputStream = interactivePDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
            response.setContentLength(fileInputStream.available());
            ServletOutputStream responseOutputStream = response.getOutputStream();

            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }

            responseOutputStream.flush();
            responseOutputStream.close();
        }
        catch(Exception e)
        {
            logger.debug(e.getMessage());
        }


    }
}
```

### Interactieve PDF renderen

De volgende code maakt gebruik van [&#x200B; de Dienst API van Forms &#x200B;](https://helpx.adobe.com/nl/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) interactieve PDF met de gegevens van de mobiele vorm terug te geven.

```java
package com.aemforms.mobileforms.core.documentservices.impl;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.AcrobatVersion;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.adobe.fd.forms.api.PDFFormRenderOptions;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(service = GeneratePDFFromMobileForm.class, immediate = true)
public class GeneratePDFFromMobileFormImpl implements GeneratePDFFromMobileForm {
    @Reference
    FormsService formsService;
    private   transient Logger log = LoggerFactory.getLogger(this.getClass());

    @Override
    public Document generateInteractivePDF(Document xmlData, String xdpPath)
    {
        String uri = "crx:///content/dam/formsanddocuments";
        String xdpName = xdpPath.substring(31, xdpPath.lastIndexOf("/jcr:content"));
        log.debug("####In mobile form to interactive pdf####   " + xdpName);
        PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
        renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
        renderOptions.setContentRoot(uri);
        Document interactivePDF = null;
        try
        {
            interactivePDF = this.formsService.renderPDFForm(xdpName, xmlData, renderOptions);
        }
        catch (FormsServiceException e)
        {
            log.error(e.getMessage());
        }

        return interactivePDF;

    }
}
```

## Volgende stappen

[Formulierverzending verwerken](./handle-form-submission.md)
