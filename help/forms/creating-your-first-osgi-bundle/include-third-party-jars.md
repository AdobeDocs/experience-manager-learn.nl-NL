---
title: Uw eerste OSGi-bundel maken met AEM Forms
description: Bouw uw eerste bundel OSGi gebruikend Maven en Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: kt-11245
source-git-commit: 8944a4feaefbc4cf0db52011a0d49b22341780c0
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# Het opnemen van bundels van derden in uw AEM project

In dit artikel doorlopen we de stappen die nodig zijn om een OSGi-bundel van derden op te nemen in uw AEM-project. Voor dit artikel nemen we de [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) in ons AEM project.  ALS OSGi in een bepaalde bewaarplaats beschikbaar is omvat de gebiedsafhankelijkheid van de bundel in het POM.xml- dossier van het project.

>[!NOTE]
> Aangenomen wordt dat de derde-partijpot een OSGi-bundel is

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Als uw bundel OSGi op uw dossiersysteem is zal het gebiedsdeel iets als dit kijken

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## De mapstructuur maken

We voegen deze bundel toe aan ons AEM project **AEMFormsProcessStep** die in het **c:\aemformsbundles** map

* Open de **filter.xml** van C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter element.

* De volgende mapstructuur C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install maken
* De **apps/AEMFormsProcessStep-vendor-packages** is de waarde van het wortelattribuut in filter.xml
* Werk de gebiedsdeelsectie van POM.xml van het project bij
* Opdrachtprompt openen. Navigeer in mijn geval naar de map van uw project (c:\aemformsbundles\AEMFormsProcessStep). Voer het volgende bevel uit

```java
mvn clean install -pAutoInstallSinglePackage
```

Als alles goed gaat, wordt het pakket samen met de bundel van derden in uw AEM-instantie geïnstalleerd. U kunt op de bundel controleren gebruikend [felix-webconsole](http://localhost:4502/system/console/bundles). De bundel van derden is beschikbaar in de map /apps van het dialoogvenster `crx` opslagplaats zoals hieronder weergegeven
![derde](assets/custom-bundle1.png)



