---
title: AEM Forms met JSON-schema en -gegevens[Deel2]
description: Zelfstudie met meerdere onderdelen om u door de stappen te laten lopen die nodig zijn voor het maken van een adaptief formulier met JSON-schema en het opvragen van de verzonden gegevens.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
duration: 110
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Verzendgegevens opslaan in database


>[!NOTE]
>
>Het wordt aanbevolen MySQL 8 te gebruiken als uw database omdat deze ondersteuning biedt voor JSON-gegevenstype. U moet ook het juiste stuurprogramma voor MySQL DB installeren. Ik heb het stuurprogramma gebruikt dat beschikbaar is op deze locatie https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12

Om de voorgelegde gegevens in gegevensbestand op te slaan, zullen wij een servlet schrijven om de gebonden gegevens en de vormnaam en opslag te halen. De volledige code voor het verwerken van het verzenden van het formulier en het opslaan van de afBoundData in de database vindt u hieronder.

We hebben een aangepaste verzending gemaakt voor de verzending van het formulier. In deze aangepaste submit&#39;s post.POST.jsp sturen we het verzoek door naar onze servlet.

Lees dit voor meer informatie over Aangepast verzenden [artikel](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmission&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![connectionpool](assets/connectionpooled.gif)

Volg de volgende stappen om dit systeem te laten werken

* [Het ZIP-bestand downloaden en uitpakken](assets/aemformswithjson.zip)
* Maak een adaptiefFormulier met JSON-schema. U kunt het JSON-schema gebruiken dat is opgegeven als onderdeel van deze artikelelementen. Zorg ervoor dat de handeling voor het verzenden van het formulier op de juiste wijze is geconfigureerd. Verzendactie moet worden geconfigureerd voor de &quot;CustomSubmitHelpx&quot;.
* Creeer een schema in uw instantie MySQL door het schema.sql- dossier in te voeren gebruikend het hulpmiddel MySQL Workbench. Het bestand schema.sql wordt ook aan u geleverd als onderdeel van deze zelfstudie-elementen.
* Apache Sling Connection Pooled DataSource configureren vanuit de Felix-webconsole
* Zorg ervoor u uw gegevensbronnaam &quot;aemformswithjson&quot;noemt. Dit is de naam die wordt gebruikt door de bundel van steekproef OSGi die aan u wordt verstrekt
* Zie de bovenstaande afbeelding voor eigenschappen. Dit veronderstelt u MySQL als uw Gegevensbestand gaat gebruiken.
* Implementeer de OSGi-bundel(s) die als onderdeel van dit artikel worden geleverd.
* Bekijk een voorbeeld van het formulier en verzend het.
* De JSON-gegevens worden opgeslagen in de database die is gemaakt toen u het bestand &quot;schema.sql&quot; importeerde.
