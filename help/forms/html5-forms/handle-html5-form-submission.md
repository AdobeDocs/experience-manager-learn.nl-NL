---
title: Een HTML5-formulierverzending verwerken
description: HTML5-handler voor het verzenden van formulieren maken.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# HTML5-formulier verzenden

HTML5-formulieren kunnen worden ingediend bij een servlet die in AEM wordt gehost. De verzonden gegevens zijn toegankelijk in de server als een invoerstream. Als u uw HTML5-formulier wilt verzenden, voegt u via AEM Forms Designer een knop HTTP verzenden toe aan uw formuliersjabloon.

## Verzendhandler maken

Een eenvoudige servlet kan de verzending van het HTML5-formulier afhandelen. Extraheer de verzonden gegevens met behulp van het volgende codefragment. Download [ servlet ](assets/html5-submit-handler.zip) die in dit leerprogramma wordt verstrekt. Installeer [ servlet ](assets/html5-submit-handler.zip) gebruikend de [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp).

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

Verzeker u de [ Configuratie van SDK van de Cliënt LiveCycle van Adobe ](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) hebt gevormd als u van plan bent om de code te gebruiken om een proces aan te halen J2EE.

## De verzendURL van het HTML5-formulier configureren

![ voorlegt URL ](assets/submit-url.PNG)

- Open xdp en navigeer aan _Eigenschappen_ -> _Geavanceerd_.
- Kopieer http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html en plak het in het tekstveld URL verzenden.
- Klik _SaveAndClose_ knoop.

### Item toevoegen in Paden uitsluiten

- Ga naar [ configMgr ](http://localhost:4502/system/console/configMgr).
- Onderzoek naar _de GranietCSRF Filter van Adobe_.
- Voeg de volgende vermelding toe in de sectie Uitgesloten paden: _/content/AemFormsSamples/handlehml5formsubmission_ .
- Sla uw wijzigingen op.

### Het formulier testen

- Open de xdp-sjabloon.
- Klik op _Voorproef_ ->Voorproef als HTML.
- Voer gegevens in het formulier in en klik op Verzenden.
- Controleer het bestand stdout.log van de server op de verzonden gegevens.

### Extra lezingen

Voor meer informatie bij het produceren van PDFs van HTML5 vormvoorlegging, verwijs naar dit [ artikel ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html).

