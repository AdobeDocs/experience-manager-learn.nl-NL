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
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# De elementen implementeren

De volgende middelen/configuraties zijn geïmplementeerd op een AEM Forms-publicatieserver.

* [Adobe Sign Wrapper Bundle](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Voorbeeld van interactieve communicatiesjabloon](assets/waiver-interactive-communication.zip)
* [De DevelopingWithServiceUser-bundel implementeren](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Voeg de volgende ingang in de Dienst van het Mapper van de Gebruiker van de Dienst Apache Sling toe gebruikend OSGi configMgr
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [Hier kunt u een voorbeeld van React App-code downloaden](assets/src.zip)



De voorbeeldtoepassing moet worden geïmplementeerd in uw lokale omgeving

U zult het eindpunt URL moeten veranderen om uw milieu aan te passen. Open het bestand EmergencyContact.js en wijzig de URL in de methode fetch

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

Om het maken van vraag van de POST aan het AEM eindpunt van uw REACT app toe te laten, zult u de aangewezen entiteiten in het Toegestane gebied van Oorsprong in de Adobe van de Grensoverschrijdende Herkomst van het Middel moeten specificeren die de configuratie van het Beleid deelt

![cors-setting](assets/cors-settings.png)


