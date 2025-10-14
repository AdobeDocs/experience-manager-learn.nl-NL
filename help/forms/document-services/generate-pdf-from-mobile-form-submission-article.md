---
title: PDF genereren op basis van HTML5-formulierverzending
description: PDF genereren op basis van mobiele formulierverzending
feature: Mobile Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
duration: 132
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# PDF genereren op basis van HTML5-formulierverzending {#generate-pdf-from-htm-form-submission}

In dit artikel worden de stappen doorlopen die nodig zijn voor het genereren van pdf-formulieren via een HTML5-formulierverzending (ook bekend als Mobile Forms). In deze demo worden ook de stappen uitgelegd die nodig zijn om een afbeelding toe te voegen aan het HTML5-formulier en de afbeelding samen te voegen in de uiteindelijke PDF.


Om de voorgelegde gegevens in het xdp malplaatje samen te voegen doen wij het volgende

Schrijf een servlet voor de verzending van HTML5-formulieren

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

XDP-sjabloon - In de xdp-sjabloon hebben we een afbeeldingsveld en een knop met de naam btnAddImage toegevoegd. De volgende code handelt de klikgebeurtenis van btnAddImage in ons douaneprofiel af. Zoals u kunt zien, activeren wij de file1 klikgebeurtenis. Er is geen codering nodig in de xdp om deze use-case te voltooien

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[&#x200B; Profiel van de Douane &#x200B;](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). Met een aangepast profiel kunt u gemakkelijker HTML DOM-objecten van het mobiele formulier bewerken. Er wordt een verborgen bestandselement toegevoegd aan HTML.jsp. Wanneer de gebruiker op &quot;Uw foto toevoegen&quot; klikt, wordt de gebeurtenis click van het bestandselement geactiveerd. Op deze manier kan de gebruiker door de foto bladeren en de foto selecteren die u wilt bijvoegen. Vervolgens gebruiken we het Javascript FileReader-object om de base64-gecodeerde tekenreeks van de afbeelding op te halen. De base64-afbeeldingstekenreeks wordt opgeslagen in het tekstveld in het formulier. Wanneer het formulier wordt verzonden, extraheren wij deze waarde en voegen u deze in het img-element van de XML in. Deze XML wordt vervolgens gebruikt om samen te voegen met de xdp om de uiteindelijke PDF te genereren.

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
* HTML-renderprofiel - Zorg ervoor dat u &quot;AddImageToMobileForm&quot; selecteert. Hierdoor wordt de code geactiveerd om afbeelding aan het formulier toe te voegen.

Voer de volgende stappen uit om deze mogelijkheid op uw eigen server te testen:

* [De AemFormsDocumentServices-bundel implementeren](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Implementeer de bundel Developing with Service User](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Download en installeer het pakket dat aan dit artikel is gekoppeld.](assets/pdf-from-mobile-form-submission.zip)

* Zorg voor verstuur URL en HTML teruggeeft profiel correct wordt geplaatst door de eigenschappen pagina van [&#x200B; te bekijken xdp &#x200B;](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [&#x200B; Voorproef XDP als html &#x200B;](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Voeg een afbeelding toe aan het formulier en verzend deze. PDF moet terug met de afbeelding erin.
