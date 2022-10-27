---
title: Document certificeren in AEM Forms
description: De documentatiedienst gebruiken om PDF-documenten te certificeren in AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Document certificeren in AEM Forms

Een gecertificeerd document biedt ontvangers van PDF-documenten en formulieren extra garanties ten aanzien van authenticiteit en integriteit.

Als u een document wilt certificeren, kunt u Acrobat DC op het bureaublad of AEM Forms Document Services gebruiken als onderdeel van een geautomatiseerd proces op een server.

In dit artikel vindt u voorbeeldversies van de OSGI-bundel voor het certificeren van PDF-documenten met AEM Forms Document Services. De code in het voorbeeld is [hier beschikbaar](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Als u documenten wilt certificeren met AEM Forms, moet u de volgende stappen uitvoeren

## Certificaat toevoegen aan vertrouwde winkel {#adding-certificate-to-trust-store}

Volg de onderstaande stappen om het certificaat toe te voegen aan het sleutelarchief in AEM

* [Globale vertrouwde winkel initialiseren](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Zoeken naar fd-service](http://localhost:4502/security/users.html) user
* **U zult de resultatenpagina moeten scrollen om alle gebruikers te laden om de fd-dienst gebruiker te vinden**
* Dubbelklik op de gebruiker van de fd-service om het venster met gebruikersinstellingen te openen
* Klik op &quot;Persoonlijke sleutel toevoegen uit het sleutelarchiefbestand&quot;.Geef de alias en het wachtwoord op die specifiek zijn voor uw certificaat
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Uw wijzigingen opslaan

## OSGI-service maken

U kunt uw eigen bundel OSGi schrijven en de AEM Forms Cliënt SDK gebruiken om de dienst uit te voeren om de documenten van de PDF te certificeren. De volgende verbindingen zouden nuttig zijn om uw eigen bundel te schrijven OSGi

* [Uw eerste OSGi-bundel maken](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Documentservice-API gebruiken](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

U kunt de voorbeeldbundel ook gebruiken als onderdeel van deze zelfstudie-elementen.

>[!NOTE]
>
>De voorbeeldbundel gebruikt de alias &quot;ares&quot; om de documenten te certificeren. Zorg er dus voor dat uw alias &quot;ares&quot; wordt genoemd wanneer u deze bundel gebruikt

## Het voorbeeld op uw lokale systeem testen

* Downloaden en installeren [Pakket voor aangepaste documentservices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Downloaden en installeren [Ontwikkelen met de Bundel van de Gebruiker van de Dienst](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Controleer of u de volgende vermelding hebt toegevoegd aan de Apache Sling Service User Mapper Service](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** zoals weergegeven in onderstaande schermafbeelding
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Voorbeeld van adaptief formulier importeren](assets/certify-pdf-af.zip)
* [Aangepast verzenden importeren en installeren](assets/custom-submit-certify.zip)
* [Het adaptieve formulier openen](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* PDF-document uploaden dat moet worden gecertificeerd
   **optioneel** - Geef het handtekeningveld op dat u wilt gebruiken voor de certificering van het document
* Klik op Verzenden.
* Gecertificeerde PDF moet naar u worden teruggestuurd.
