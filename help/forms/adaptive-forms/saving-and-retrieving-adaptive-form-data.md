---
title: Aangepaste formuliergegevens opslaan en ophalen
description: Aangepaste formuliergegevens opslaan en ophalen uit de database. Hierdoor kunnen invullers het formulier opslaan en het formulier op een latere datum invullen.
feature: Adaptive Forms
topic: Development
role: Developer
type: Tutorial
version: 6.4,6.5
last-substantial-update: 2019-06-09T00:00:00Z
duration: 794
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---


# Aangepaste formuliergegevens opslaan en ophalen

In dit artikel worden de stappen beschreven die nodig zijn voor het opslaan en ophalen van adaptieve formuliergegevens uit de database. MySQL-database is gebruikt om de Adaptief-formuliergegevens op te slaan. Op hoog niveau zijn de volgende stappen om het gebruiksgeval te bereiken:

* [Gegevensbron configureren](#Configure-Data-Source)
* [Servlet maken om gegevens naar de database te schrijven](#create-servlet)
* [OSGI-service maken om opgeslagen gegevens op te halen](#create-osgi-service)
* [Clientbibliotheek maken](#create-client-library)
* [Aangepaste formuliersjabloon en paginacomponent maken](#form-template-and-page-component)
* [Capability Demonstratie](#capability-demo)
* [Distribueren op uw server](#deploy-on-your-server)

## Gegevensbron configureren {#Configure-Data-Source}

Apache Sling Connection Pooled DataSource is geconfigureerd om te verwijzen naar de database die wordt gebruikt om de Adaptive Form-gegevens op te slaan. Het volgende schermafbeelding toont de configuratie voor mijn instantie. U kunt de volgende eigenschappen kopiëren en plakken

* Naam gegevensbron:informatieStutorial - dit is de naam die in mijn code wordt gebruikt.

* JDBC-stuurprogramma, klasse:com.mysql.jdbc.Driver

* URL JDBC-verbinding:jdbc:mysql://localhost:3306/aemformstutorial

![connectionpool](assets/storingdata.PNG)

### Servlet maken {#create-servlet}

Hier volgt de code van de servlet die Adaptieve formuliergegevens in de database invoegt/bijwerkt. De Apache Sling Connection Pooled DataSource wordt geconfigureerd met de AEM ConfigMgr en er wordt in regel 26 naar verwezen. De rest van de code is vrij eenvoudig. De code voegt een nieuwe rij in de database in of werkt een bestaande rij bij. De opgeslagen Adaptieve gegevens van de Vorm worden geassocieerd met een GUID. Dezelfde GUID wordt vervolgens gebruikt om de formuliergegevens bij te werken.

```java
package com.techmarketing.core.servlets;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;
import javax.servlet.Servlet;
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
 
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/storeafdata"})
public class StoreDataInDB extends SlingAllMethodsServlet {
     private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
        private static final long serialVersionUID = 1L;
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
    public String updateData(String afdata,String guid)
    {
         String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
         Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(updateTableSQL);
                pstmt.setString(1,afdata);
                pstmt.setString(2,guid);
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
            return guid;
     
         
    }
     public String insertData(String afdata) {
            log.debug("### Insert Data #### The json object I got to insert was " + afdata);
            String insertTableSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
            UUID uuid = UUID.randomUUID();
            String randomUUIDString = uuid.toString();
            log.debug("The query is " + insertTableSQL);
            Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(insertTableSQL);
                pstmt.setString(1,randomUUIDString);
                pstmt.setString(2,afdata);
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
            return randomUUIDString;
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
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
    {
        log.debug("Inside my save af data servlet");
        if(request.getParameter("operation").equalsIgnoreCase("update"))
        {
            log.debug("The operation is update");
            log.debug("The data I got was "+request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"),request.getParameter("guid"));
            log.debug("The guid I got was  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(request.getParameter("operation").equalsIgnoreCase("insert"))
        {
            log.debug("The data I got was +request.getParameter("formdata");
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

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

## OSGI-service maken voor het ophalen van gegevens {#create-osgi-service}

De volgende code is geschreven om de opgeslagen gegevens van het Adaptieve formulier op te halen. Een eenvoudige vraag wordt gebruikt om de Adaptieve gegevens van de Vorm te halen verbonden aan een bepaalde GUID. De opgehaalde gegevens worden vervolgens geretourneerd aan de aanroepende toepassing. Dezelfde gegevensbron gemaakt in de eerste stap waarnaar in deze code wordt verwezen.

```java
package com.techmarketing.core.impl;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
import javax.sql.DataSource;
 
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
import com.techmarketing.core.AemFormsAndDB;
 
 
@Component(service=AemFormsAndDB.class,immediate = true)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
 
    @Override
    public String getData(String guid) {
        System.out.println("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '"+guid+"'"+"";
            log.debug(" Got Result Set"+query);
            ResultSet rs = st.executeQuery(query);
            while(rs.next())
            {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
        return null;
    }
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.debug("not able to get connection ");
                e.printStackTrace();
            }
            return null;
        }
 
 
}
```

## Clientbibliotheek maken {#create-client-library}

AEM Clientbibliotheek beheert al uw JavaScript-code aan de clientzijde. Voor dit artikel heb ik een eenvoudige javascript gemaakt om de Adaptieve formuliergegevens op te halen met behulp van de bridge-API voor handleidingen. Nadat de Adaptief-formuliergegevens zijn opgehaald, wordt de POST naar het servlet aangeroepen om de adaptieve formuliergegevens in de database in te voegen of bij te werken. De functie getALLUrlParams retourneert de parameters in de URL. Dit wordt gebruikt wanneer u de gegevens wilt bijwerken. De rest van de functionaliteit wordt behandeld in de code verbonden aan de klikgebeurtenis van.savebutton klasse. Als de guid-parameter aanwezig is in de URL, moeten we de updatebewerking uitvoeren als dit geen invoegbewerking is.

```javascript
function getAllUrlParams(url) {
 
  // get query string from url (optional) or window
  var queryString = url ? url.split('?')[1] : window.location.search.slice(1);
 
  // we'll store the parameters here
  var obj = {};
 
  // if query string exists
  if (queryString) {
 
    // stuff after # is not part of query string, so get rid of it
    queryString = queryString.split('#')[0];
 
    // split our query string into its component parts
    var arr = queryString.split('&');
 
    for (var i = 0; i < arr.length; i++) {
      // separate the keys and the values
      var a = arr[i].split('=');
 
      // set parameter name and value (use 'true' if empty)
      var paramName = a[0];
      var paramValue = typeof (a[1]) === 'undefined' ? true : a[1];
 
      // (optional) keep case consistent
      paramName = paramName.toLowerCase();
      if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();
 
      // if the paramName ends with square brackets, e.g. colors[] or colors[2]
      if (paramName.match(/\[(\d+)?\]$/)) {
 
        // create key if it doesn't exist
        var key = paramName.replace(/\[(\d+)?\]/, '');
        if (!obj[key]) obj[key] = [];
 
        // if it's an indexed array e.g. colors[2]
        if (paramName.match(/\[\d+\]$/)) {
          // get the index value and add the entry at the appropriate position
          var index = /\[(\d+)\]/.exec(paramName)[1];
          obj[key][index] = paramValue;
        } else {
          // otherwise add the value to the end of the array
          obj[key].push(paramValue);
        }
      } else {
        // we're dealing with a string
        if (!obj[paramName]) {
          // if it doesn't exist, create property
          obj[paramName] = paramValue;
        } else if (obj[paramName] && typeof obj[paramName] === 'string'){
          // if property does exist and it's a string, convert it to an array
          obj[paramName] = [obj[paramName]];
          obj[paramName].push(paramValue);
        } else {
          // otherwise add the property
          obj[paramName].push(paramValue);
        }
      }
    }
  }
 
  return obj;
}
 
$(document).ready(function()
   {
        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
       linktext.visible = false;
       linktext1.visible = false;
        $(".savebutton").click(function(){
           var params = getAllUrlParams(window.location.href);
           console.log(getAllUrlParams(window.location.href));
            window.guideBridge.getDataXML({
                 success:function(guideResultObject) 
                 {
                     console.log("The data is "+guideResultObject.data);
                     let xhr = new XMLHttpRequest();
                      xhr.open('POST','/bin/storeafdata');
                     let formData = new FormData();
                     if(typeof(params.guid)!="undefined")
                     {
                         formData.append("operation","update");
                         formData.append("guid",params.guid);
 
                     }
                     if(typeof(params.guid)=="undefined")
                     {
                         formData.append("operation","insert");
 
 
                     }
 
 
                formData.append("formdata",guideResultObject.data);
                xhr.send(formData);
                     xhr.onload = function(e)
                {
                    console.log("The data is ready");
                    if (this.status == 200)
                        {
                            var jsonResponse = JSON.parse(this.response);
                            console.log(jsonResponse.guid);
                            var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                            var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                            linktext1.visible = true;
                            linktext.value = "http://localhost:4502/content/dam/formsanddocuments/saveformdata/jcr:content?wcmmode=disabled&guid="+jsonResponse.guid;
                            linktext.visible = true;
                            guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        }
 
                }
                  }
             });
 
 
       });
 
 
});
```

## Aangepaste formuliersjabloon en paginacomponent maken {#form-template-and-page-component}


>[!VIDEO](https://video.tv.adobe.com/v/27828?quality=12&learn=on)

### Bewijs van de bekwaamheid {#capability-demo}

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)

#### Distribueren op uw server {#deploy-on-your-server}

Voer de volgende stappen uit om deze mogelijkheid te testen op uw AEM Forms-exemplaar

* [Download en decomprimeer de DemoAssets.zip naar uw lokale systeem](assets/demoassets.zip)
* Implementeer en start de bundels techmarketingdemos.jar en mysqldriver.jar met behulp van de Felix-webconsole.
*** Importeer het bestand aemformstutorial.sql met MYSQL Workbench. Hierdoor worden het benodigde schema en de benodigde tabellen in uw database gemaakt
* Importeer StoreAndRetrieve.zip met AEM pakketbeheer. Dit pakket bevat de adaptieve formuliersjabloon, de client lib van de paginacomponent en een voorbeeldconfiguratie voor adaptieve formulieren en gegevensbronnen.
* Aanmelden bij configMgr. Zoek naar &quot;Apache Sling Connection Pooled DataSource. Open de gegevensbroningang verbonden aan een modelstudie en ga de gebruikersbenaming en het wachtwoord in specifiek voor uw gegevensbestandinstantie.
* Het adaptieve formulier openen
* Vul enkele details in en klik op de knop &quot;Opslaan en verdergaan&quot;
* U zou terug een URL met een GUID in het moeten krijgen.
* Kopieer de URL en plak deze in een nieuw browsertabblad
* Het adaptieve formulier moet worden gevuld met de gegevens uit de vorige stap**
