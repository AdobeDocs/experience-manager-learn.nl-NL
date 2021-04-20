---
title: PDF genereren van HTM5-formulierverzending
seo-title: PDF genereren van HTML5-formulierverzending
description: PDF genereren van verzending van mobiele formulieren
seo-description: PDF genereren van verzending van mobiele formulieren
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---


# PDF genereren van HTM5-formulierverzending {#generate-pdf-from-htm-form-submission}

In dit artikel worden de stappen doorlopen die nodig zijn voor het genereren van PDF-bestanden via het verzenden van een HTML5-formulier (ook bekend als Mobile Forms). In deze demo worden ook de stappen uitgelegd die nodig zijn om een afbeelding toe te voegen aan HTML5-formulier en de afbeelding samen te voegen in de uiteindelijke PDF.

Voor een live demonstratie van deze mogelijkheid gaat u naar de [voorbeeldserver](https://forms.enablementadobe.com/content/samples/samples.html?query=0) en zoekt u naar &quot;Mobiel formulier naar PDF&quot;.

Om de voorgelegde gegevens in het xdp malplaatje samen te voegen doen wij het volgende

Een servlet schrijven om de verzending van HTML5-formulieren af te handelen

* In deze server worden de verzonden gegevens opgeslagen
* Deze gegevens samenvoegen met de xdp-sjabloon om pdf te genereren
* De pdf terugsturen naar de opvragende toepassing

Hier volgt de servletcode die de ingediende gegevens uit het verzoek extraheert. Vervolgens wordt de aangepaste methode documentServices.mobileFormToPDF aangeroepen om de PDF op te halen.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
   response.setContentType("application/pdf");
   response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
   response.setContentLength((int) fileInputStream.available());
   OutputStream responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    responseOutputStream.write(bytes);
   }
   responseOutputStream.flush();
   responseOutputStream.close();

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

We hebben het volgende gebruikt om een afbeelding toe te voegen aan een mobiel formulier en die afbeelding weer te geven in de PDF

XDP-sjabloon - In de xdp-sjabloon hebben we een afbeeldingsveld en een knop met de naam btnAddImage toegevoegd. De volgende code handelt de klikgebeurtenis van btnAddImage in ons douaneprofiel af. Zoals u kunt zien, activeren wij de file1 klikgebeurtenis. Er is geen codering nodig in de xdp om deze use case te voltooien

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Aangepast profiel](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). Met een aangepast profiel kunt u HTML DOM-objecten van het mobiele formulier gemakkelijker bewerken. Er wordt een verborgen bestandselement toegevoegd aan HTML.jsp. Wanneer de gebruiker op &quot;Uw foto toevoegen&quot; klikt, wordt de gebeurtenis click van het bestandselement geactiveerd. Op deze manier kan de gebruiker door de foto bladeren en de foto selecteren die u wilt bijvoegen. Vervolgens gebruiken we het Javascript FileReader-object om de base64-gecodeerde tekenreeks van de afbeelding op te halen. De base64-afbeeldingstekenreeks wordt opgeslagen in het tekstveld in het formulier. Wanneer het formulier wordt verzonden, extraheren wij deze waarde en voegen u deze in het img-element van de XML in. Deze XML wordt vervolgens gebruikt om samen te voegen met de xdp om de uiteindelijke PDF te genereren.

Het aangepaste profiel dat voor dit artikel wordt gebruikt, is beschikbaar gesteld als onderdeel van de elementen van dit artikel.

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

De bovenstaande code wordt uitgevoerd wanneer de gebeurtenis click van het bestandselement wordt geactiveerd. Met regel 5 extraheert u de inhoud van het geüploade bestand als base64-tekenreeks en slaat u dit op in het tekstveld. Deze waarde wordt vervolgens opgehaald wanneer het formulier bij ons servlet wordt ingediend.

Vervolgens configureren we de volgende (geavanceerde) eigenschappen van ons mobiele formulier in AEM

* URL verzenden - http://localhost:4502/bin/handlemobileformsubmission. Dit is onze servlet die de voorgelegde gegevens met het xdp malplaatje zal samenvoegen
* HTML-renderprofiel - zorg dat u &quot;AddImageToMobileForm&quot; selecteert. Hierdoor wordt de code geactiveerd om afbeelding aan het formulier toe te voegen.

Voer de volgende stappen uit om deze mogelijkheid op uw eigen server te testen:

* [De AemFormsDocumentServices-bundel implementeren](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Implementeer de bundel Developing with Service User](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Download en installeer het pakket dat aan dit artikel is gekoppeld.](assets/pdf-from-mobile-form-submission.zip)

* Zorg ervoor dat het verzendURL- en HTML-renderprofiel correct zijn ingesteld door de eigenschappenpagina van [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp) te bekijken

* [Voorvertoning van de XDP weergeven als HTML](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Voeg een afbeelding toe aan het formulier en verzend deze. U moet de PDF terugkrijgen met de afbeelding in de PDF.

