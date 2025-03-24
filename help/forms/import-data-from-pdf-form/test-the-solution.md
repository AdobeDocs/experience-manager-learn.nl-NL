---
title: Gegevens uit een PDF-bestand importeren in adaptief formulier
description: Zelfstudie om een adaptief formulier te vullen door een PDF-bestand te importeren
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 21
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# De voorbeeldelementen implementeren

U kunt de voorbeeldmiddelen gebruiken om deze oplossing te laten werken aan uw lokale AEM Forms-exemplaar

* [De clientbibliotheek en de aangepaste component importeren om het PDF-formulier te uploaden via Package Manager](./assets/client-libs-custom-component.zip)
* Download en stel de bundel op gebruikend de Bundel van de Diensten van het Document van het Document van de Douane van de OSGi- Webconsole ](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)[
* Download en stel de bundel op gebruikend de OSGi Webconsole [ Ontwikkelend met de Bundel van de Gebruiker van de Dienst ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* De download en stelt de bundel op gebruikend de OSGi Webconsole [ invoergegevens van pdf- dossier ](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* Voeg de ingang _**DevelopingWithServiceUser.core toe:getresourceresolver=data**_ in de _**Apache Sling Service User Mapper Service**_ OSGi configuratieconsole
