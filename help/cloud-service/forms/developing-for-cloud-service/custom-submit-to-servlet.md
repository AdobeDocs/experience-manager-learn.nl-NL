---
title: Aangepaste Verzendactie-handler maken
description: Aangepast formulier verzenden naar een aangepaste verzendhandler
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8852
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# servlet maken om de verzonden gegevens te verwerken

Start je aem-banking project in IntelliJ.
Creeer een eenvoudige servlet om de voorgelegde gegevens aan het logboekdossier uit te voeren.Zorg ervoor de code in het kernproject zoals aangetoond in het het schermschot hieronder is
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## Aangepaste verzending maken

Aangepaste verzendgegevens maken in de map app/bankingapplication op dezelfde manier als in het dialoogvenster [eerdere versies van AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
De volgende code in post.POST.jsp door:sturen eenvoudig het verzoek aan servlet op /bin/formstutorial. Dit is dezelfde servlet die in de vorige stap is gemaakt

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## Adaptief formulier configureren

U kunt nu uw Adaptief formulier configureren voor verzending naar deze aangepaste verzendhandler met de naam **Verzenden naar AEM servlet**



