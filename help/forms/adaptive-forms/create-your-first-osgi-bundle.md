---
title: Uw eerste OSGi-bundel maken met AEM formulieren
description: Bouw uw eerste bundel OSGi gebruikend maven en eclipse
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
duration: 240
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '816'
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
* Download de [binair ZIP-archief](http://maven.apache.org/download.cgi)
* De inhoud van het ZIP-archief extraheren naar `c:\maven`
* Een omgevingsvariabele maken met de naam `M2_HOME` met een waarde van `C:\maven\apache-maven-3.6.0`. In mijn geval **mvn** versie is 3.6.0. Op het moment dat dit artikel wordt geschreven, is de nieuwste versie 3.6.3
* Voeg de `%M2_HOME%\bin` naar uw pad
* Uw wijzigingen opslaan
* Open een nieuwe opdrachtprompt en typ in `mvn -version`. U moet de **mvn** versie weergegeven zoals weergegeven in onderstaande schermafbeelding

![gegevensbron](assets/mvn-version.JPG)

## Settings.xml

A Maven `settings.xml` In dit bestand worden waarden gedefinieerd die de uitvoering van Maven op verschillende manieren configureren. Meestal wordt het gebruikt om een lokale opslaglocatie, alternatieve externe opslagservers en verificatiegegevens voor privéopslagruimten te definiëren.

Navigeren naar `C:\Users\<username>\.m2 folder`
De inhoud van [settings.zip](assets/settings.zip) en plaatst deze in het `.m2` map.

## Eclipse installeren

De nieuwste versie van [verduisteren](https://www.eclipse.org/downloads/)

## Uw eerste project maken

Archetype is een Maven project sjabloonkit. Een archetype wordt gedefinieerd als een origineel patroon of model waaruit alle andere dingen van dezelfde soort zijn gemaakt. De naam past bij de manier waarop we proberen een systeem te bieden dat een consistente manier biedt om Maven-projecten te genereren. Archetype helpt auteurs Maven projectmalplaatjes voor gebruikers tot stand brengen, en voorziet gebruikers van de middelen om geparameterized versies van die projectmalplaatjes te produceren.
Voer de volgende stappen uit om uw eerste gemaakte project te maken:

* Een nieuwe map maken met de naam `aemformsbundles` in uw C-station
* Een opdrachtprompt openen en naar `c:\aemformsbundles`
* Voer het volgende bevel in uw bevelherinnering in werking
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Het Maven project wordt op interactieve wijze geproduceerd en u wordt gevraagd om waarden aan een aantal eigenschappen zoals te verstrekken

| Eigenschapnaam | Significantie | Waarde |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId identificeert uniek uw project over alle projecten | com.learningaemforms.adobe |
| appsFolderName | Naam van de map die de projectstructuur bevat | leervormen |
| artefactId | artifactId is de naam van de pot zonder versie. Als u het creeerde, dan kunt u kiezen welke naam u met kleine letters en geen vreemde symbolen wilt. | leervormen |
| versie | Als u het verspreidt, kunt u elke typische versie kiezen met nummers en punten (1.0, 1.1, 1.0.1, ...). | 1,0 |

Accepteer de standaardwaarden voor de andere eigenschappen door op Enter te drukken.
Als alles goed gaat zou u een bouwstijlsuccesbericht in uw bevelvenster moeten zien

## Overschrijvingsproject maken van uw in kaart gebrachte project

Wijzig uw werkmap in `learningaemforms`.
Uitvoeren `mvn eclipse:eclipse` vanaf de opdrachtregel Met de bovenstaande opdracht leest u uw pombestand en maakt u Eclipse-projecten met de juiste metagegevens, zodat Eclipse de projecttypen, relaties, klassenpad enzovoort begrijpt.

## Het project in een ovaal importeren

Starten **Eclipse**

Ga naar **Bestand -> Importeren** en selecteert u **Bestaande Maven Projecten** zoals hier getoond

![gegevensbron](assets/import-mvn-project.JPG)

Klik op Volgende

Selecteer de `c:\aemformsbundles\learningaemform`s door op de knop **Bladeren** knop

![gegevensbron](assets/select-mvn-project.JPG)

>[!NOTE]
>U kunt desgewenst de geschikte modules importeren. Selecteer en importeer alleen de kernmodule als u alleen Java-code gaat maken in uw project.

Klikken **Voltooien** om het importproces te starten

Project wordt geïmporteerd in Eclipse en er wordt een aantal `learningaemforms.xxxx` mappen

Breid uit `src/main/java` onder de `learningaemforms.core` map. Dit is de map waarin u het grootste deel van uw code schrijft.

![gegevensbron](assets/learning-core.JPG)

## Uw project samenstellen

Zodra u uw dienst OSGi, of servlet hebt geschreven, moet u uw project bouwen om de bundel te produceren OSGi die kan worden opgesteld gebruikend de het Webconsole van Felix. Zie [AEMFD Client SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) om de aangewezen cliënt SDK in uw Geweven project op te nemen. U moet de AEM FD Client SDK opnemen in de sectie voor afhankelijkheden van `pom.xml` van het kernproject, zoals hieronder aangegeven.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Voer de volgende stappen uit om uw project te maken:

* Openen **opdrachtpromptvenster**
* Navigeren naar `c:\aemformsbundles\learningaemforms\core`
* De opdracht uitvoeren `mvn clean install`
Als alles goed gaat, moet u de bundel op de volgende locatie zien `C:\AEMFormsBundles\learningaemforms\core\target`. Deze bundel kan nu worden geïmplementeerd in AEM met de Felix-webconsole.
