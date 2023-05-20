---
title: Ontwikkelen met Output and Forms Services in AEM Forms
description: Uitvoer- en Forms Service-API in AEM Forms gebruiken
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 46df7b13401ee3497c871eac3b8158148c2e6a04
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# Ontwikkelen met Output and Forms Services in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Uitvoer- en Forms Service-API in AEM Forms gebruiken

In dit artikel zullen we het volgende bekijken:

* Uitvoerservice - Deze service wordt doorgaans gebruikt om XML-gegevens samen te voegen met de xdp-sjabloon of pdf om samengevoegde pdf te genereren. Voor meer informatie, gelieve te verwijzen naar dit [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) voor de service Uitvoer.
* FormsService - Dit is een zeer veelzijdige service waarmee u gegevens kunt exporteren/importeren vanuit en naar een PDF-bestand. Voor meer informatie, gelieve te verwijzen naar dit [javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) voor de Forms-service.


Het volgende codefragment exporteert gegevens uit het PDF-bestand

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

Regel 6 voert xmlData uit het Dossier van PDF uit

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
1. Schakel het selectievakje Lege toestaan in. (Deze instelling is alleen bedoeld voor testdoeleinden) Er zijn verschillende manieren om de voorbeeldcode te testen. De snelste en eenvoudigste manier is om Postman-app te gebruiken. Met Postman kunt u POSTEN aanvragen bij uw server. Installeer de Postman-toepassing op uw systeem.
Start de app en voer de volgende URL in om de API voor exportgegevens te testen

Controleer of u &quot;POST&quot; hebt geselecteerd in de vervolgkeuzelijst http://localhost:4502/content/AemFormsSamples/exportdata.html Controleer of u &quot;Autorisatie&quot; opgeeft als &quot;Basisauth&quot;. Geef de gebruikersnaam en het wachtwoord voor AEM server op en ga naar het tabblad Body en geef de aanvraagparameters op zoals in de onderstaande afbeelding wordt getoond
![export](assets/postexport.png)
Klik vervolgens op de knop Verzenden

De verpakking bevat 3 monsters. De volgende paragrafen verklaren wanneer om de outputdienst of de Dienst van Forms te gebruiken, de url van de dienst, inputparameters die elke dienst verwacht

## Gegevens samenvoegen en uitvoer afvlakken

* Met Uitvoerservice kunt u gegevens samenvoegen met xdp- of pdf-document om samengevoegde pdf te genereren
* **POST-URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parameters aanvragen -**

   * **xdp_or_pdf_file** : Het xdp- of pdf-bestand waarmee u gegevens wilt samenvoegen
   * **xmlfile**: Het XML-gegevensbestand dat is samengevoegd met het bestand xdp_or_pdf_file
   * **saveLocation**: De locatie waar het gerenderde document moet worden opgeslagen op uw bestandssysteem. Bijvoorbeeld c:\\documents\\sample.pdf

### Gegevens importeren in PDF-bestand

* FormsService gebruiken om gegevens te importeren in PDF-bestand
* **POST-URL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Parameters aanvragen:**

   * **pdffile** : Het PDF-bestand waarmee u gegevens wilt samenvoegen
   * **xmlfile**: Het XML-gegevensbestand dat wordt samengevoegd met het PDF-bestand
   * **saveLocation**: De locatie waar het gerenderde document moet worden opgeslagen op uw bestandssysteem. Bijvoorbeeld `c:\\outputsample.pdf`.

**Gegevens exporteren uit PDF-bestand**
* FormsService gebruiken om gegevens uit het PDF-bestand te exporteren
* **POST UR** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parameters aanvragen:**

   * **pdffile** : Het PDF-bestand waaruit u gegevens wilt exporteren
   * **saveLocation**: De locatie waar de geëxporteerde gegevens moeten worden opgeslagen op uw bestandssysteem. Bijvoorbeeld c:\\documents\\exported_data.xml

[U kunt deze postmanverzameling importeren om de API te testen](assets/document-services-postman-collection.json)
