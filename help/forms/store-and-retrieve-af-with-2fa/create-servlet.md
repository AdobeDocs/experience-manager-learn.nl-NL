---
title: servlet maken
description: Maak servlet voor de verwerking van de POST-aanvragen om de formuliergegevens op te slaan
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6539
thumbnail: 6539.pg
topic: Development
role: Developer
level: Experienced
exl-id: a24ea445-3997-4324-99c4-926b17c8d2ac
duration: 70
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 0%

---

# servlet maken

De volgende stap is een servlet tot stand te brengen die de aangewezen methodes van onze douaneOSGi dienst roept. De servlet heeft toegang tot de adaptieve formuliergegevens, bestandsbijlagen. De servlet retourneert een unieke toepassings-id die kan worden gebruikt om het gedeeltelijk ingevulde adaptieve formulier op te halen.

Deze servlet wordt aangeroepen wanneer de gebruiker op de knop Opslaan en afsluiten klikt op het adaptieve formulier

```java
package saveandresume.core.servlets;

import java.io.PrintWriter;

import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.saveAndResume.core.SaveAndFetchDataFromDB;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/storeafdatawithattachments"
})
public class StoreDataInDBWithAttachmentsInfo extends SlingAllMethodsServlet {
  private Logger log = LoggerFactory.getLogger(StoreDataInDBWithAttachmentsInfo.class);
  private static final long serialVersionUID = 1 L;
  @Reference
  SaveAndFetchDataFromDB saveAndFetchFromDB;

  public void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    final String afData = request.getParameter("data");
    final String tel = request.getParameter("mobileNumber");
    log.debug("$$$The telephone number is  " + tel);
    log.debug("The request parameter data is  " + afData);
    try {
      JsonObject fileMap = JsonParser.parseString(request.getParameter("fileMap")).getAsJsonObject();
      log.debug("The file map is: " + fileMap.toString());
      String newFileMap = saveAndFetchFromDB.storeAFAttachments(fileMap, request);
      String application_id = saveAndFetchFromDB.storeFormData(afData, newFileMap, tel);
      log.debug("The application id:  " + application_id);
      JsonObject jsonObject = new JsonObject();
      jsonObject.addProperty("applicationID", application_id);
      response.setContentType("application/json");
      response.setHeader("Cache-Control", "nocache");
      response.setCharacterEncoding("utf-8");
      PrintWriter out = null;
      out = response.getWriter();
      out.println(jsonObject.toString());
    } catch (Exception ex) {
      log.error(ex.getMessage());
    }
  }

}
```

## Volgende stappen

[Formulier weergeven met opgeslagen formuliergegevens](./retrieve-saved-form.md)
