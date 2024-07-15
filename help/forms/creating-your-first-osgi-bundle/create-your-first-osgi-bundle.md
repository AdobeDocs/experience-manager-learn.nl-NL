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
duration: 145
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 0%

---

# Maak uw eerste OSGi-bundel

Een bundel OSGi is een Java™ archiefdossier dat code Java, middelen, en manifest bevat die de bundel en zijn gebiedsdelen beschrijft. De bundel is de eenheid van plaatsing voor een toepassing. Dit artikel is bedoeld voor ontwikkelaars die OSGi-service of een servlet willen maken met AEM Forms 6.4 of 6.5. Ga als volgt te werk om uw eerste OSGi-bundel te maken:


## JDK installeren

De ondersteunde versie van JDK installeren. Ik heb JDK1.8 gebruikt. Zorg ervoor u **JAVA_HOME** in uw milieuvariabelen hebt toegevoegd en aan de wortelomslag van uw installatie richt JDK.
De %JAVA_HOME%/bin toevoegen aan het pad

![ gegeven-bron ](assets/java-home.JPG)

>[!NOTE]
> Gebruik JDK 15 niet. Dit wordt niet ondersteund door AEM.

### Uw JDK-versie testen

Open een nieuw opdrachtpromptvenster en typ: `java -version` . De JDK-versie die wordt aangeduid door de variabele `JAVA_HOME` , moet worden hersteld

![ gegeven-bron ](assets/java-version.JPG)

## Gemaakt installeren

Maven is een tool voor automatisering van build die voornamelijk wordt gebruikt voor Java-projecten. Voer de volgende stappen uit om te installeren op uw lokale systeem.

* Maak een map met de naam `maven` in station C
* Download het [ binaire zip archief ](https://maven.apache.org/download.cgi)
* De inhoud van het ZIP-archief extraheren naar `c:\maven`
* Maak een omgevingsvariabele met de naam `M2_HOME` met de waarde `C:\maven\apache-maven-3.6.0` . In mijn geval, is de **mvn** versie 3.6.0. Op het moment dat dit artikel wordt geschreven, is de nieuwste versie 3.6.3
* Voeg de `%M2_HOME%\bin` toe aan het pad
* Uw wijzigingen opslaan
* Open een nieuwe opdrachtprompt en typ in `mvn -version` . U zou de **mvn** versie moeten zien die zoals hieronder getoond in het schermschot wordt vermeld

![ gegeven-bron ](assets/mvn-version.JPG)


## Eclipse installeren

Installeer de recentste versie van [ eclipse ](https://www.eclipse.org/downloads/)

## Uw eerste project maken

Archetype is een Maven project sjabloonkit. Een archetype wordt gedefinieerd als een origineel patroon of model waaruit alle andere dingen van dezelfde soort zijn gemaakt. De naam past bij de manier waarop we proberen een systeem te bieden dat een consistente manier biedt om Maven-projecten te genereren. Archetype helpt auteurs Maven projectmalplaatjes voor gebruikers tot stand brengen, en voorziet gebruikers van de middelen om geparameterized versies van die projectmalplaatjes te produceren.
Voer de volgende stappen uit om uw eerste gemaakte project te maken:

* Maak een nieuwe map met de naam `aemformsbundles` in station C
* Een opdrachtprompt openen en naar `c:\aemformsbundles` navigeren
* Voer het volgende bevel in uw bevelherinnering in werking

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

Na succesvolle voltooiing zou u een bericht van het bouwstijlsucces in uw bevelvenster moeten zien

## Overschrijvingsproject maken van uw in kaart gebrachte project

* De werkmap wijzigen in `mysite`
* Voer `mvn eclipse:eclipse` uit vanaf de opdrachtregel. De opdracht leest uw pombestand en maakt Eclipse-projecten met de juiste metagegevens, zodat Eclipse de projecttypen, relaties, klassenpad enzovoort begrijpt.

## Het project in een ovaal importeren

**Verduistering van de lancering**

Ga naar **Dossier -> de Invoer** en selecteer **Bestaande Gemaakte Projecten** zoals hier getoond

![ gegeven-bron ](assets/import-mvn-project.JPG)

Klik op Volgende

Selecteer c:\aemformsbundles\mysite door **te klikken doorbladert** knoop

![ gegeven-bron ](assets/mysite-eclipse-project.png)

>[!NOTE]
>U kunt desgewenst de geschikte modules importeren. Selecteer en importeer alleen de kernmodule als u alleen Java-code gaat maken in uw project.

Klik **Afwerking** om het de invoerproces te beginnen

Het project wordt geïmporteerd in Eclipse en u ziet een aantal `mysite.xxxx` mappen

Vouw `src/main/java` onder de map `mysite.core` uit. Dit is de map waarin u het grootste deel van uw code schrijft.

![ gegeven-bron ](assets/mysite-core-project.png)

## AEMFD Client SDK opnemen

U moet de SDK van de AEMFD-client in uw project opnemen om te kunnen profiteren van verschillende services die bij AEM Forms worden geleverd. Gelieve te verwijzen [ AEMFD de Cliënt SDK van SDK ](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) om de aangewezen cliënt SDK in uw Gemaakt project te omvatten. U moet de AEM FD Client SDK opnemen in de sectie voor afhankelijkheden van `pom.xml` van het kernproject, zoals hieronder wordt weergegeven.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Voer de volgende stappen uit om uw project te maken:

* Open **bevel snel venster**
* Navigeren naar `c:\aemformsbundles\mysite\core`
* De opdracht uitvoeren `mvn clean install -PautoInstallBundle`
De bovenstaande opdracht bouwt en installeert de bundel op de AEM server waarop `http://localhost:4502` wordt uitgevoerd. De bundel is ook beschikbaar in het bestandssysteem op
  `C:\AEMFormsBundles\mysite\core\target` en kan worden opgesteld gebruikend [ het Webconsole van Felix ](http://localhost:4502/system/console/bundles)

## Volgende stappen

[OSGi-service maken](./create-osgi-service.md)

