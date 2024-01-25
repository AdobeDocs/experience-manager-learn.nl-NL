---
title: DAM-mapitems toevoegen aan keuzeselectiegroep
description: Items dynamisch toevoegen aan keuzerondjesgroepcomponent
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 29f56d13-c2e2-4bc2-bfdc-664c848dd851
duration: 100
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Items dynamisch toevoegen aan keuzeselectiegroep

In AEM Forms 6.5 is de mogelijkheid geïntroduceerd om items dynamisch toe te voegen aan een adaptieve Forms-keuzerondjesgroepcomponent, zoals CheckBox, Radio Button en Afbeeldingslijst. In dit artikel bekijken we of een keuzegroep kan worden gevuld met de DAM-mapinhoud. In het schermafbeelding bevinden de drie bestanden zich in de map Nieuwsbrief. Telkens wanneer een nieuwe nieuwsbrief aan de map wordt toegevoegd, wordt de groepscomponent Keuze bijgewerkt en wordt de inhoud ervan automatisch weergegeven. De gebruiker kan een of meer nieuwsbrieven selecteren om te downloaden.

![Regeleditor](assets/newsletters-download.png)

## servlet maken om de inhoud van de DAM-map te retourneren

De volgende code is geschreven om de inhoud van de DAM-map in JSON-indeling te retourneren.

```java
package com.newsletters.core.servlets;
import static com.day.cq.commons.jcr.JcrConstants.JCR_CONTENT;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.Gson;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=get",
  "sling.servlet.paths=/bin/listfoldercontents"
})
public class ListFolderContent extends SlingSafeMethodsServlet {
  private static final long serialVersionUID = 1 L;
  private static final Logger log = LoggerFactory.getLogger(ListFolderContent.class);
  protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    Resource resource = request.getResourceResolver().getResource(request.getParameter("damFolder"));
    List < JsonObject > results = new ArrayList < > ();
    resource.getChildren().forEach(child -> {
      if (!JCR_CONTENT.equals(child.getName())) {
        JsonObject asset = new JsonObject();
        log.debug("##The child name is " + child.getName());
        asset.addProperty("assetname", child.getName());
        asset.addProperty("assetpath", child.getPath());
        results.add(asset);

      }
    });
    PrintWriter out = null;
    try {
      out = response.getWriter();
    } catch (IOException e) {

      log.debug(e.getMessage());
    }
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    Gson gson = new Gson();
    out.print(gson.toJson(results));
    out.flush();
  }

}
```

## Clientbibliotheek maken met JavaScript-functie

De servlet wordt aangeroepen vanuit een JavaScript-functie. De functie retourneert een matrixobject dat wordt gebruikt om de keuzegroep te vullen

```javascript
/**
 * Populate drop down/choice group  with assets from specified folder
 * @return {string[]} 
 */
function getDAMFolderAssets(damFolder) {
   // strUrl is whatever URL you need to call
   var strUrl = '/bin/listfoldercontents?damFolder=' + damFolder;
   var documents = [];
   $.ajax({
      url: strUrl,
      success: function(jsonData) {
         for (i = 0; i < jsonData.length; i++) {
            documents.push(jsonData[i].assetpath + "=" + jsonData[i].assetname);
         }
      },
      async: false
   });
   return documents;
}
```

## Adaptief formulier maken

Een adaptief formulier maken en het formulier koppelen aan de clientbibliotheek **listfoldermiddelen**. Voeg een component CheckBox toe aan het formulier. Gebruik de regelredacteur om de opties van checkbox zoals aangetoond in scherm-schot te bevolken
![instellen, opties](assets/set-options-newsletter.png)

Er wordt een JavaScript-functie aangeroepen **getDAMFolderAssets** en geeft u het pad van de elementen van de DAM-map door naar de lijst in het formulier.

## Volgende stappen

[Geselecteerde elementen samenstellen](./assemble-selected-newsletters.md)
