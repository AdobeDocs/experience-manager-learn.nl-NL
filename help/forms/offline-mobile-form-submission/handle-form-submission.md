---
title: Trigger AEM workflow voor het verzenden van HTML5-formulieren - PDF verwerken
description: De verzending van het HTML5/PDF-formulier afhandelen
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
level: Experienced
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# Formulierverzending verwerken

In dit deel maken we een eenvoudige servlet die op AEM Publish wordt uitgevoerd voor het verwerken van het PDF-formulier of het verzenden van het HTML5-formulier. Deze servlet, doet een verzoek van de POST van HTTP aan servlet die in een AEM auteursinstantie loopt verantwoordelijk voor het opslaan van de voorgelegde gegevens als a `nt:file` knoop in AEM bewaarplaats van de Auteur.

Hier volgt de code van de servlet die de PDF/HTML5-formulierverzending afhandelt. In deze servlet maken wij een POST vraag aan servlet op **wordt opgezet/bin/startworkflow** in een AEM instantie van de Auteur die. Deze server slaat de formuliergegevens op in de opslagplaats van de AEM auteur.


## AEM Publish servlet

De volgende code handelt de PDF/HTML5 formulierverzending af. Deze code wordt uitgevoerd op de publicatie-instantie.

```java
package com.aemforms.mobileforms.core.servlets;
import com.aemforms.mobileforms.core.configuration.service.AemServerCredentialsConfigurationService;
import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.ServletInputStream;
import javax.servlet.ServletOutputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.PrintWriter;
import java.io.Serializable;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.Base64;
import java.util.List;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/handleformsubmission"})
public class HandleFormSubmission extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final transient Logger logger = LoggerFactory.getLogger(this.getClass());
    @Reference
    AemServerCredentialsConfigurationService aemServerCredentialsConfigurationService;



    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        logger.debug("In do POST of bin/handleformsubmission");
        ByteArrayOutputStream result = new ByteArrayOutputStream();
        try {
            ServletInputStream is = request.getInputStream();
            byte[] buffer = new byte[1024];
            int length;
            while ((length = is.read(buffer)) != -1) {
                result.write(buffer, 0, length);
            }
            logger.debug(result.toString(StandardCharsets.UTF_8.name()));
        } catch (IOException e1) {
            logger.error("An error occurred", e1);
        }
        String postURL = aemServerCredentialsConfigurationService.getWorkflowServer();
        logger.debug("The url to invoke workflow is  "+postURL);
        HttpPost postReq = new HttpPost(postURL);
        // This is the base64 encoding of the admin credentials. This call should be made over HTTPS in production scenarios to avoid leaking credentials.
        String userName = aemServerCredentialsConfigurationService.getUserName();
        String password = aemServerCredentialsConfigurationService.getPassword();
        String credential = userName+":"+password;
        String encodedString = Base64.getEncoder().encodeToString(credential.getBytes());
        postReq.addHeader("Authorization", "Basic "+encodedString);
        System.out.println("The encoded string is "+"Basic "+encodedString);

        CloseableHttpClient httpClient = HttpClients.createDefault();
        List<NameValuePair> urlParameters = new ArrayList<NameValuePair>();

        logger.debug("added url parameters");
        try {
            urlParameters.add(new BasicNameValuePair("xmlData", result.toString(StandardCharsets.UTF_8.name())));
            postReq.setEntity(new UrlEncodedFormEntity(urlParameters));
            HttpResponse httpResponse = httpClient.execute(postReq);
            logger.debug("Sent request to author instance");
            String startWorkflowResponse = EntityUtils.toString(httpResponse.getEntity());
            response.setContentType("text/plain");
            PrintWriter out = response.getWriter();
            out.write(startWorkflowResponse);

        } catch (IOException e) {
            logger.error("An error occurred", e);
        }


    }
}
```

## Volgende stappen

[Verzonden gegevens opslaan op instantie van auteur](./author-servlet.md)