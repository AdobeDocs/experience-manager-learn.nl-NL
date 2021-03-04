---
title: Foutopsporing op afstand van de AEM SDK
description: Met de lokale QuickStart van de AEM SDK kunt u externe Java-foutopsporing vanaf uw IDE gebruiken, zodat u de uitvoering van live code in AEM kunt doorlopen om precies te weten wat de exacte uitvoeringsstroom is.
feature: Gereedschappen voor ontwikkelaars
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Ontwikkeling
role: Developer
level: Begin, tussenliggend
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# Foutopsporing op afstand van de AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

Met de lokale QuickStart van de AEM SDK kunt u externe Java-foutopsporing vanaf uw IDE gebruiken, zodat u de uitvoering van live code in AEM kunt doorlopen om precies te weten wat de exacte uitvoeringsstroom is.

Om een verre debugger aan AEM te verbinden, moet de lokale QuickStart van AEM SDK met specifieke parameters (`-agentlib:...`) begonnen zijn toestaand winde om met het te verbinden.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` geeft de poort aan die AEM luistert naar externe foutopsporingsverbindingen en kan worden gewijzigd in elke beschikbare poort op de lokale ontwikkelcomputer.
+ De laatste parameter (bijv. `aem-author-p4502.jar`) is de AEM SKD Quickstart Jar. Dit kan ofwel de AEM-auteurservice (`aem-author-p4502.jar`) zijn, ofwel de AEM-publicatieservice (`aem-publish-p4503.jar`).

## IDE-installatie-instructies

De meeste Java IDE&#39;s bieden ondersteuning voor foutopsporing op afstand van Java-programma&#39;s, maar de exacte instellingsstappen van elke IDE variëren. Gelieve te herzien de de opstellingsinstructies van de verre debugging van uw winde voor de nauwkeurige stappen. Typisch vereisen de configuraties van winde:

+ De lokale quickstart van de host AEM SDK luistert, namelijk `localhost`.
+ De lokale quickstart van de poort AEM SDK luistert naar een externe foutopsporingsverbinding. Dit is de poort die wordt opgegeven door de parameter `address` bij het starten van AEM lokale quickstart van SDK.
+ Af en toe, moeten het Maven project (de Maven) die de broncode aan ver verstrekken zuiveren worden gespecificeerd; Dit zijn uw OSGi bundelgemaakte projecten.

### Instructies instellen

+ [VS Code Java Remote Debugger instellen](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA Remote Debugger instellen](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Installatie van Eclipse Remote Debugger](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
