---
title: Ontwikkelen met Output and Forms Services in AEM Forms
seo-title: Ontwikkelen met Output and Forms Services in AEM Forms
description: Uitvoer- en Forms Service-API in AEM Forms gebruiken
seo-description: Uitvoer- en Forms Service-API in AEM Forms gebruiken
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---


# Ontwikkelen met Output en Forms Services in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Uitvoer- en Forms Service-API in AEM Forms gebruiken

In dit artikel zullen we het volgende bekijken:

* Uitvoerservice - Deze service wordt doorgaans gebruikt om XML-gegevens samen te voegen met de Xdp-sjabloon of pdf om samengevoegde pdf te genereren
* FormsService - Dit is een zeer veelzijdige service waarmee u gegevens uit en naar PDF-bestanden kunt exporteren/importeren

De officiële javadoc voor de AEM Forms API wordt [hier](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html) vermeld

Het volgende codefragment exporteert gegevens uit een PDF-bestand

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

Regel 1 haalt pdfile uit het verzoek uit

Line2 haalt saveLocation uit het verzoek uit

Regel 5 krijgt greep van FormsService

Regel 6 exporteert de xmlGegevens uit het PDF-bestand

**De monsterverpakking op uw systeem testen**

[Download en installeer het pakket met AEM pakketbeheer](assets/outputandformsservice.zip)




**Nadat u het pakket installeert, zult u volgende URLs in de Filter van CSRF van de Adobe moeten lijsten van gewenste personen Granite.**

1. Volg de onderstaande stappen om de hierboven vermelde paden te lijsten van gewenste personen.
1. [Aanmelden bij configMgr](http://localhost:4502/system/console/configMgr)
1. Zoeken naar Adobe Granite CSRF-filter
1. Voeg de volgende drie paden toe aan de uitgesloten secties en sla deze op
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Zoeken naar het filter Verticale verwijzing
1. Schakel het selectievakje Lege toestaan in. (Deze instelling mag alleen voor testdoeleinden worden gebruikt)
U kunt de voorbeeldcode op verschillende manieren testen. De snelste en eenvoudigste oplossing is het gebruik van de Postman-app. Met Postman kunt u POSTEN aanvragen bij de server. Installeer de Postman-app op uw systeem.
Start de app en voer de volgende URL in om de API voor exportgegevens te testen

Controleer of u &quot;POST&quot; hebt geselecteerd in de vervolgkeuzelijst
http://localhost:4502/content/AemFormsSamples/exportdata.html
Zorg ervoor dat u &quot;Autorisatie&quot; opgeeft als &quot;Basic Auth&quot;. Geef de gebruikersnaam en het wachtwoord voor AEM server op
Navigeer naar het tabblad Body en geef de aanvraagparameters op, zoals in de onderstaande afbeelding wordt getoond
![export](assets/postexport.png)
Klik vervolgens op de knop Verzenden

De verpakking bevat 3 monsters. De volgende paragrafen verklaren wanneer om de outputdienst of de Dienst van Forms, de url van de dienst, inputparameters te gebruiken die elke dienst verwacht

**Gegevens samenvoegen en uitvoer afvlakken:**

* Met Uitvoerservice kunt u gegevens samenvoegen met xdp- of pdf-document om samengevoegde pdf te genereren
* **URL** POST: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parameters aanvragen -**

   * xdp_or_pdf_file : Het xdp- of pdf-bestand waarmee u gegevens wilt samenvoegen
   * xmlfile: Het XML-gegevensbestand dat wordt samengevoegd met xdp_or_pdf_file
   * saveLocation: De locatie waar het gerenderde document moet worden opgeslagen op uw bestandssysteem

**Gegevens importeren in PDF-bestand:**
* FormsService gebruiken om gegevens te importeren in PDF-bestand
* **POST-URL** : http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parameters aanvragen:**

   * pdffile : Het PDF-bestand waarmee u gegevens wilt samenvoegen
   * xmlfile: Het XML-gegevensbestand dat wordt samengevoegd met het PDF-bestand
   * saveLocation: De locatie waar het gerenderde document moet worden opgeslagen op uw bestandssysteem. Bijvoorbeeld c:\\\outputsample.pdf.

**Gegevens exporteren uit PDF-bestand**
* FormsService gebruiken om gegevens uit PDF-bestand te exporteren
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parameters aanvragen:**

   * pdffile : Het PDF-bestand waaruit u gegevens wilt exporteren
   * saveLocation: De locatie waar de geëxporteerde gegevens moeten worden opgeslagen op uw bestandssysteem

[U kunt deze postmanverzameling importeren om de API te testen](assets/document-services-postman-collection.json)

