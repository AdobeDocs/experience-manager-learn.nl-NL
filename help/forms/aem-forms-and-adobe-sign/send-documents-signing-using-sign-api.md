---
title: Adobe Sign API gebruiken in AEM Forms
description: Documenten verzenden voor ondertekening met Adobe Sign-helpermethoden
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="Integratie" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 3400b728-58ca-44c3-a882-e3170755f845
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Adobe Sign-helpermethoden gebruiken

In bepaalde gevallen hebt u mogelijk de verplichting om een document voor handtekeningen te verzenden zonder een AEM workflow te gebruiken. In dergelijke gevallen is het erg handig om de verpakkingsmethoden te gebruiken die door de monsterbundel in dit artikel worden aangeboden.

## De voorbeeldbundel OSGi implementeren

[De OSGi-bundel implementeren](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar) via de AEM OSGi-webconsole. Specificeer de API integratiesleutel en API gebruiker die de configuratie OSGi zoals hieronder getoond, via de Manager van de Configuratie van de Console van het Web van AEM OSGi gebruikt.

 Houd er rekening mee dat de `AdobeSignHelperMethods` OSGi-bundel wordt niet herkend als Adobe Experience Manager (AEM)-productcode en wordt daarom niet ondersteund door ondersteuning van Adoben.
![sign-configuration](assets/sign-configuration.png)


## API-documentatie

Het volgende is beschikbaar via de `AcrobatSignHelperMethods` De OSGi-dienst werd geleverd in de OSGi-bundel.

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


Het document waarmee een overeenkomst of een webformulier wordt gemaakt. Het document wordt eerst door de afzender geüpload naar Acrobat Sign. Dit wordt aangeduid als _transient_ omdat het slechts 7 dagen na het uploaden beschikbaar is voor gebruik. Deze methoden accepteren `com.adobe.aemfd.docmanager.Document` en retourneert tijdelijke document-id.

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

Verzend het document voor ondertekening met de tijdelijke document-id voor ondertekening naar de gebruiker die door de parameter email wordt geïdentificeerd.

### getWidgetID

`String getWidgetID(String transientDocumentID)`

Een widget is vergelijkbaar met een herbruikbare sjabloon die meerdere keren aan gebruikers kan worden getoond en meerdere keren kan worden ondertekend. Gebruik deze methode om widget-id op te halen met behulp van de tijdelijke document-id.

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

Een widget-URL ophalen voor een specifieke widget-id. Deze widget-URL kan vervolgens aan de gebruikers worden getoond voor ondertekening van het document.

## De API gebruiken

De `AcrobatSignHelperMethods` is een dienst OSGi, zodat moet het worden geannoteerd gebruikend de @annotation in uw java code.

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```
