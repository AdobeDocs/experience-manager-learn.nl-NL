---
title: Stel de steekproefactiva op uw server op
description: Gebruik de gebruikscase op uw lokale server
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 44f4261b-d6fe-42ad-a3aa-2a36ca897b5e
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# De elementen implementeren

De volgende middelen/configuraties zijn geïmplementeerd op een AEM Forms-publicatieserver.

* [Adobe Sign Wrapper Bundle](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Voorbeeld van interactieve communicatiesjabloon](assets/waiver-interactive-communication.zip)
* [De DevelopingWithServiceUser-bundel implementeren](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Voeg de volgende ingang in de Dienst van het Mapper van de Gebruiker van de Dienst Apache Sling toe gebruikend OSGi configMgr
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## De voorbeeldreactie-app implementeren

* [De voorbeeldreactie-app downloaden](assets/mult-step-form1.zip)
* Pak de inhoud van de reactieapp uit in een nieuwe map
* Navigeer naar de map en voer de volgende opdrachten uit

```java
npm install
npm start
```

Open het bestand EmergencyContact.js en wijzig de URL in de methode fetch in overeenstemming met uw omgeving.


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

Om het maken van vraag van de POST aan het AEM eindpunt van uw REACT app toe te laten, zult u de aangewezen entiteiten in het Toegestane gebied van Oorsprong in Adobe graniet moeten specificeren het Delen van het Beleid van het Middel van de Cross-Origin.

![cors-setting](assets/cors-settings.png)
