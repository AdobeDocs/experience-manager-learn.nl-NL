---
title: PDF versleutelen met een wachtwoord voor machtigingen
description: Gebruik DocAssuranceService om een PDF te coderen
feature: Document Services
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-15849
last-substantial-update: 2024-07-19T00:00:00Z
exl-id: 5df8581c-a44c-449c-bf3b-8cdf57635c4d
source-git-commit: b823f9e294c42ba258049a942816f9a154a6e1a6
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# PDF versleutelen met een wachtwoord voor machtigingen

Voor het kopiëren, bewerken of afdrukken van een PDF-document is een wachtwoord voor machtigingen vereist, ook wel bekend als een eigenaar- of hoofdwachtwoord. Leer om [ DocAssuranceService ](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/docassurance/client/api/DocAssuranceService.html) API te gebruiken om een toestemmingswachtwoord op PDF programmatically toe te passen

Met de volgende JSP-code wordt een PDF gecodeerd met een wachtwoord voor machtigingen:

```java
<%--
     Encrypt PDF with permissions password
--%>
    <%@include file="/libs/foundation/global.jsp"%>
<%@ page import="com.adobe.fd.docassurance.client.api.EncryptionOptions,java.util.*,java.io.*,com.adobe.fd.encryption.client.*" %>
    <%@page session="false" %>
<%
    String filePath = request.getParameter("saveLocation");
    InputStream pdfIS = null;
    com.adobe.aemfd.docmanager.Document generatedDocument = null;
    // get the pdf file
    javax.servlet.http.Part pdfPart = request.getPart("pdfFile");
    pdfIS = pdfPart.getInputStream();
    com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);


// encrypt the document with permssions password. You can only print this document
    PasswordEncryptionOptionSpec poSpec = new PasswordEncryptionOptionSpec();    
    poSpec.setCompatability(PasswordEncryptionCompatability.ACRO_X);
    poSpec.setEncryptOption(PasswordEncryptionOption.ALL);
    List<PasswordEncryptionPermission> permissionList = new ArrayList<PasswordEncryptionPermission>();
    permissionList.add(PasswordEncryptionPermission.PASSWORD_PRINT_LOW);
    //hardcoding passwords into code is for demonstration purposes only.In real life scenarios the password is sourced from a secure location
    poSpec.setPermissionPassword("adobe");
    poSpec.setPermissionsRequested(permissionList);
    EncryptionOptions encryptionOptions = EncryptionOptions.getInstance();
    encryptionOptions.setEncryptionType(com.adobe.fd.docassurance.client.api.DocAssuranceServiceOperationTypes.ENCRYPT_WITH_PASSWORD);
    encryptionOptions.setPasswordEncryptionOptionSpec(poSpec);
    com.adobe.fd.docassurance.client.api.DocAssuranceService docAssuranceService = sling.getService(com.adobe.fd.docassurance.client.api.DocAssuranceService.class);
    com.adobe.aemfd.docmanager.Document securedDocument = docAssuranceService.secureDocument(pdfDocument,encryptionOptions,null,null,null);
    securedDocument.copyToFile(new java.io.File(filePath));
    out.println("Document encrypted and saved to " +filePath);
%>
```


**om het steekproefpakket op uw systeem** te testen

[Download en installeer het pakket met AEM pakketbeheer](assets/encryptpdf.zip)

**nadat u het pakket installeert, voeg volgende URLs toe de Adobe van de OSGi van de configuratielijst van gewenste personen van de Filter van Granite CSRF:**

1. [ Login aan configMgr ](http://localhost:4502/system/console/configMgr)
1. Zoeken naar graniet-CSRF-filter voor Adobe
1. Het volgende pad toevoegen aan de uitgesloten secties en opslaan
1. /content/AemFormsSamples/encrypt

## Het monster testen

U kunt de voorbeeldcode op verschillende manieren testen. De snelste en eenvoudigste manier is om Postman-app te gebruiken. Postman staat u toe om POST verzoeken aan uw server te maken.Het volgende het schermschot toont u de verzoekparameters nodig voor het postverzoek te werken. Zorg dat u het juiste machtigingstype opgeeft voordat u de aanvraag verzendt.

![ encrypt-pdf-postman ](assets/encrypt-pdf-postman.png)
