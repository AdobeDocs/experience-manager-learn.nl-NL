---
title: HTML5-formulierverzending verwerken
description: HTML5-handler voor formulierverzending maken
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# HTML5-formulierverzending verwerken

HTML5-formulieren kunnen worden ingediend bij servlet die in AEM wordt gehost. De verzonden gegevens zijn toegankelijk in de server als een invoerstream. Als u uw HTML5-formulier wilt verzenden, moet u de knop HTTP verzenden aan uw formuliersjabloon toevoegen met AEM Forms Designer

## Verzendhandler maken

U kunt een eenvoudige servlet maken voor het verzenden van het HTML5-formulier. De ingediende gegevens kunnen vervolgens worden geëxtraheerd met de volgende code. Dit [ servlet ](assets/html5-submit-handler.zip) wordt ter beschikking gesteld aan u als deel van dit leerprogramma. Gelieve te installeren [ servlet ](assets/html5-submit-handler.zip) gebruikend [ pakketmanager ](http://localhost:4502/crx/packmgr/index.jsp)

De code van lijn 9 kan worden gebruikt om J2EE proces aan te halen. Gelieve te zorgen u {de Configuratie van SDK van de Cliënt van het LiveCycle van de Adobe ](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) hebt gevormd als u van plan bent de code te gebruiken om J2EE proces aan te halen.[

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## De verzendURL van het HTML5-formulier configureren

![ voorleggen-url ](assets/submit-url.PNG)

* Tik op xdp en klik _Eigenschappen_ -> _Geavanceerd_
* http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html kopiëren en plakken in het tekstveld URL verzenden
* Klik _SaveAndClose_ knoop.

### Item toevoegen in Paden uitsluiten

* Navigeer aan [ configMgr ](http://localhost:4502/system/console/configMgr).
* Onderzoek naar _de Filter van Granite CSRF van de Adobe_
* De volgende vermelding toevoegen in de sectie Uitgesloten paden
* _/content/AemFormsSamples/handlehml5formsubmission_
* Uw wijzigingen opslaan

### Het formulier testen

* Tik op de xdp-sjabloon.
* Klik op _Voorproef_ ->Voorproef als HTML
* Voer gegevens in het formulier in en klik op Verzenden
* De verzonden gegevens worden naar het bestand stdout.log van de server geschreven

### Extra lezingen

Dit [ artikel ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) bij het produceren van PDF van HTML5 vormvoorlegging wordt ook geadviseerd.
