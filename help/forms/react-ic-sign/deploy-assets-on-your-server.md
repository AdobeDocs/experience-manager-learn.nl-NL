---
title: Stel de steekproefactiva op uw server op
description: Gebruik de gebruikscase op uw lokale server
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: f12f83fa-673a-454c-aa52-6ea769a182b7
duration: 36
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# De elementen implementeren

De volgende middelen/configuraties zijn geïmplementeerd op een AEM Forms-publicatieserver.

* [Adobe Sign Wrapper Bundle](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Voorbeeld van interactieve communicatiesjabloon](assets/waiver-interactive-communication.zip)
* [ stel de bundel DevelopingWithServiceUser ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) op
* Voeg de volgende ingang in de Dienst van het Mapper van de Gebruiker van de Dienst Apache Sling toe gebruikend OSGi configMgr
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## De voorbeeldreactie-app implementeren

* [De voorbeeldreactie-app downloaden](assets/mult-step-form1.zip)
* Pak de inhoud van de reactieapp uit in een nieuwe map
* Ga naar de map en voer de volgende opdrachten uit

```java
npm install
npm start
```

Open het bestand EmergencyContact.js en wijzig de URL in de methode fetch om deze aan te passen aan uw omgeving.


```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

Om het maken van vraag van de POST aan het AEM eindpunt van uw REACT app toe te laten, zult u de aangewezen entiteiten in het Toegestane gebied van Oorsprong in het gebied van het Beleid van het Delen van het Middel van Granite van de Adobe moeten specificeren.

![ cors-setting ](assets/cors-settings.png)

Zie [ het begrip CORS met AEM ](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html) voor meer details over de configuratieopties van CORS.
