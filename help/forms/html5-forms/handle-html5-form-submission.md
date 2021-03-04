---
title: HTML5-formulierverzending verwerken
description: HTML5-formulierverzendhandler maken
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: Ontwikkeling
role: Developer
level: Ervaren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---


# HTML5-formulierverzending verwerken

HTML5-formulieren kunnen worden verzonden naar servlet die wordt gehost in AEM. De verzonden gegevens zijn toegankelijk in de server als een invoerstream. Als u uw HTML5-formulier wilt verzenden, moet u &quot;Knop HTTP verzenden&quot; toevoegen aan uw formuliersjabloon met AEM Forms Designer

## Verzendhandler maken

U kunt een eenvoudige servlet maken voor het verzenden van HTML5-formulieren. De ingediende gegevens kunnen vervolgens worden geëxtraheerd met de volgende code. Dit [servlet](assets/html5-submit-handler.zip) wordt ter beschikking gesteld aan u als deel van dit leerprogramma. Installeer [servlet](assets/html5-submit-handler.zip) met [pakketbeheer](http://localhost:4502/crx/packmgr/index.jsp)

De code van lijn 9 kan worden gebruikt om J2EE proces aan te halen. Zorg ervoor dat u [Adobe LiveCycle client SDK Configuration](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) hebt geconfigureerd als u de code wilt gebruiken om J2EE-proces aan te roepen.

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

![submit-url](assets/submit-url.PNG)

* Tik op de xdp en klik op _Eigenschappen_->_Geavanceerd_
* http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html kopiëren en plakken in het tekstveld URL verzenden
* Klik _SaveAndClose_ knoop.

### Item toevoegen in Paden uitsluiten

* Navigeer naar [configMgr](http://localhost:4502/system/console/configMgr).
* Zoeken naar _Adobe granite CSRF-filter_
* De volgende vermelding toevoegen in de sectie Uitgesloten paden
* _/content/AemFormsSamples/handlehml5formsubmission_
* Uw wijzigingen opslaan

### Het formulier testen

* Tik op de xdp-sjabloon.
* Klik op _Voorvertoning_->Voorvertoning als HTML
* Voer gegevens in het formulier in en klik op Verzenden
* De verzonden gegevens worden naar het bestand stdout.log van de server geschreven

### Extra lezingen

Dit [artikel](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) over het genereren van PDF van het verzenden van HTML5-formulieren wordt ook aanbevolen.




