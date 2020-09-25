---
title: Lokale AEM Runtime instellen voor AEM als ontwikkeling van Cloud Servicen
description: Opstelling Lokale AEM Runtime die de AEM als QuickStart Jar van SDK van de Cloud Service gebruikt.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 0%

---


# Lokale AEM-runtime instellen

Adobe Experience Manager (AEM) kan lokaal worden uitgevoerd met de AEM als QuickStart Jar van een Cloud Service-SDK. Dit staat ontwikkelaars toe om op te stellen aan, en douanecode, configuratie en inhoud te testen alvorens het aan broncontrole vast te leggen, en het op te stellen aan een AEM als milieu van de Cloud Service.

Merk op dat `~` als steno voor de Folder van de Gebruiker wordt gebruikt. In Windows is dit het equivalent van `%HOMEPATH%`.

>[!VIDEO](https://video.tv.adobe.com/v/32551/?quality=12&learn=on)

>[!NOTE]
>
> In deze video ziet u hoe u een lokale versie van Adobe Experience Manager binnen een paar minuten kunt installeren en uitvoeren met de lokale snelstartfunctie van de AEM SDK. In deze video wordt getoond hoe u de lokale snelstartfunctie van de AEM SDK kunt starten door te dubbelklikken op het Jar-bestand met de snelstartserver. Dit werkt echter niet in Java 8 en is geïnstalleerd op de computer. U kunt ook de lokale snelstart van de AEM SDK starten vanaf de opdrachtregel met de `java -jar ...` opdracht zoals [beschreven op deze pagina](#set-up-local-aem-author-service).

## Java installeren

Experience Manager is een Java-toepassing en daarom is ondersteuning van ontwikkelingstools vereist voor de SDK van Java.

1. [Download en installeer de nieuwste Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2fx jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Controleer of de Java 11 SDK is geïnstalleerd met de opdracht:
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Download de AEM als Cloud Service SDK

De AEM als Cloud Service SDK, of AEM SDK, bevat de Quickstart Jar die wordt gebruikt om AEM Auteur in werking te stellen en plaatselijk voor ontwikkeling, evenals de compatibele versie van de Hulpmiddelen van de Verzender te publiceren.

1. Meld u aan bij [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) met uw Adobe ID
   + Merk op dat uw organisatie van de Adobe voor AEM als Cloud Service __moet__ worden provisioned om de AEM als Cloud Service SDK te downloaden.
1. Ga naar het __AEM als tabblad Cloud Service__
1. Sorteren op __gepubliceerde datum__ in __aflopende__ volgorde
1. Klik op de meest recente __AEM SDK__ -resultaatrij
1. Controleer en accepteer de EULA en tik op de knop __Downloaden__

## Haal de QuickStart Jar uit het ZIP van de AEM SDK

1. Het gedownloade `aem-sdk-XXX.zip` bestand uitpakken
1. Zorg ervoor dat het bestand __license.properties__ voor uw Experience Manager-ontwikkelaars beschikbaar is

Let op: dezelfde QuickStart Jar- en license.properties-bestanden worden gebruikt om _zowel_ AEM Author als Publish Services te starten.

## Lokale AEM-auteurservice instellen{#set-up-local-aem-author-service}

De lokale AEM Author Service biedt ontwikkelaars een lokale ervaring met auteurs van digitale markten/content die ze delen om inhoud te maken en te beheren.  De AEM AuteurDienst wordt ontworpen zowel als creatie als voorproefmilieu, toestaand de meeste bevestigingen van eigenschapontwikkeling kunnen tegen het worden uitgevoerd, die tot het een essentieel element van het lokale ontwikkelingsproces maken.

1. De map maken `~/aem-sdk/author`
1. Kopieer het JAR __-bestand voor__ QuickStart naar `~/aem-sdk/author` en wijzig de naam van het bestand in `aem-author-p4502.jar`
1. Kopieer het bestand __license.properties__ naar  `~/aem-sdk/author`
1. Start de lokale AEM-auteurservice door het volgende uit te voeren vanaf de opdrachtregel:
   + `java -jar aem-author-p4502.jar`
      + Geef het beheerderswachtwoord op als `admin`. Om het even welk admin wachtwoord is aanvaardbaar, nochtans adviseert zijn om het gebrek voor lokale ontwikkeling te gebruiken om de behoefte te verminderen om te vormen.

   U *kunt niet* de AEM als Cloud Service Quickstart Jar beginnen [door te dubbelklikken](#troubleshooting-double-click).
1. Ga naar de lokale AEM-auteurservice op [http://localhost:4502](http://localhost:4502) in een webbrowser

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\author
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cp ../license.properties ~/aem-sdk/author
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Lokale AEM-publicatieservice instellen

De lokale AEM-publicatieservice biedt ontwikkelaars de lokale ervaring die eindgebruikers van de AEM zullen hebben, zoals bladeren door de website die op AEM wordt gehost. Een lokale AEM-publicatieservice is belangrijk omdat deze integreert met de [Dispatcher-gereedschappen](./dispatcher-tools.md) van AEM SDK en ontwikkelaars in staat stelt de ervaring van de eindgebruiker te testen en nauwkeurig af te stemmen.

1. De map maken `~/aem-sdk/publish`
1. Kopieer het JAR __-bestand voor__ QuickStart naar `~/aem-sdk/publish` en wijzig de naam van het bestand in `aem-publish-p4503.jar`
1. Kopieer het bestand __license.properties__ naar  `~/aem-sdk/publish`
1. Start de lokale AEM-publicatieservice door het volgende vanaf de opdrachtregel uit te voeren:
   + `java -jar aem-publish-p4503.jar`
      + Geef het beheerderswachtwoord op als `admin`. Om het even welk admin wachtwoord is aanvaardbaar, nochtans adviseert zijn om het gebrek voor lokale ontwikkeling te gebruiken om de behoefte te verminderen om te vormen.

   U *kunt niet* de AEM als Cloud Service Quickstart Jar beginnen [door te dubbelklikken](#troubleshooting-double-click).
1. Open de lokale AEM-publicatieservice op [http://localhost:4503](http://localhost:4503) in een webbrowser

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\publish
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cp ../license.properties ~/aem-sdk/publish
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Snelstartmodi voor Jar

De naamgeving van de QuickStart Jar `aem-<tier>_<environment>-p<port number>.jar` geeft aan hoe deze wordt gestart. Wanneer AEM zoals begonnen in een specifieke rij, auteur of publiceert, kan het niet in de afwisselende rij worden veranderd. Hiervoor moet de `crx-Quickstart` map die tijdens de eerste uitvoering wordt gegenereerd, worden verwijderd en moet QuickStart Jar opnieuw worden uitgevoerd. Het milieu en de Havens kunnen worden veranderd, nochtans vereisen zij einde/begin van de lokale AEM instantie.

Het veranderen van milieu&#39;s, `dev`, `stage` en `prod`, kan voor ontwikkelaars nuttig zijn om milieu-specifieke configuraties te verzekeren correct worden bepaald en door AEM opgelost. Aanbevolen wordt om lokale ontwikkeling vooral uit te voeren in de standaarduitvoermodus van de `dev` omgeving.

De beschikbare permutaties zijn als volgt:

+ `aem-author-p4502.jar`
   + Als Auteur in Dev loopwijze op haven 4502
+ `aem-author_dev-p4502.jar`
   + Als Auteur in Dev loopwijze op haven 4502 (het zelfde als `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + Als Auteur in Staging run mode op poort 4502
+ `aem-author_prod-p4502.jar`
   + Als Auteur in de run mode van de Productie op haven 4502
+ `aem-publish-p4503.jar`
   + Als Auteur in Dev loopwijze op haven 4503
+ `aem-publish_dev-p4503.jar`
   + Als Auteur in Dev loopwijze op haven 4503 (het zelfde als `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + Als Auteur in Staging run mode op poort 4503
+ `aem-publish_prod-p4503.jar`
   + Als auteur in de uitvoermodus Productie op poort 4503

Merk op dat het havenaantal om het even welke beschikbare haven op de lokale ontwikkelingsmachine kan zijn, echter door overeenkomst:

+ Poort __4502__ wordt gebruikt voor de __lokale AEM-auteurservice__
+ Poort __4503__ wordt gebruikt voor de __lokale AEM-publicatieservice__

Het wijzigen van deze instellingen kan aanpassingen in AEM SDK-configuraties vereisen

## Een lokale AEM-runtime stoppen

Als u een lokale AEM-runtime wilt stoppen, opent u het opdrachtregelvenster dat is gebruikt om de AEM-runtime te starten, en tikt u op `Ctrl-C`. Wacht tot AEM is afgesloten. Wanneer het sluitingsproces volledig is, zal de herinnering van de bevellijn beschikbaar zijn.

## Wanneer werkt u de QuickStart-jar bij

Werk de AEM SDK ten minste maandelijks bij op of kort na de laatste donderdag van elke maand. Dit is de releasekadentie voor AEM als Cloud Service &quot;functiereleases&quot;.

>[!WARNING]
>
> Als u de QuickStart-jar wilt bijwerken naar een nieuwe versie, moet u de volledige lokale ontwikkelomgeving vervangen. Dit leidt tot verlies van alle code, configuratie en inhoud in de lokale AEM. Zorg ervoor dat om het even welke code, config of inhoud die niet zou moeten worden vernietigd veilig aan Git wordt begaan, of uit de lokale AEM instantie als AEM Pakketten wordt uitgevoerd.

### Hoe te vermijden inhoudsverlies wanneer het bevorderen van de AEM SDK

Door de upgrade van de AEM SDK wordt in feite een geheel nieuwe AEM-runtime gemaakt, waaronder een nieuwe opslagplaats. Dit betekent dat eventuele wijzigingen in de opslagplaats van een eerdere AEM SDK verloren gaan. Hieronder volgen levensvatbare strategieën voor het ondersteunen van blijvende inhoud tussen AEM SDK-upgrades en u kunt deze op discrete wijze of in overleg gebruiken:

1. Maak een inhoudspakket dat gewijd is aan het bevatten van &#39;sample&#39;-inhoud voor hulp bij ontwikkeling en onderhoud dit in Git. Alle inhoud die via AEM SDK-upgrades moet worden voortgezet, blijft in dit pakket aanwezig en wordt opnieuw geïmplementeerd nadat de AEM SDK is bijgewerkt.
1. Gebruik een [eik-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) met de `includepaths` instructie om inhoud van de vorige AEM SDK-opslagplaats te kopiëren naar de nieuwe AEM SDK-opslagplaats.
1. Maak een back-up van alle inhoud met AEM pakketbeheer en inhoudspakketten op de vorige AEM SDK en installeer deze opnieuw op de nieuwe AEM SDK.

Herinner me, gebruikend de bovengenoemde benaderingen om code tussen AEM verbeteringen van SDK te handhaven, wijst op een ontwikkelantipatroon. De niet-besteedbare code zou in uw Ontwikkeling winde moeten voortkomen en in AEM SDK via plaatsingen stromen.

## Problemen oplossen

## Als u dubbelklikt op het QuickStart Jar-bestand, treedt er een fout op{#troubleshooting-double-click}

Wanneer u dubbelklikt op de Quickstart Jar om te starten, wordt een modaal foutbericht weergegeven om te voorkomen dat AEM lokaal wordt gestart.

![Problemen oplossen - Dubbelklik op het QuickStart Jar-bestand](./assets/aem-runtime/troubleshooting__double-click.png)

Dit komt omdat AEM als Cloud Service Quickstart Jar het dubbelklikken van de Jar van QuickStart niet steunt om plaatselijk AEM te beginnen. In plaats daarvan moet u het Jar-bestand vanaf die opdrachtregel uitvoeren.

Om de dienst van de Auteur AEM, in de folder te beginnen die QuickStart Jar bevat en het bevel uit te voeren: `cd`

`$ java -jar aem-author-p4502.jar`

of, om de publicatieservice van AEM te beginnen, in de folder die QuickStart Jar bevat en het bevel uitvoeren: `cd`

`$ java -jar aem-author-p4503.jar`

## Het beginnen van Jar Quickstart van de bevellijn aborteert onmiddellijk{#troubleshooting-java-8}

Wanneer het beginnen van Jar QuickStart van de bevellijn, het proces onmiddellijk aborteert en de AEM dienst begint niet, met de volgende fout:

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

De reden hiervoor is dat AEM als Cloud Service Java SDK 11 vereist en u een andere versie gebruikt, die waarschijnlijk Java 8 is. Download en installeer [Oracle Java SDK 11 om dit probleem op te lossen](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2fx jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Nadat Java SDK 11 is geïnstalleerd, controleert u of dit de actieve versie is door het volgende via de opdrachtregel uit te voeren.

Zodra Java 11 SDK wordt geïnstalleerd, verifieer het de actieve versie door het bevel van de bevellijn in werking te stellen is:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Aanvullende bronnen

+ [SDK AEM downloaden](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker downloaden](https://www.docker.com/)
+ [Documentatie Experience Manager Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/dispatcher.html)
