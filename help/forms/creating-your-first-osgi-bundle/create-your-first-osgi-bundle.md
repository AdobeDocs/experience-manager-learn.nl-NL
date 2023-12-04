---
title: Uw eerste OSGi-bundel maken met AEM Forms
description: Bouw uw eerste bundel OSGi gebruikend Maven en Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
duration: 199
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 0%

---

# Maak uw eerste OSGi-bundel

Een bundel OSGi is een Java™ archiefdossier dat code Java, middelen, en manifest bevat die de bundel en zijn gebiedsdelen beschrijft. De bundel is de eenheid van plaatsing voor een toepassing. Dit artikel is bedoeld voor ontwikkelaars die OSGi-service of een servlet willen maken met AEM Forms 6.4 of 6.5. Ga als volgt te werk om uw eerste OSGi-bundel te maken:


## JDK installeren

De ondersteunde versie van JDK installeren. Ik heb JDK1.8 gebruikt. Zorg ervoor dat u hebt toegevoegd **JAVA_HOME** in uw milieuvariabelen en wijst naar de wortelomslag van uw installatie JDK.
De %JAVA_HOME%/bin toevoegen aan het pad

![gegevensbron](assets/java-home.JPG)

>[!NOTE]
> Gebruik JDK 15 niet. Dit wordt niet ondersteund door AEM.

### Uw JDK-versie testen

Open een nieuw opdrachtpromptvenster en typ: `java -version`. U moet de JDK-versie die door de `JAVA_HOME` variabel

![gegevensbron](assets/java-version.JPG)

## Gemaakt installeren

Maven is een tool voor automatisering van build die voornamelijk wordt gebruikt voor Java-projecten. Voer de volgende stappen uit om te installeren op uw lokale systeem.

* Een map maken met de naam `maven` in uw C-station
* Download de [binair ZIP-archief](https://maven.apache.org/download.cgi)
* De inhoud van het ZIP-archief extraheren naar `c:\maven`
* Een omgevingsvariabele maken met de naam `M2_HOME` met een waarde van `C:\maven\apache-maven-3.6.0`. In mijn geval **mvn** versie is 3.6.0. Op het moment dat dit artikel wordt geschreven, is de nieuwste versie 3.6.3
* Voeg de `%M2_HOME%\bin` naar uw pad
* Uw wijzigingen opslaan
* Open een nieuwe opdrachtprompt en typ in `mvn -version`. U moet de **mvn** versie weergegeven zoals weergegeven in onderstaande schermafbeelding

![gegevensbron](assets/mvn-version.JPG)


## Eclipse installeren

De nieuwste versie van [verduisteren](https://www.eclipse.org/downloads/)

## Uw eerste project maken

Archetype is een Maven project sjabloonkit. Een archetype wordt gedefinieerd als een origineel patroon of model waaruit alle andere dingen van dezelfde soort zijn gemaakt. De naam past bij de manier waarop we proberen een systeem te bieden dat een consistente manier biedt om Maven-projecten te genereren. Archetype helpt auteurs Maven projectmalplaatjes voor gebruikers tot stand brengen, en voorziet gebruikers van de middelen om geparameterized versies van die projectmalplaatjes te produceren.
Voer de volgende stappen uit om uw eerste gemaakte project te maken:

* Een nieuwe map maken met de naam `aemformsbundles` in uw C-station
* Een opdrachtprompt openen en naar `c:\aemformsbundles`
* Voer het volgende bevel in uw bevelherinnering in werking

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

Na succesvolle voltooiing zou u een bericht van het bouwstijlsucces in uw bevelvenster moeten zien

## Overschrijvingsproject maken van uw in kaart gebrachte project

* Wijzig uw werkmap in `mysite`
* Uitvoeren `mvn eclipse:eclipse` vanaf de opdrachtregel. De opdracht leest uw pombestand en maakt Eclipse-projecten met de juiste metagegevens, zodat Eclipse de projecttypen, relaties, klassenpad enzovoort begrijpt.

## Het project in een ovaal importeren

Starten **Eclipse**

Ga naar **Bestand -> Importeren** en selecteert u **Bestaande Maven Projecten** zoals hier getoond

![gegevensbron](assets/import-mvn-project.JPG)

Klik op Volgende

Selecteer de site C:\aemformsbundles\mijndoor op de knop **Bladeren** knop

![gegevensbron](assets/mysite-eclipse-project.png)

>[!NOTE]
>U kunt desgewenst de geschikte modules importeren. Selecteer en importeer alleen de kernmodule als u alleen Java-code gaat maken in uw project.

Klikken **Voltooien** om het importproces te starten

Project wordt geïmporteerd in Eclipse en er wordt een aantal `mysite.xxxx` mappen

Breid uit `src/main/java` onder de `mysite.core` map. Dit is de map waarin u het grootste deel van uw code schrijft.

![gegevensbron](assets/mysite-core-project.png)

## AEMFD Client SDK opnemen

U moet de SDK van de AEMFD-client in uw project opnemen om te kunnen profiteren van verschillende services die bij AEM Forms worden geleverd. Zie [AEMFD Client SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) om de aangewezen cliënt SDK in uw Geweven project op te nemen. U moet de AEM FD Client SDK opnemen in de sectie Afhankelijkheden van `pom.xml` van het kernproject, zoals hieronder aangegeven.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Voer de volgende stappen uit om uw project te maken:

* Openen **opdrachtpromptvenster**
* Navigeren naar `c:\aemformsbundles\mysite\core`
* De opdracht uitvoeren `mvn clean install -PautoInstallBundle`
Bovenstaande opdracht bouwt en installeert de bundel op de AEM server waarop `http://localhost:4502`. De bundel is ook beschikbaar in het bestandssysteem op
  `C:\AEMFormsBundles\mysite\core\target` en kan worden ingezet met behulp van [Felix-webconsole](http://localhost:4502/system/console/bundles)

## Volgende stappen

[OSGi-service maken](./create-osgi-service.md)

