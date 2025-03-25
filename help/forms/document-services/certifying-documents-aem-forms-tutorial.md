---
title: Document certificeren in AEM Forms
description: PDF-documenten certificeren in AEM Forms met de documentatieservice
feature: Document Security
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 75
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# Document certificeren in AEM Forms

Een gecertificeerd document biedt ontvangers van PDF-documenten en formulieren extra garanties ten aanzien van authenticiteit en integriteit.

Als u een document wilt certificeren, kunt u Acrobat DC op het bureaublad of AEM Forms Document Services gebruiken als onderdeel van een geautomatiseerd proces op een server.

Dit artikel verstrekt u steekproefOSGI bundel om pdf- documenten te certificeren gebruikend het Document Services van AEM Forms.De code die in de steekproef wordt gebruikt is [ hier beschikbaar ](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Als u documenten wilt certificeren met AEM Forms, moet u de volgende stappen uitvoeren

## Certificaat toevoegen aan vertrouwde winkel {#adding-certificate-to-trust-store}

Voer de onderstaande stappen uit om het certificaat toe te voegen aan het sleutelarchief in AEM

* [ initialiseer globale vertrouwensopslag ](http://localhost:4502/libs/granite/security/content/truststore.html)
* [ Onderzoek naar fd-dienst ](http://localhost:4502/security/users.html) gebruiker
* **u zult de resultatenpagina moeten scrollen om alle gebruikers te laden om de fd-dienst gebruiker** te vinden
* Dubbelklik op de gebruiker van de fd-service om het venster met gebruikersinstellingen te openen
* Klik op &quot;Persoonlijke sleutel toevoegen uit het sleutelarchiefbestand&quot;.Geef de alias en het wachtwoord op die specifiek zijn voor uw certificaat
  ![ toe:voegen-certificaat ](assets/adding-certificate-keystore.PNG)
* Uw wijzigingen opslaan

## OSGI-service maken

U kunt uw eigen OSGi-bundel schrijven en AEM Forms Client SDK gebruiken om een service te implementeren voor de certificering van PDF-documenten. De volgende verbindingen zouden nuttig zijn om uw eigen bundel te schrijven OSGi

* [ Creërend uw eerste bundel OSGi ](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [ de Dienst API van het Document van het Gebruik ](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

U kunt de voorbeeldbundel ook gebruiken als onderdeel van deze zelfstudie-elementen.

>[!NOTE]
>
>De voorbeeldbundel gebruikt de alias &quot;ares&quot; om de documenten te certificeren. Zorg er dus voor dat uw alias &quot;ares&quot; wordt genoemd wanneer u deze bundel gebruikt

## Het voorbeeld op uw lokale systeem testen

* De download en installeert [ Bundel van de Diensten van het Document van de Douane ](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Download en installeer [ Ontwikkelen met de Bundel van de Gebruiker van de Dienst ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [ zorg ervoor u de volgende ingang in de Dienst van het Mapper van de Gebruiker van de Dienst Apache Sling ](http://localhost:4502/system/console/configMgr) hebt toegevoegd
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** zoals aangetoond in het hieronder scherm
  ![ gebruiker-Mapper ](assets/user-mapper-service.PNG)
* [Voorbeeld van adaptief formulier importeren](assets/certify-pdf-af.zip)
* [Aangepast verzenden importeren en installeren](assets/custom-submit-certify.zip)
* [ open de Aangepaste Vorm ](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* PDF-document uploaden dat moet worden gecertificeerd
  **facultatief** - specificeer het handtekeningsgebied dat u in het verklaren van het document wilt gebruiken
* Klik op Verzenden.
* Gecertificeerde PDF moet naar je worden teruggestuurd.
