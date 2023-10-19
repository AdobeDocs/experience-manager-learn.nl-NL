---
title: Gegevens uit een PDF-bestand importeren in adaptief formulier
description: Zelfstudie om een adaptief formulier te vullen door een PDF-bestand te importeren
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# De voorbeeldelementen implementeren

U kunt de voorbeeldmiddelen gebruiken om deze oplossing te laten werken aan uw lokale AEM Forms-exemplaar

* [De clientbibliotheek en de aangepaste component importeren om het PDF-formulier te uploaden via Package Manager](./assets/client-libs-custom-component.zip)
* Download en implementeer de bundel met de OSGi-webconsole[Pakket voor aangepaste documentservices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Download en implementeer de bundel met de OSGi-webconsole [Ontwikkelen met de Bundel van de Gebruiker van de Dienst](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Download en implementeer de bundel met de OSGi-webconsole[gegevens importeren uit PDF-bestand](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* De vermelding toevoegen _**DevelopingWithServiceUser.core:getresourceresolver=data**_ in de _**Apache Sling Service User Mapper Service**_ OSGi-configuratieconsole