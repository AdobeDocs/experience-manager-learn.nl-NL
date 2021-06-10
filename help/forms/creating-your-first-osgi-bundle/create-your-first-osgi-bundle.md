---
title: Uw eerste OSGi-bundel maken met AEM Forms
description: Bouw uw eerste bundel OSGi gebruikend Maven en Eclipse
version: 6.4,6.5
feature: Adaptieve Forms
topic: Ontwikkeling
role: Developer
level: Beginner
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---


# Maak uw eerste OSGi-bundel

Een bundel OSGi is een Java™ archiefdossier dat code Java, middelen, en manifest bevat die de bundel en zijn gebiedsdelen beschrijft. De bundel is de eenheid van plaatsing voor een toepassing. Dit artikel is bedoeld voor ontwikkelaars die OSGi-service of een servlet willen maken met AEM Forms 6.4 of 6.5. Ga als volgt te werk om uw eerste OSGi-bundel te maken:


## JDK installeren

Installeer de ondersteunde versie van JDK. Ik heb JDK1.8 gebruikt. Zorg ervoor u **JAVA_HOME** in uw milieuvariabelen hebt toegevoegd en aan de wortelomslag van uw installatie richt JDK.
De %JAVA_HOME%/bin toevoegen aan het pad

![gegevensbron](assets/java-home.JPG)

>[!NOTE]
> Gebruik JDK 15 niet. Dit wordt niet ondersteund door AEM.

### Uw JDK-versie testen

Open een nieuw opdrachtpromptvenster en typ: `java -version`. U zou de versie van JDK moeten terugkrijgen die door `JAVA_HOME` variabele wordt geïdentificeerd

![gegevensbron](assets/java-version.JPG)

## Gemaakt installeren

Maven is een tool voor automatisering van build die voornamelijk wordt gebruikt voor Java-projecten. Voer de volgende stappen uit om te installeren op uw lokale systeem.

