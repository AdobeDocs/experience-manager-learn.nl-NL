---
title: Cloudserviceproject bijwerken met het nieuwste archetype
description: AEM Forms Cloud Service-project bijwerken met nieuwste archetype
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 0%

---

# Migreren van oud aem archetype

Als u uw bestaande AEM Forms-project wilt bijwerken met het nieuwste maven archetype, moet u uw code/configuraties enz. handmatig kopiëren, van het oude project naar het nieuwe project.

De volgende stappen werden gevolgd om het gemaakte project te migreren gebruikend archetype 30 aan archetype 33 project

## Gemaakt project maken met het nieuwste archetype

* Opdrachtprompt openen en naar c:\cloudmanager navigeren
* Gemaakt project maken met het nieuwste archetype.
* Kopieer en plak de inhoud van de [tekstbestand](assets/creating-maven-project.txt) in uw opdrachtpromptvenster. Afhankelijk van het [nieuwste versie](https://github.com/adobe/aem-project-archetype/releases). Archetype 33 bevat nieuwe AEM Forms-thema&#39;s.
Aangezien we het nieuwe gefabriceerde project maken in de cloudmanager-map die al een aem-banking-applicatieproject bevat, moet u het **DartifactId** van een &#39;aem-banking&#39;-toepassing naar iets anders. Ik heb voor dit artikel gebruik gemaakt van aem-banking-application1.

>[!NOTE]
>
>Als u dit nieuwe project zoals is de instantie van de wolkendienst opstelt zal niet HandleFormSubmission en SubmitToAEMServlet hebben. Dit komt omdat elke keer dat u een project implementeert met gebruik van cloudbeheer, alles in de map apps wordt verwijderd en overschreven.

## Java-code kopiëren

Zodra uw project met succes wordt gecreeerd, kunt u beginnen code/configuraties etc., van oud project aan dit nieuwe project te kopiëren

* Kopieer de HandleFormSubmission servlet van ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
tot

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Kopieer de CustomSubmit vanuit
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` van een elektronisch-bancaire toepassing tot een aem-banking-application1-project

* het nieuwe project importeren in IntelliJ

* Werk filter.xml in de module ui.apps van het aem-bank-application1 project bij om de volgende lijn te omvatten
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Nadat u alle code naar uw nieuwe project hebt gekopieerd, kunt u dit project naar de manager van de wolk duwen.

>[!NOTE]
>
>Om de inhoud (Aangepast Forms, Model van de Gegevens van de Vorm, enz.,) in uw nieuw project te synchroniseren zult u de aangewezen omslagstructuur in uw project moeten creëren IntelliJ en dan uw project IntelliJ met uw AEM instantie synchroniseren gebruikend het krijgen bevel van het repo hulpmiddel.
