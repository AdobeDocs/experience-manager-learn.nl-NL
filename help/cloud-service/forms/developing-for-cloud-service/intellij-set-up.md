---
title: IntelliJ Community Edition installeren
description: Installeer en importeer het AEM-project in IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
duration: 43
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---

# IntelliJ installeren

Installeer [ IntelliJ communautaire uitgave ](https://www.jetbrains.com/idea/download/#section=windows). U kunt de standaardinstellingen accepteren terwijl u dit tijdens de installatie aanbeveelt.

## AEM-project importeren

* Start IntelliJ
* Importeer het AEM-project dat u in de vorige stap hebt gemaakt. Nadat het project wordt ingevoerd zou uw scherm iets als dit ![ a-bank-app ](assets/aem-banking-app.png) moeten kijken. U zult typisch met kern,ui.apps, ui.config en ui.content subprojecten werken.
* Als u het gemaakte en terminalvenster niet ziet, gaat u naar view->Tools Window en selecteert u Maven en Terminal

## De module Lettertypen toevoegen

Als u aangepaste lettertypen in uw PDF-bestand wilt gebruiken, moet u de aangepaste lettertypen naar de AEM Forms CS-instantie duwen. Voer de volgende stappen uit

* Creeer een omslag genoemd **doopvonten** in C:\CloudManager\aem-banking-application
* Extraheer de inhoud van [ font.zip ](assets/fonts.zip) in de pas gecreëerde doopvontenomslag
* In de module Lettertypen bevat een aantal aangepaste lettertypen. U kunt aangepaste lettertypen van uw organisatie toevoegen aan de map C:\CloudManager\aem-banking-application\fonts\src\main\resources van de module Lettertypen
* Het bestand C:\CloudManager\aem-banking-application\pom.xml openen
* Voeg de volgende regel ```<module>fonts</module>``` toe in de sectie Modules van de pom.xml
* Uw pom.xml opslaan
* Vernieuw het aem-bank-toepassingsproject in IntelliJ

Projectstructuur met de module Lettertypen
![ doopvonten-module ](assets/fonts-module.png)

De module Fonts die is opgenomen in de POM Projecten
![ doopvonten-pom ](assets/fonts-module-pom.png)

## Volgende stappen

[Installatiegit](./setup-git.md)
