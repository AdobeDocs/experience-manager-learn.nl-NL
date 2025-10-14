---
title: Stel de steekproefactiva op uw server op
description: Gebruik de gebruikscase op uw lokale server
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: f12f83fa-673a-454c-aa52-6ea769a182b7
duration: 36
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# De elementen implementeren

De volgende middelen/configuraties zijn geïmplementeerd op een AEM Forms-publicatieserver.

* [Adobe-bundel voor omsluitend ondertekenen](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Voorbeeld van interactieve communicatiesjabloon](assets/waiver-interactive-communication.zip)
* [&#x200B; stel de bundel DevelopingWithServiceUser &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=nl-NL) op
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

Als u POST-aanroepen naar het AEM-eindpunt van uw REACT-app wilt inschakelen, moet u de juiste entiteiten opgeven in het veld Toegestane originelen in de configuratie Beleid voor het delen van bronnen van Adobe Granite Cross-Origin.

![&#x200B; cors-setting &#x200B;](assets/cors-settings.png)

Zie [&#x200B; het begrip CORS met AEM &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=nl-NL) voor meer details over de configuratieopties van CORS.
