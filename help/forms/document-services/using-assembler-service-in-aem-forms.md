---
title: Assembler Service gebruiken in AEM Forms
description: Het gebruiken van de Dienst van de Assembler in AEM Forms om veelvoudige pdf- dossiers samen te stellen
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
duration: 121
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Assembler Service gebruiken in AEM Forms{#using-assembler-service-in-aem-forms}

In dit artikel vindt u de elementen waarmee u kunt aantonen dat u meerdere PDF-bestanden in de browser kunt slepen en neerzetten en het geassembleerde PDF-bestand op uw bestandssysteem kunt opslaan. Hier volgt de code voor de servlet die de PDF-bestanden samenstelt die zijn geüpload met de browser.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

Om deze functie te laten werken op uw AEM

* Download de [Meerdere bestanden samenstellen.zip](assets/assemble-multiple-files.zip) naar uw lokale systeem.
* Upload en installeer het pakket met de [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)
* Downloaden[Pakket voor aangepaste documentservices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Downloaden [Ontwikkelen met de Bundel van de Gebruiker van de Dienst](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* De bundels implementeren en starten met de [felix-webconsole](http://localhost:4502/system/console/bundles)
* Wijs uw browser aan [PDF&#39;s samenstellen.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* Enkele bestanden met PDF-bestanden slepen en neerzetten

>[!NOTE]
>
>Zorg ervoor dat de AEM Forms-installatie is voltooid. Alle bundels moeten actief zijn.
>
>Zorg ervoor u hebt toegevoegd - de bibliotheken van RSA en BouncyCastle van de Vraag zoals vermeld in dit [AEM Forms installeren](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**Voorschriften voor deze demo**
>
> * De code verwerkt geen op XFA gebaseerde PDF-documenten
>
> * Alleen PDF-bestanden slepen en neerzetten
>
>
