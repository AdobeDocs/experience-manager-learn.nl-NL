---
title: De AEM van de trekker op de Verzending van het Vorm van HTML5 - Gebruiksgeval krijgen om te werken
description: De voorbeeldbestanden op uw lokale systeem implementeren
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 0%

---

# Gebruiksscenario&#39;s voor dit gebruik aan uw systeem laten werken

>[!NOTE]
>
>De voorbeeldbestanden werken alleen op uw systeem als u toegang hebt tot een AEM Forms-auteur en een AEM Forms-publicatieexemplaar.

Ga als volgt te werk om deze kwestie van het gebruik op uw lokale systeem te laten werken:

## Het volgende implementeren op uw AEM Forms-auteurinstantie

* [De MobileFormToWorkflow-bundel installeren](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [ stel het Ontwikkelen met de bundel van de Gebruiker van de Dienst ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en) op
Voeg de volgende ingang in de Dienst van het Mapper van de Gebruiker van de Dienst van Apache Sling toe gebruikend configMgr

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* U kunt de vormvoorlegging in een verschillende omslag opslaan door de omslagnaam in de configuratie van de Referenties van de AEMServer te specificeren gebruikend [ configMgr ](http://localhost:4502/system/console/configMg). Als u de omslag verandert, zorg ervoor u een lancerer op de omslag creeert om de werkstroom **te teweegbrengen ReviewSubmissionPDF**

![ config-auteur ](assets/author-config.png)
* [ voer de steekproef xdp en het werkschemapakket in gebruikend pakketmanager ](assets/xdp-form-and-workflow.zip).


## De volgende elementen implementeren in een publicatie-instantie

* [De MobileFormToWorkflow-bundel installeren](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* Specificeer de gebruikersbenaming/het wachtwoord voor de auteursinstantie en een **bestaande plaats in uw AEM bewaarplaats** om de voorgelegde gegevens in de geloofsbrieven van de AEMServer op te slaan gebruikend [ configMgr ](http://localhost:4503/system/console/configMgr). U kunt URL van het eindpunt op AEM Server van het Werkschema verlaten zoals is. Dit is het eindpunt dat de gegevens uit de voorlegging in gespecificeerde knoop haalt en opslaat.
  ![ publiceren-config ](assets/publish-config.png)

* [ stel het Ontwikkelen met de bundel van de Gebruiker van de Dienst ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en) op
* [ open de configuratie van osgi ](http://localhost:4503/system/console/configMgr).
* Onderzoek naar **Apache het Verdelen Filter van de Verwijzing**. Controleer of het selectievakje Lege waarden toestaan is ingeschakeld.


## De oplossing testen

* Aanmelden bij de instantie van uw auteur
* [ geeft de geavanceerde eigenschappen van w9.xdp ](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp) uit. Zorg ervoor dat de verzendURL en het renderprofiel op de juiste wijze zijn ingesteld, zoals hieronder wordt weergegeven.
  ![ xdp-advanced-properties ](assets/mobile-form-properties.png)

* Publish de w9.xdp
* Aanmelden voor publicatie-instantie
* [ Voorproef de w9 vorm ](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* Een aantal formuliervelden invullen en het formulier verzenden
* Aanmelden bij AEM instantie Auteur als beheerder
* [ Controle de AEM Inbox ](http://localhost:4502/aem/inbox)
* Je moet een werkitem hebben om de verzonden PDF te bekijken

>[!NOTE]
>
>In plaats van de PDF naar servlet te verzenden die op publicatieexemplaar loopt, hebben sommige klanten servlet in servlet container zoals Tomcat opgesteld. Het hangt allemaal van de topologie af de klant comfortabel met is.Voor het doel van dit leerprogramma gaan wij servlet gebruiken die bij wordt opgesteld om instantie te publiceren voor de behandeling van de vormvoorlegging wordt opgesteld.
