---
title: Foutopsporing op afstand van de AEM SDK
description: Met de lokale QuickStart van de AEM SDK kunt u externe Java-foutopsporing vanaf uw IDE gebruiken, zodat u de uitvoering van live code in AEM kunt doorlopen om precies te weten wat de exacte uitvoeringsstroom is.
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Foutopsporing op afstand van de AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

Met de lokale QuickStart van de AEM SDK kunt u externe Java-foutopsporing vanaf uw IDE gebruiken, zodat u de uitvoering van live code in AEM kunt doorlopen om precies te weten wat de exacte uitvoeringsstroom is.

Om verre debugger aan AEM te verbinden, moet lokale QuickStart van AEM SDK met specifieke parameters (`-agentlib:...`) begonnen zijn toestaand winde om met het te verbinden.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK ondersteunt alleen Java 11
+ `address` geeft de poort aan AEM luistert naar externe foutopsporingsverbindingen en kan worden gewijzigd in een willekeurige beschikbare poort op de lokale ontwikkelcomputer.
+ De laatste parameter (bijvoorbeeld `aem-author-p4502.jar`) is de AEM SKD Quickstart Jar. Dit kan of de dienst van de Auteur van de AEM (`aem-author-p4502.jar`) of de dienst van AEM Publish (`aem-publish-p4503.jar`) zijn.


## IDE-installatie-instructies

De meeste Java IDE&#39;s bieden ondersteuning voor foutopsporing op afstand van Java-programma&#39;s, maar de exacte instellingsstappen van elke IDE variëren. Gelieve te herzien de de opstellingsinstructies van de verre debugging van uw winde voor de nauwkeurige stappen. Typisch vereisen de configuraties van winde:

+ De lokale quickstart van de host AEM SDK luistert, namelijk `localhost` .
+ De lokale quickstart van de poort AEM SDK luistert naar een externe foutopsporingsverbinding. Dit is de poort die door de parameter `address` wordt opgegeven bij het starten AEM de lokale quickstart van SDK.
+ Af en toe, moeten het Gegrafeerde project (de Maven) die de broncode aan ver verstrekken zuivert worden gespecificeerd; dit is uw OSGi bundelmaven project(en).

### Instructies instellen

+ [&#x200B; de Verre debugger opstelling van Java van de Code Java &#x200B;](https://code.visualstudio.com/docs/java/java-debugging)
+ [&#x200B; IntelliJ IDEA Verre debugger opstelling &#x200B;](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [&#x200B; Verre debugger opstelling van de Verduistering &#x200B;](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
