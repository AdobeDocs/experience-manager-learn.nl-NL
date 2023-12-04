---
title: Inclusief jars van derden
description: Leer jar-bestand van derden te gebruiken in uw AEM-project
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 78
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '269'
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

Als uw bundel OSGi op uw dossiersysteem is, creeer een omslag genoemd **localjar** onder de basisfolder van uw project (C:\aemformsbundles\AEMFormsProcessStep\localjar) zal het gebiedsdeel ongeveer als dit kijken

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

* Open de **filter.xml** in de map C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault van uw project Noteer het hoofdkenmerk van het filterelement.

* De volgende mapstructuur C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install maken
* De **apps/AEMFormsProcessStep-vendor-packages** is de waarde van het wortelattribuut in filter.xml
* Werk de gebiedsdeelsectie van POM.xml van het project bij
* Opdrachtprompt openen. Navigeer in mijn geval naar de map van uw project (c:\aemformsbundles\AEMFormsProcessStep). Voer het volgende bevel uit

```java
mvn clean install -PautoInstallSinglePackage
```

Als alles goed gaat, wordt het pakket samen met de bundel van derden in uw AEM geïnstalleerd. U kunt op de bundel controleren gebruikend [felix-webconsole](http://localhost:4502/system/console/bundles). De bundel van derden is beschikbaar in de map /apps van het dialoogvenster `crx` opslagplaats zoals hieronder weergegeven
![derde](assets/custom-bundle1.png)
