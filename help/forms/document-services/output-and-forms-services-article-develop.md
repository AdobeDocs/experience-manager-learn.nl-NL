---
title: Ontwikkelen met Output and Forms Services in AEM Forms
description: Meer informatie over het ontwikkelen met Output en Forms Service API in AEM Forms.
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
duration: 122
source-git-commit: 12af84e3d9be24fabb01a64eced6279749668599
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 0%

---

# Ontwikkelen met Output and Forms Services in AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Meer informatie over het ontwikkelen met Output en Forms Service API in AEM Forms.

In dit artikel zullen we het volgende bekijken:

* [ de Dienst van de Output ](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) - typisch wordt deze dienst gebruikt om xml- gegevens met xdp malplaatje of pdf samen te voegen om samengevoegde pdf te produceren.
* [ FormsService ](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) - dit is de zeer veelzijdige dienst die u toestaat om xdp als pdf en uitvoer/invoergegevens van en in het dossier van PDF terug te geven.


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

**om het steekproefpakket op uw systeem** te testen

[Download en installeer het pakket met AEM pakketbeheer](assets/using-output-and-form-service-api.zip)




**nadat u het pakket installeert zult u volgende URLs in de Filter van de Adobe moeten lijsten van gewenste personen Granite CSRF.**

1. Volg de onderstaande stappen om de hierboven vermelde paden te lijsten van gewenste personen.
1. [ Login aan configMgr ](http://localhost:4502/system/console/configMgr)
1. Zoeken naar graniet-CSRF-filter voor Adobe
1. Voeg de volgende drie paden toe aan de uitgesloten secties en sla deze op
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. Zoeken naar het filter Verticale verwijzing
1. Schakel het selectievakje Lege toestaan in. (Deze instelling mag alleen voor testdoeleinden worden gebruikt)

## De monsters testen

U kunt de voorbeeldcode op verschillende manieren testen. De snelste en eenvoudigste manier is om Postman-app te gebruiken. Met Postman kunt u POSTEN aanvragen bij uw server.

* Installeer de Postman-toepassing op uw systeem.
* Start de app en voer de juiste URL in
* Controleer of u &quot;POST&quot; hebt geselecteerd in de vervolgkeuzelijst
* Zorg ervoor dat u &quot;Autorisatie&quot; opgeeft als &quot;Basic Auth&quot;. Geef de gebruikersnaam en het wachtwoord voor AEM server op
* Geef de aanvraagparameters op op het tabblad body
* Klik op Verzenden

De verpakking bevat 4 monsters. De volgende paragrafen verklaren wanneer om de outputdienst of de Dienst van Forms te gebruiken, de url van de dienst, inputparameters die elke dienst verwacht

## OutputService gebruiken om gegevens samen te voegen met xdp-sjabloon

* Met Uitvoerservice kunt u gegevens samenvoegen met xdp- of pdf-document om samengevoegde pdf te genereren
* **POST URL**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Parameters van het Verzoek -**

   * **xdp_or_pdf_file** : Het xdp of pdf- dossier dat u gegevens met wilt samenvoegen
   * **xmlfile**: Het xml- gegevensdossier dat met xdp_or_pdf_file wordt samengevoegd
   * **saveLocation**: De plaats om het teruggegeven document op uw dossiersysteem op te slaan. Bijvoorbeeld c:\\documents\\sample.pdf

### FormsService API gebruiken

#### Gegevens importeren

* FormsService-importData gebruiken om gegevens te importeren in PDF-bestand
* **POST URL** - http://localhost:4502/content/AemFormsSamples/mergedata.html

* **Parameters van het Verzoek:**

   * **pdfile** : Het pdf- dossier dat u gegevens met wilt samenvoegen
   * **xmlfile**: Het xml- gegevensdossier dat met pdf- dossier wordt samengevoegd
   * **saveLocation**: De plaats om het teruggegeven document op uw dossiersysteem op te slaan. Bijvoorbeeld `c:\\outputsample.pdf` .

#### Gegevens exporteren

* FormsService-API voor exportData gebruiken om gegevens uit PDF File te exporteren
* **POST URL** - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Parameters van het Verzoek:**

   * **pdfile** : Het pdf- dossier dat u gegevens van wilt uitvoeren
   * **saveLocation**: De plaats om de uitgevoerde gegevens op uw dossiersysteem op te slaan. Bijvoorbeeld c:\\documents\\export_data.xml

#### XDP renderen

* XDP-sjabloon renderen als statisch/dynamisch pdf
* FormsService renderPDFForm-API gebruiken om xdp-sjabloon te renderen als PDF
* **POST URL** - http://localhost:4502/content/AemFormsSamples/renderxdp?xdpName=f1040.xdp
* Request-parameter:
   * xdpName: naam van het xdp-bestand dat moet worden gerenderd als een pdf

[U kunt deze postmanverzameling importeren om de API te testen](assets/UsingDocumentServicesInAEMForms.postman_collection.json)

