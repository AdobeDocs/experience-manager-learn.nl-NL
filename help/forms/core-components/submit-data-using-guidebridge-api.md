---
title: GuideBridge-API gebruiken voor toegang tot formuliergegevens
description: Toegang tot formuliergegevens en bijlagen met de GuideBridge-API voor een basiscomponentgebaseerd adaptief formulier.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-15286
last-substantial-update: 2024-04-05T00:00:00Z
source-git-commit: 73c15a195c438dd7a07142bba719c6f820bf298a
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---

# GuideBridge-API gebruiken om formuliergegevens te POSTEN

Met &quot;Opslaan en hervatten&quot; voor een formulier kunnen gebruikers de invulling van het formulier opslaan en het later hervatten.
De volgende stappen zijn uitgevoerd voor het gebruik van de Geolocation API in Adaptive Forms. Om dit te verwezenlijken gebruiken wij het geval, moeten wij tot de vormgegevens toegang hebben en verzenden gebruikend GuideBridge API aan het REST eindpunt voor opslag en herwinning.

De formuliergegevens worden opgeslagen in de gebeurtenis click van een knop met de regeleditor
![regelaar](assets/rule-editor.png)

De volgende JavaScript-functie is geschreven om de gegevens naar het opgegeven eindpunt te verzenden

```javascript
/**
* Submits data and attachments 
* @name submitFormDataAndAttachments Submit form data and attachments to REST end point
* @param {string} endpoint in Stringformat
* @return {string} 
 */

function submitFormDataAndAttachments(endpoint) {

    guideBridge.getFormDataObject({
        success: function(resultObj) {
            afFormData = resultObj.data.data;
            var formData = new FormData();
            formData.append("dataXml", afFormData);
            for (i = 0; i < resultObj.data.attachments.length; i++) {
                var attachment = resultObj.data.attachments[i];
                console.log(attachment.name);
                formData.append(attachment.name, attachment.data);
            }
            var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && this.status == 200) {
                    console.log("successfully saved");
                    var fld = guideBridge.resolveNode("$form.confirmation");
                    return " Form data was saved successfully";

                }
            };
            xhttp.open('POST', endpoint)
            xhttp.send(formData);
        }
    });

}
```



## Code aan serverzijde

De volgende Java-code aan de serverzijde is geschreven om de formuliergegevens af te handelen. Hier volgt de Java-servlet die wordt uitgevoerd in AEM die wordt aangeroepen via de XHR-aanroep in het JavaScript hierboven.

```java
package com.azuredemo.core.servlets;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import javax.servlet.Servlet;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.Serializable;
import java.util.List;
@Component(
   service = {
      Servlet.class
   }
)
@SlingServletResourceTypes(
   resourceTypes = "azure/handleFormSubmission",
   methods = "POST",
   extensions = "json"
)
public class StoreFormSubmission extends SlingAllMethodsServlet implements Serializable {
   private static final long serialVersionUID = 1 L;
   private final transient Logger log = LoggerFactory.getLogger(this.getClass());
   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      List < RequestParameter > listOfRequestParameters = request.getRequestParameterList();
      log.debug("The size of list is " + listOfRequestParameters.size());
      for (int i = 0; i < listOfRequestParameters.size(); i++) {
         RequestParameter requestParameter = listOfRequestParameters.get(i);
         log.debug("is this request parameter a form field?" + requestParameter.isFormField());
         if (!requestParameter.isFormField()) {
            Document attachmentDOC = new Document(requestParameter.getInputStream());
            attachmentDOC.copyToFile(new File(requestParameter.getName()));
         } else {
            log.debug("Not a form field " + requestParameter.getName());
            log.debug(requestParameter.getString());
         }
      }
      response.setStatus(HttpServletResponse.SC_OK);
   }
}
```
