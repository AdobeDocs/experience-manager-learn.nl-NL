---
title: Aangepaste verzendservice maken voor het verzenden van koploze, adaptieve formulieren
description: Aangepaste reactie retourneren op basis van de verzonden gegevens
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: c23275d7-daf7-4a42-83b6-4d04b297c470
duration: 134
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 0%

---

# Aangepaste verzending maken

AEM Forms bevat een aantal verzendopties uit het vak die voldoen aan de meeste gebruiksgevallen. Naast deze vooraf gedefinieerde verzendacties kunt u in AEM Forms uw eigen aangepaste verzendhandler schrijven om het verzenden van het formulier naar wens te verwerken.

Om een douane te schrijven verzend de dienst, werden de volgende stappen gevolgd

## AEM project maken

Als u al een bestaand AEM Forms Cloud Service-project hebt, kunt u [springen naar het schrijven van de aangepaste verzendservice](#Write-the-custom-submit-service)

* Maak een map met de naam cloudmanager op het station C.
* Naar deze nieuwe map navigeren
* Inhoud van [dit tekstbestand](./assets/creating-maven-project.txt) in uw bevel snel venster.U kunt DarchetypeVersion=41 afhankelijk van moeten veranderen [nieuwste versie](https://github.com/adobe/aem-project-archetype/releases). De meest recente versie was 41 op het moment dat dit artikel werd geschreven.
* Voer het bevel uit door op Enter te drukken.Als alles correct gaat zou u het bericht van het bouwstijlsucces moeten zien.

## De aangepaste verzendservice schrijven{#Write-the-custom-submit-service}

Start IntelliJ en open AEM project. Een nieuwe Java-klasse maken met de naam **HandleRegistrationFormSubmission** zoals weergegeven in onderstaande schermafbeelding
![custom-submit-service](./assets/custom-submit-service.png)

De volgende code is geschreven om de service te implementeren

```java
package com.aem.bankingapplication.core;
import java.util.HashMap;
import java.util.Map;
import com.google.gson.Gson;
import org.osgi.service.component.annotations.Component;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.google.gson.JsonObject;
import org.slf4j.*;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class HandleRegistrationFormSubmission implements FormSubmitActionService {
    private static final String serviceName = "Core Custom AF Submit";
    private static Logger logger = LoggerFactory.getLogger(HandleRegistrationFormSubmission.class);



    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        logger.error("in my custom submit service");
        Map<String, Object> result = new HashMap<>();
        logger.error("in my custom submit service");
        String data = formSubmitInfo.getData();
        JsonObject formData = new Gson().fromJson(data,JsonObject.class);
        logger.error("The form data is "+formData);
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("firstName",formData.get("firstName").getAsString());
        jsonObject.addProperty("lastName",formData.get("lastName").getAsString());
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
        result.put("json",jsonObject.toString());
        return result;
    }

}
```

## Een crx-knooppunt maken onder apps

Het knooppunt ui.apps uitbreiden om een nieuw pakket met de naam **HandleRegistrationFormSubmission** onder het knooppunt Apps, zoals in de onderstaande schermafbeelding wordt getoond
![crx-node](./assets/crx-node.png)
Maak een bestand met de naam .content.xml onder de lijst **HandleRegistrationFormSubmission**. Kopieer en plak de volgende code in de .content.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="Handle Registration Form Submission"
    jcr:primaryType="sling:Folder"
    guideComponentType="fd/af/components/guidesubmittype"
    guideDataModel="xfa,xsd,basic"
    submitService="Core Custom AF Submit"/>
```

De waarde van **submitService** element moet overeenkomen  **serviceName = &quot;Core Custom AF Submit&quot;** in de FormSubmitActionService-implementatie.

## De code naar uw lokale AEM Forms-instantie implementeren

Voordat u de wijzigingen doorvoert naar de gegevensopslagruimte voor cloudbeheer, wordt aanbevolen de code te implementeren in de ontwerpinstantie van uw lokale cloud om de code te testen. Zorg ervoor dat de instantie van de auteur wordt uitgevoerd.
Als u de code wilt implementeren naar de ontwerpinstantie in de cloud, navigeert u naar de hoofdmap van uw AEM project en voert u de volgende opdracht uit

```
mvn clean install -PautoInstallSinglePackage
```

Hiermee wordt de code als één pakket geïmplementeerd op uw auteurinstantie

## Push the code to cloud manager and Deploy the code

Nadat u de code op uw lokale instantie hebt gecontroleerd, drukt u op de code naar uw cloudinstantie.
Breng de wijzigingen aan in uw lokale opslagplaats voor it en vervolgens aan het cloudbeheerprogramma. U kunt verwijzen naar de  [Git-instelling](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/setup-git.html), [AEM project naar cloudbeheergegevensopslagruimte duwen](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git.html) en [inzetten in de ontwikkelomgeving](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/deploy-to-dev-environment.html) artikelen.

Zodra de pijplijn met succes is uitgevoerd, zou u de verzendactie van uw vorm aan de douane moeten kunnen associëren voorlegt manager zoals aangetoond in het hieronder ontsproten scherm.
![voorlegging](./assets/configure-submit-action.png)

## Volgende stappen

[Aangepaste reactie weergeven in uw reactie-app](./handle-response-react-app.md)
