---
title: OSGi-service maken
description: OSGi-service maken om de formulieren op te slaan ter ondertekening
feature: Workflow
version: 6.4,6.5
thumbnail: 6886.jpg
jira: KT-6886
topic: Development
role: Developer
level: Experienced
exl-id: 49e7bd65-33fb-44d4-aaa2-50832dffffb0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# OSGi-service maken

De volgende code is geschreven om de formulieren op te slaan die moeten worden ondertekend. Elk formulier ter ondertekening is gekoppeld aan een unieke hulplijn en een klant-id. Zo kunnen één of meerdere vormen met zelfde klant identiteitskaart worden geassocieerd, maar zal een unieke GUID hebben die aan de vorm wordt toegewezen.

## Interface

Het volgende is de interfacedeclaratie die werd gebruikt

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## Gegevens invoegen

De tussenvoegselgegevensmethode neemt een rij in het gegevensbestand op dat door de gegevensbron wordt geïdentificeerd. Elke rij in het gegevensbestand beantwoordt aan een vorm en door een GUID en een klant identiteitskaart uniek geïdentificeerd. De formuliergegevens en de URL van het formulier worden ook in deze rij opgeslagen. In de statuskolom wordt aangegeven of het formulier is ingevuld en ondertekend. De waarde 0 geeft aan dat het formulier nog moet worden ondertekend.

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## Formuliergegevens ophalen

De volgende code wordt gebruikt om adaptieve vormgegevens te halen verbonden aan bepaalde GUID. De formuliergegevens worden vervolgens gebruikt om het aangepaste formulier vooraf in te vullen.

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## Handtekeningstatus bijwerken

Als de ondertekeningsceremonie met succes is voltooid, wordt een AEM workflow geactiveerd die aan het formulier is gekoppeld. De eerste stap in de workflow is een processtap waarmee de status in de database wordt bijgewerkt voor de rij die door de id en de id van de klant wordt geïdentificeerd. De waarde van het ondertekende element in de formuliergegevens wordt ook ingesteld op Y om aan te geven dat het formulier is ingevuld en ondertekend. Het adaptieve formulier wordt gevuld met deze gegevens en de waarde van het ondertekende gegevenselement in de XML-gegevens wordt gebruikt om het juiste bericht weer te geven. De code updateSignatureStatus wordt aangeroepen vanuit de stap voor het aangepaste proces.


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## Volgend formulier ophalen om te ondertekenen

De volgende code is gebruikt om het volgende formulier voor ondertekening voor een bepaalde customerID met de status 0 op te halen. Als de sql-query geen rijen retourneert, wordt de tekenreeks geretourneerd **&quot;AllDone&quot;** Dit geeft aan dat er geen formulieren meer zijn voor ondertekening voor de opgegeven id van de klant.

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## Assets

De OSGi-bundel met bovengenoemde diensten kan [hier gedownload](assets/sign-multiple-forms.jar)

## Volgende stappen

[Een hoofdwerkstroom maken voor het verwerken van de eerste formulierverzending](./create-main-workflow.md)
