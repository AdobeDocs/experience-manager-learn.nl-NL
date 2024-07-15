---
title: Cloudserviceproject bijwerken met het nieuwste archetype
description: AEM Forms Cloud Service-project bijwerken met nieuwste archetype
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
duration: 67
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Migreren van oud aem archetype

Als u uw bestaande AEM Forms-project wilt bijwerken met het nieuwste maven archetype, moet u uw code/configuraties enz. handmatig kopiëren, van het oude project naar het nieuwe project.

De volgende stappen werden gevolgd om het gemaakte project te migreren gebruikend archetype 30 aan archetype 33 project

## Gemaakt project maken met het nieuwste archetype

* Opdrachtprompt openen en naar c:\cloudmanager navigeren
* Gemaakt project maken met het nieuwste archetype.
* Kopieer en kleef de inhoud van het [ tekstdossier ](assets/creating-maven-project.txt) in uw venster van de bevelherinnering. U kunt DarchetypeVersion=33 afhankelijk van de [ recentste versie ](https://github.com/adobe/aem-project-archetype/releases) moeten veranderen. Archetype 33 bevat nieuwe AEM Forms-thema&#39;s.
Aangezien wij het nieuwe geleide project in de cloudmanager omslag creëren die reeds a-bank-toepassing project heeft, zou u **DartifactId** van a-bank-toepassing in iets anders moeten veranderen. Ik heb voor dit artikel gebruik gemaakt van aem-banking-application1.

>[!NOTE]
>
>Als u dit nieuwe project zoals is de instantie van de cloudservice zal niet HandleFormSubmission en SubmitToAEMServlet hebben. Dit komt doordat elke keer dat u een project implementeert met Cloud Manager, alles in de map `/apps` wordt verwijderd en overschreven.

## Java-code kopiëren

Zodra uw project met succes wordt gecreeerd, kunt u beginnen code/configuraties etc., van oud project aan dit nieuwe project te kopiëren

* Kopieer het HandleFormSubmission servlet van ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
tot
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Kopieer de CustomSubmit vanuit
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` van aem-bank-application aan aem-banking-application1 project

* het nieuwe project importeren in IntelliJ

* Werk filter.xml in de module ui.apps van het aem-bank-application1 project bij om de volgende lijn te omvatten
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Nadat u alle code naar uw nieuwe project hebt gekopieerd, kunt u dit project naar de manager van de wolk duwen.

>[!NOTE]
>
>Om de inhoud (Aangepast Forms, Model van de Gegevens van de Vorm, enz.,) in uw nieuw project te synchroniseren zult u de aangewezen omslagstructuur in uw project moeten creëren IntelliJ en dan uw project IntelliJ met uw AEM instantie synchroniseren gebruikend het krijgen bevel van het repo hulpmiddel.
