---
title: Document certificeren in AEM Forms
seo-title: Document certificeren in AEM Forms
description: PDF-documenten certificeren in AEM Forms met de documentatieservice
seo-description: PDF-documenten certificeren in AEM Forms met de documentatieservice
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: documentbeveiliging
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 0%

---


# Document certificeren in AEM Forms

Een gecertificeerd document biedt ontvangers van PDF-documenten en formulieren extra garanties ten aanzien van authenticiteit en integriteit.

Als u een document wilt certificeren, kunt u Acrobat DC gebruiken op het bureaublad of AEM Forms Document Services als onderdeel van een geautomatiseerd proces op een server.

In dit artikel vindt u voorbeelden van OSGI-bundels voor het certificeren van PDF-documenten met AEM Forms Document Services. De code in het voorbeeld is [beschikbaar hier](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Als u documenten wilt certificeren met AEM Forms, moet u de volgende stappen uitvoeren

## Certificaat toevoegen aan vertrouwde opslag {#adding-certificate-to-trust-store}

Volg de onderstaande stappen om het certificaat toe te voegen aan het sleutelarchief in AEM

* [Globale vertrouwde winkel initialiseren](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Zoeken naar Ffd-](http://localhost:4502/security/users.html) Service-gebruiker
* **U zult de resultatenpagina moeten scrollen om alle gebruikers te laden om de fd-dienst gebruiker te vinden**
* Dubbelklik op de gebruiker van de fd-service om het venster met gebruikersinstellingen te openen
* Klik op &quot;Persoonlijke sleutel toevoegen uit het sleutelarchiefbestand&quot;.Geef de alias en het wachtwoord op die specifiek zijn voor uw certificaat
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Uw wijzigingen opslaan

## OSGI-service maken

U kunt uw eigen OSGi-bundel schrijven en de AEM Forms Client SDK gebruiken om een service te implementeren voor de certificering van PDF-documenten. De volgende verbindingen zouden nuttig zijn om uw eigen bundel te schrijven OSGi

* [Uw eerste OSGi-bundel maken](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Documentservice-API gebruiken](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

U kunt de voorbeeldbundel ook gebruiken als onderdeel van deze zelfstudie-elementen.

>[!NOTE]
>
>De voorbeeldbundel gebruikt de alias &quot;ares&quot; om de documenten te certificeren. Zorg er dus voor dat uw alias &quot;ares&quot; wordt genoemd wanneer u deze bundel gebruikt

## Het voorbeeld op uw lokale systeem testen

* [Custom Document Services Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) downloaden en installeren
* [Developing with Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) downloaden en installeren
* [Controleer of u de volgende vermelding hebt toegevoegd aan de Apache Sling Service User Mapper Service](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** services zoals weergegeven in de onderstaande schermafbeelding
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Voorbeeld van adaptief formulier importeren](assets/certify-pdf-af.zip)
* [Aangepast verzenden importeren en installeren](assets/custom-submit-certify.zip)
* [Het adaptieve formulier openen](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* PDF-document uploaden dat moet worden gecertificeerd
   **optioneel**  - Geef het handtekeningveld op dat u wilt gebruiken voor de certificering van het document
* Klik op Verzenden.
* Gecertificeerde PDF moet naar u worden teruggestuurd.


