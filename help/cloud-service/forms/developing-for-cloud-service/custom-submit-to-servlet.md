---
title: Aangepaste Verzendactie-handler maken
description: Aangepast formulier verzenden naar een aangepaste verzendhandler
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
duration: 52
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# servlet maken om de verzonden gegevens te verwerken

Start je aem-banking project in IntelliJ.
Creeer een eenvoudige servlet om de voorgelegde gegevens aan het logboekdossier uit te voeren.Zorg ervoor de code in het kernproject zoals aangetoond in het het schermschot hieronder is
![&#x200B; creeer-servlet &#x200B;](assets/create-servlet.png)

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

## Aangepaste verzendhandler maken

Creeer uw douane voorlegt actie in de `apps/bankingapplication` omslag de zelfde manier u in de [&#x200B; vroegere versies van AEM Forms &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=nl-NL) creeerde. In het kader van deze zelfstudie maak ik een map met de naam SubmitToAEMServlet onder het knooppunt `apps/bankingapplication` in de CRX-opslagplaats.

De volgende code in post.POST.jsp door:sturen eenvoudig het verzoek aan servlet op /bin/formstutorial. Dit is dezelfde servlet die in de vorige stap is gemaakt

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

Klik in uw AEM-project in IntelliJ met de rechtermuisknop op de map `apps/bankingapplication` en selecteer Nieuw | Verpak en typ in SubmitToAEMServlet na apps.bankingapplication in het nieuwe de pakketdialoogvakje. Klik met de rechtermuisknop op het knooppunt VerzendenToAEMServlet en selecteer Opnieuw | Opdracht ophalen om het AEM-project te synchroniseren met de AEM-serveropslagplaats.


## Adaptief formulier configureren

U kunt om het even welke AanpassingsVorm nu vormen om aan deze douane voor te leggen voorlegt manager genoemd **voorlegt aan AEM Servlet**

## Volgende stappen

[Serlet registreren met middeltype](./registering-servlet-using-resourcetype.md)
