---
title: Inclusief jars van derden
description: Leer JAR-bestand van derden te gebruiken in uw AEM-project
version: Experience Manager 6.4, Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 53
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Bundels van derden opnemen in uw AEM-project

In dit artikel zullen wij door de stappen betrokken bij het omvatten van de bundel van derde partij OSGi in uw project van AEM lopen.Voor dit artikel gaan wij [ jsch-0.1.55.jar ](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) in ons project van AEM omvatten.  ALS OSGi in een bepaalde bewaarplaats beschikbaar is omvat de gebiedsafhankelijkheid van de bundel in het POM.xml- dossier van het project.

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

Als uw bundel OSGi op uw dossiersysteem is, creeer een omslag genoemd **localjar** onder de basisfolder van uw project (C:\aemformsbundles\AEMFormsProcessStep\localjar) het gebiedsdeel zal ongeveer als dit kijken

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

Wij voegen deze bundel aan ons AEM project **AEMFormsProcessStep** toe die in **c verblijft:\ aemformsbundles** omslag

* Open **filter.xml** van de C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault omslag van uw project
Noteer het basiskenmerk van het filterelement.

* De volgende mapstructuur C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install maken
* **apps/AEMFormsProcessStep-verkoper-pakketten** is de waarde van wortelattributen in filter.xml
* Werk de gebiedsdeelsectie van POM.xml van het project bij
* Opdrachtprompt openen. Navigeer in mijn geval naar de map van uw project (c:\aemformsbundles\AEMFormsProcessStep). Voer het volgende bevel uit

```java
mvn clean install -PautoInstallSinglePackage
```

Als alles goed gaat, wordt het pakket samen met de bundel van derden in uw AEM-instantie geïnstalleerd. U kunt voor de bundel controleren gebruikend [ felix Webconsole ](http://localhost:4502/system/console/bundles). De bundel van derden is beschikbaar in de map /apps van de `crx` -opslagplaats, zoals hieronder wordt weergegeven
![ derde ](assets/custom-bundle1.png)