* Creeer een omslag genoemd `maven` in uw aandrijving van C
* Download het [binaire ZIP-archief](http://maven.apache.org/download.cgi)
* De inhoud van het ZIP-archief extraheren naar `c:\maven`
* Maak een omgevingsvariabele met de naam `M2_HOME` met de waarde `C:\maven\apache-maven-3.6.0`. In mijn geval is de **mvn** versie 3.6.0. Op het moment dat dit artikel wordt geschreven, is de nieuwste versie 3.6.3
* `%M2_HOME%\bin` toevoegen aan het pad
* Uw wijzigingen opslaan
* Open een nieuwe bevelherinnering en typ in `mvn -version`. U zou **mvn** versie moeten zien zoals aangetoond in het hieronder ontsproten scherm

![gegevensbron](assets/mvn-version.JPG)

## Settings.xml

Met een Maven `settings.xml`-bestand worden waarden gedefinieerd die uitvoering via Maven op verschillende manieren configureren. Meestal wordt het gebruikt om een lokale opslaglocatie, alternatieve externe opslagservers en verificatiegegevens voor privéopslagruimten te definiëren.

Naar `C:\Users\<username>\.m2 folder` navigeren
Pak de inhoud van het bestand [settings.zip](assets/settings.zip) uit en plaats dit in de map `.m2`.

## Verduistering installeren

De nieuwste versie van [eclipse](https://www.eclipse.org/downloads/) installeren

## Uw eerste project maken

Archetype is een Maven project sjabloonkit. Een archetype wordt gedefinieerd als een origineel patroon of model waaruit alle andere dingen van dezelfde soort zijn gemaakt. De naam past bij de manier waarop we proberen een systeem te bieden dat een consistente manier biedt om Maven-projecten te genereren. Archetype zal auteurs helpen Maven projectmalplaatjes voor gebruikers tot stand brengen, en voorziet gebruikers van de middelen om geparameterized versies van die projectmalplaatjes te produceren.
Voer de volgende stappen uit om uw eerste gemaakte project te maken:

* Creeer een nieuwe omslag genoemd `aemformsbundles` in uw aandrijving van C
* Een opdrachtprompt openen en naar `c:\aemformsbundles` navigeren
* Voer het volgende bevel in uw bevelherinnering in werking
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Het Maven project zal interactief worden geproduceerd en u zal worden gevraagd om waarden aan een aantal eigenschappen zoals te verstrekken:

| Eigenschapnaam | Significantie | Waarde |
------------------------|---------------------------------------|---------------------
| groupId | groupId identificeert uniek uw project over alle projecten | com.learningaemforms.adobe |
| appsFolderName | Naam van de map die de projectstructuur bevat | leervormen |
| artifactId | artifactId is de naam van de pot zonder versie. Als u het creeerde, dan kunt u kiezen welke naam u met kleine letters en geen vreemde symbolen wilt. | leervormen |
| version | Als u het verspreidt, kunt u elke typische versie kiezen met nummers en punten (1.0, 1.1, 1.0.1, ...). | 1.0 |

Accepteer de standaardwaarden voor de andere eigenschappen door op Enter te drukken.
Als alles goed gaat zou u een bericht van het bouwstijlsucces in uw bevelvenster moeten zien

## Overschrijvingsproject maken van uw in kaart gebrachte project

Wijzig de werkmap in `learningaemforms`.
`mvn eclipse:eclipse` uitvoeren vanaf de opdrachtregel
Het bovenstaande bevel leest uw poemdossier en leidt tot projecten Eclipse met correcte meta-gegevens zodat Eclipse projecttypes, verhoudingen, klassenpad, enz. zal begrijpen.

## Het project in een ovaal importeren

**Eclipse** starten

Ga naar **Bestand -> Importeren** en selecteer **Bestaande gefinancierde projecten** zoals hier wordt getoond

![gegevensbron](assets/import-mvn-project.JPG)

Klik op Next

Selecteer `c:\aemformsbundles\learningaemform`s door op de knop **Bladeren** te klikken

![gegevensbron](assets/select-mvn-project.JPG)

>[!NOTE]
>U kunt desgewenst de geschikte modules importeren. Selecteer en importeer alleen de kernmodule als u alleen Java-code gaat maken in uw project.

Klik **Voltooien** om het importproces te starten

Project wordt geïmporteerd in Eclipse en er wordt een aantal `learningaemforms.xxxx` mappen weergegeven

Vouw `src/main/java` onder de map `learningaemforms.core` uit. Dit is de map waarin u het grootste deel van uw code gaat schrijven.

![gegevensbron](assets/learning-core.JPG)

## Uw project samenstellen




Zodra u uw dienst OSGi, of servlet hebt geschreven, zult u uw project moeten bouwen om de bundel te produceren OSGi die kan worden opgesteld gebruikend de het Webconsole van Felix. Raadpleeg [AEMFD Client SDK](https://repo.adobe.com/nexus/content/groups/public/com/adobe/aemfd/aemfd-client-sdk/) om de juiste client SDK op te nemen in uw Maven-project. U moet de AEM FD Client SDK opnemen in de sectie voor afhankelijkheden van `pom.xml` van het kernproject, zoals hieronder wordt weergegeven.





```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Voer de volgende stappen uit om uw project te maken:

* **opdrachtpromptvenster openen**
* Ga naar `c:\aemformsbundles\learningaemforms\core`
* De opdracht `mvn clean install -PautoInstallBundle` uitvoeren
De bovenstaande opdracht bouwt en installeert de bundel in de AEM server die op `http://localhost:4502` wordt uitgevoerd. De bundel is ook beschikbaar op het bestandssysteem op
   `C:\AEMFormsBundles\learningaemforms\core\target` en kan worden geïmplementeerd met  [Felix-webconsole](http://localhost:4502/system/console/bundles)
