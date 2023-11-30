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
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# Foutopsporing op afstand van de AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

Met de lokale QuickStart van de AEM SDK kunt u externe Java-foutopsporing vanaf uw IDE gebruiken, zodat u de uitvoering van live code in AEM kunt doorlopen om precies te weten wat de exacte uitvoeringsstroom is.

Als u een extern foutopsporingsprogramma wilt aansluiten op AEM, moet de lokale QuickStart van de AEM-SDK zijn gestart met specifieke parameters (`-agentlib:...`) waarmee de IDE er verbinding mee kan maken.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK ondersteunt alleen Java 11
+ `address` geeft de poort aan die AEM luistert naar externe foutopsporingsverbindingen en kan worden gewijzigd in elke beschikbare poort op de lokale ontwikkelcomputer.
+ De laatste parameter (bijvoorbeeld `aem-author-p4502.jar`) is de AEM SKD Quickstart Jar. Dit kan de AEM Auteur-service zijn (`aem-author-p4502.jar`) of de AEM-publicatieservice (`aem-publish-p4503.jar`).


## IDE-installatie-instructies

De meeste Java IDE&#39;s bieden ondersteuning voor foutopsporing op afstand van Java-programma&#39;s, maar de exacte instellingsstappen van elke IDE variëren. Gelieve te herzien de de opstellingsinstructies van de verre debugging van uw winde voor de nauwkeurige stappen. Typisch vereisen de configuraties van winde:

+ De lokale quickstart van de host AEM SDK luistert aan, namelijk `localhost`.
+ De lokale quickstart van de poort AEM SDK luistert naar een externe foutopsporingsverbinding. Dit is de poort die door de `address` parameter bij het starten van AEM lokale quickstart van SDK.
+ Af en toe, moeten het Gegrafeerde project (de Maven) die de broncode aan ver verstrekken zuivert worden gespecificeerd; dit is uw OSGi bundelmaven project(en).

### Instructies instellen

+ [VS Code Java Remote Debugger instellen](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA Remote Debugger instellen](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Installatie van Eclipse Remote Debugger](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
