---
title: AEM-workflow activeren bij het verzenden van HTML5-formulieren - Formulier verwerken
description: Leer hoe u de AEM-workflow kunt activeren wanneer het HTML5-formulier wordt verzonden en de verzonden gegevens in de gegevensopslagruimte kunt opslaan.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
badgeVersions: label="AEM Forms 6.5" before-title="false"
level: Experienced
jira: kt-16215
exl-id: e0bde892-1da0-4b72-a408-ad7b84086939
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 0%

---

# Verzonden gegevens opslaan

De volgende stap bestaat uit het opslaan van de verzonden gegevens in de gegevensopslagruimte van de AEM Auteur. De server die op `/bin/startworkflow` is gekoppeld, slaat de verzonden gegevens op.
Een AEM-workflowstartprogramma is geconfigureerd om elke keer dat een nieuwe resource van het type `nt:file` wordt gemaakt onder het knooppunt &lt;node_to_store_submitted_data> te activeren. Met deze workflow worden niet-interactieve of statische PDF gemaakt door de verzonden gegevens samen te voegen met de xdp-sjabloon. Het gegenereerde PDF-bestand wordt vervolgens ter controle en goedkeuring aan een gebruiker toegewezen.

Als u de verzonden gegevens wilt opslaan onder het knooppunt &lt;node_to_store_submitted_data>, maken we gebruik van de `GetResolver` OSGi-service, waarmee we de verzonden gegevens kunnen opslaan met de `fd-service` -systeemgebruiker die beschikbaar is in elke AEM Forms-installatie.

De knoop waaronder het voorgelegde gegeven wordt opgeslagen kan worden gevormd gebruikend ConfigMgr zoals die in [ wordt verklaard stelt steekproefactiva ](./deploy-assets.md) op

```java
package com.aemforms.mobileforms.core.servlets;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.UUID;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import com.aemforms.mobileforms.core.configuration.service.AemServerCredentialsConfigurationService;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import org.apache.sling.api.servlets.SlingAllMethodsServlet;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/startworkflow"})
public class StartWorkflow extends SlingAllMethodsServlet {

    private static Logger logger = LoggerFactory.getLogger(StartWorkflow.class);

    @Reference
    private GetResolver getResolver;
    
    @Reference
    private AemServerCredentialsConfigurationService aemServerCredentialsConfigurationService;
    
    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
    {
            String xmlData = null;
            logger.debug("in start workflow");
            response.setContentType("text/html;charset=UTF-8");
            if (request.getParameter("xmlData") != null)
                {
                    logger.debug("The form was submitted from Acrobat/Reader");
                    xmlData = request.getParameter("xmlData");
                    logger.debug("In start workflow" + xmlData);
                }
                logger.debug("Trying to get resource  "+aemServerCredentialsConfigurationService.getFolderPath());
                Resource r = getResolver.getFormsServiceResolver().getResource(aemServerCredentialsConfigurationService.getFolderPath());
                String responseMessage =null;
                if(r!= null)
                {

                    Session session = r.getResourceResolver().adaptTo(Session.class);
                    logger.debug("Got reosurce pdfsubmissions" + r.getPath());
                    UUID uidName = UUID.randomUUID();
                    Node xmlDataFilesNode = r.adaptTo(Node.class);
                    InputStream is = new ByteArrayInputStream(xmlData.getBytes());
                    Binary binary;
                    try {
                            Node xmlFileNode = xmlDataFilesNode.addNode(uidName.toString(), "nt:file");
                            logger.debug("Added nt file node");
                            Node jcrContent = xmlFileNode.addNode("jcr:content", "nt:resource");
                            logger.debug("Added jcr content");
                            binary = session.getValueFactory().createBinary(is);
                            jcrContent.setProperty("jcr:data", binary);
                            session.save();
                        } catch (RepositoryException e)
                        {
                            throw new RuntimeException(e);
                        }
                    responseMessage = "Your form was successfully submitted";
                }else
                {
                    logger.debug("The resource IS NULL");
                    responseMessage = "Error is processing your submission!!! Please contact the administrator";
                }

        try {
            response.setContentType("text/plain");
            response.getWriter().write(responseMessage);

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

}
```

## Volgende stappen

[Workflow starten en workflow](./review-workflow.md)
