---
title: Lokale AEM Runtime instellen voor AEM als ontwikkeling van Cloud Servicen
description: Opstelling Lokale AEM Runtime die de AEM als QuickStart Jar van SDK van de Cloud Service gebruikt.
feature: Gereedschappen voor ontwikkelaars
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Ontwikkeling
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '1734'
ht-degree: 1%

---


# Lokale AEM-runtime instellen

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Lokale AEM"
>abstract="Adobe Experience Manager (AEM) kan lokaal worden uitgevoerd met de AEM als QuickStart Jar van een Cloud Service-SDK. Dit staat ontwikkelaars toe om op te stellen aan, en douanecode, configuratie en inhoud te testen alvorens het aan broncontrole vast te leggen, en het op te stellen aan een AEM als milieu van de Cloud Service."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEM downloaden als Cloud Service SDK"

Adobe Experience Manager (AEM) kan lokaal worden uitgevoerd met de AEM als QuickStart Jar van een Cloud Service-SDK. Dit staat ontwikkelaars toe om op te stellen aan, en douanecode, configuratie en inhoud te testen alvorens het aan broncontrole vast te leggen, en het op te stellen aan een AEM als milieu van de Cloud Service.

Merk op dat `~` als steno voor de Folder van de Gebruiker wordt gebruikt. In Windows is dit het equivalent van `%HOMEPATH%`.

## Java installeren

Experience Manager is een Java-toepassing en daarom is ondersteuning van ontwikkelingstools vereist voor de SDK van Java.

1. [Download en installeer de nieuwste Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Controleer of de Java 11 SDK is geïnstalleerd met de opdracht:
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Download de AEM als Cloud Service SDK

De AEM als Cloud Service SDK, of AEM SDK, bevat de Quickstart Jar die wordt gebruikt om AEM Auteur in werking te stellen en plaatselijk voor ontwikkeling, evenals de compatibele versie van de Hulpmiddelen van de Verzender te publiceren.

1. Meld u aan bij [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) met uw Adobe ID
   + Merk op dat uw organisatie __must__ voor AEM als Cloud Service moet worden provisioned om de AEM als Cloud Service SDK te downloaden.
1. Navigeer aan __AEM als Cloud Service__ lusje
1. Sorteren op __Gepubliceerde datum__ in __Aflopende volgorde__
1. Klik op de laatste __AEM SDK__ resultaatrij
1. Controleer en accepteer de EULA en tik op de knop __Downloaden__

## De QuickStart-jar extraheren uit het ZIP van de AEM SDK

1. Het gedownloade `aem-sdk-XXX.zip`-bestand uitpakken

## Lokale AEM-auteurservice instellen{#set-up-local-aem-author-service}

De lokale AEM Author Service biedt ontwikkelaars een lokale ervaring met auteurs van digitale markten/content die ze delen om inhoud te maken en te beheren.  De AEM AuteurDienst wordt ontworpen zowel als creatie als voorproefmilieu, toestaand de meeste bevestigingen van eigenschapontwikkeling kunnen tegen het worden uitgevoerd, die tot het een essentieel element van het lokale ontwikkelingsproces maken.

1. De map `~/aem-sdk/author` maken
1. Kopieer het bestand __QuickStart JAR__ naar `~/aem-sdk/author` en wijzig de naam in `aem-author-p4502.jar`
1. Start de lokale AEM-auteurservice door het volgende uit te voeren vanaf de opdrachtregel:
   + `java -jar aem-author-p4502.jar`
      + Geef het beheerderswachtwoord op als `admin`. Om het even welk admin wachtwoord is aanvaardbaar, nochtans adviseert zijn om het gebrek voor lokale ontwikkeling te gebruiken om de behoefte te verminderen om te vormen.

   U *kunt niet* beginnen de AEM als Cloud Service QuickStart Jar [door](#troubleshooting-double-click) tweemaal te klikken.
1. Open de lokale AEM-auteurservice op [http://localhost:4502](http://localhost:4502) in een webbrowser

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Lokale AEM-publicatieservice instellen

De lokale AEM-publicatieservice biedt ontwikkelaars de lokale ervaring die eindgebruikers van de AEM zullen hebben, zoals bladeren door de website die op AEM wordt gehost. Een lokale AEM-publicatieservice is belangrijk omdat deze integreert met AEM [Dispatcher-gereedschappen](./dispatcher-tools.md) van SDK en ontwikkelaars in staat stelt de ervaring voor eindgebruikers met rook te testen en nauwkeurig af te stemmen.

1. De map `~/aem-sdk/publish` maken
1. Kopieer het bestand __QuickStart JAR__ naar `~/aem-sdk/publish` en wijzig de naam in `aem-publish-p4503.jar`
1. Start de lokale AEM-publicatieservice door het volgende vanaf de opdrachtregel uit te voeren:
   + `java -jar aem-publish-p4503.jar`
      + Geef het beheerderswachtwoord op als `admin`. Om het even welk admin wachtwoord is aanvaardbaar, nochtans adviseert zijn om het gebrek voor lokale ontwikkeling te gebruiken om de behoefte te verminderen om te vormen.

   U *kunt niet* beginnen de AEM als Cloud Service QuickStart Jar [door](#troubleshooting-double-click) tweemaal te klikken.
1. Open de lokale AEM-publicatieservice op [http://localhost:4503](http://localhost:4503) in een webbrowser

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Inhoudsdistributie simuleren {#content-distribution}

In een ware milieu van de Cloud Service wordt de inhoud van de Auteur aan de Publish Dienst verdeeld gebruikend [Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) en de Pijpleiding van de Adobe. De [Adobe Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) is een geïsoleerde microservice die alleen beschikbaar is in de cloud-omgeving.

Tijdens de ontwikkeling, kan het wenselijk zijn om de distributie van inhoud te simuleren gebruikend de lokale auteur en de Publish dienst. Dit kan worden bereikt door de agenten van de erfenisReplicatie toe te laten.

>[!NOTE]
>
> De agenten van de replicatie zijn slechts beschikbaar aan gebruik in lokale QuickStart JAR en verstrekken slechts een simulatie van inhoudsdistributie.

1. Meld u aan bij de **Auteur**-service en navigeer naar [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Klik **StandaardAgent (publiceren)** om de standaardagent van de Replicatie te openen.
1. Klik **uitgeven** om de configuratie van de agent te openen.
1. Werk de volgende velden bij onder het tabblad **Instellingen**:

   + **Ingeschakeld**  - true controleren
   + **Gebruiker-id**  agent - dit veld leeg laten

   ![Configuratie van replicatieagent - instellingen](assets/aem-runtime/settings-config.png)

1. Werk onder het tabblad **Vervoer** de volgende velden bij:

   + **URI** -  `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Gebruiker** -  `admin`
   + **Wachtwoord** -  `admin`

   ![Configuratie replicatieagent - Vervoer](assets/aem-runtime/transport-config.png)

1. Klik **Ok** om de configuratie op te slaan en de **Default** Replication Agent in te schakelen.
1. U kunt nu wijzigingen aanbrengen in de inhoud van de service Auteur en deze publiceren naar de service Publiceren.

![Pagina publiceren](assets/aem-runtime/publish-page-changes.png)

## Snelstartmodi voor Jar

De naam van de QuickStart-jar `aem-<tier>_<environment>-p<port number>.jar` geeft aan hoe deze wordt gestart. Wanneer AEM zoals begonnen in een specifieke rij, auteur of publiceert, kan het niet in de afwisselende rij worden veranderd. Hiervoor moet de `crx-Quickstart`-map die tijdens de eerste uitvoering is gegenereerd, worden verwijderd en moet QuickStart Jar opnieuw worden uitgevoerd. Het milieu en de Havens kunnen worden veranderd, nochtans vereisen zij einde/begin van de lokale AEM instantie.

Het wijzigen van omgevingen, `dev`, `stage` en `prod`, kan nuttig zijn voor ontwikkelaars om ervoor te zorgen dat omgevingspecifieke configuraties correct worden gedefinieerd en AEM worden opgelost. Aanbevolen wordt om lokale ontwikkeling vooral uit te voeren in de standaarduitvoermodus `dev`.

De beschikbare permutaties zijn als volgt:

+ `aem-author-p4502.jar`
   + Als Auteur in Dev loopwijze op haven 4502
+ `aem-author_dev-p4502.jar`
   + Als auteur in Dev loopwijze op haven 4502 (zelfde als `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + Als Auteur in Staging run mode op poort 4502
+ `aem-author_prod-p4502.jar`
   + Als Auteur in de run mode van de Productie op haven 4502
+ `aem-publish-p4503.jar`
   + Als Auteur in Dev loopwijze op haven 4503
+ `aem-publish_dev-p4503.jar`
   + Als Auteur in Dev loopwijze op haven 4503 (zelfde als `aem-publish-p4503.jar`)
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

## Optionele lokale AEM-runtime instellingstaken

+ __De variabelen van het OSGi- configuratiemilieu en geheime__ variabelen worden  [speciaal geplaatst voor AEM lokale runtime](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development), eerder dan het beheren van hen die de lucht CLI gebruiken.

## Wanneer werkt u de QuickStart-jar bij

Werk de AEM SDK ten minste maandelijks bij op of kort na de laatste donderdag van elke maand. Dit is de releasekadentie voor AEM als Cloud Service &quot;functiereleases&quot;.

>[!WARNING]
>
> Als u de QuickStart-jar wilt bijwerken naar een nieuwe versie, moet u de volledige lokale ontwikkelomgeving vervangen. Dit leidt tot verlies van alle code, configuratie en inhoud in de lokale AEM. Zorg ervoor dat om het even welke code, config of inhoud die niet zou moeten worden vernietigd veilig aan Git wordt begaan, of uit de lokale AEM instantie als AEM Pakketten wordt uitgevoerd.

### Hoe te vermijden inhoudsverlies wanneer het bevorderen van de AEM SDK

Door de upgrade van de AEM SDK wordt in feite een geheel nieuwe AEM-runtime gemaakt, waaronder een nieuwe opslagplaats. Dit betekent dat eventuele wijzigingen in de opslagplaats van een eerdere AEM SDK verloren gaan. Hieronder volgen levensvatbare strategieën voor het ondersteunen van blijvende inhoud tussen AEM SDK-upgrades en u kunt deze op discrete wijze of in overleg gebruiken:

1. Maak een inhoudspakket dat gewijd is aan het bevatten van &#39;sample&#39;-inhoud voor hulp bij ontwikkeling en onderhoud dit in Git. Alle inhoud die via AEM SDK-upgrades moet worden voortgezet, blijft in dit pakket aanwezig en wordt opnieuw geïmplementeerd nadat de AEM SDK is bijgewerkt.
1. Gebruik [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) met de instructie `includepaths` om inhoud van de vorige AEM SDK-opslagplaats naar de nieuwe AEM SDK-opslagplaats te kopiëren.
1. Maak een back-up van alle inhoud met AEM pakketbeheer en inhoudspakketten op de vorige AEM SDK en installeer deze opnieuw op de nieuwe AEM SDK.

Herinner me, gebruikend de bovengenoemde benaderingen om code tussen AEM verbeteringen van SDK te handhaven, wijst op een ontwikkelantipatroon. De niet-besteedbare code zou in uw Ontwikkeling winde moeten voortkomen en in AEM SDK via plaatsingen stromen.

## Problemen oplossen

## Als u dubbelklikt op het QuickStart Jar-bestand, treedt een fout op{#troubleshooting-double-click}

Wanneer u dubbelklikt op de Quickstart Jar om te starten, wordt een modaal foutbericht weergegeven om te voorkomen dat AEM lokaal wordt gestart.

![Problemen oplossen - Dubbelklik op het QuickStart Jar-bestand](./assets/aem-runtime/troubleshooting__double-click.png)

Dit komt omdat AEM als Cloud Service Quickstart Jar het dubbelklikken van de Jar van QuickStart niet steunt om plaatselijk AEM te beginnen. In plaats daarvan moet u het Jar-bestand vanaf die opdrachtregel uitvoeren.

Als u de AEM-auteurservice wilt starten, `cd` naar de map met de QuickStart-jar en voert u de opdracht uit:

`$ java -jar aem-author-p4502.jar`

of, om de publicatieservice van AEM te beginnen, `cd` in de folder die de Jar van QuickStart bevat en het bevel uitvoeren:

`$ java -jar aem-author-p4503.jar`

## De aanvang van Jar van QuickStart van de bevellijn aborteert onmiddellijk{#troubleshooting-java-8}

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

De reden hiervoor is dat AEM als Cloud Service Java SDK 11 vereist en u een andere versie gebruikt, die waarschijnlijk Java 8 is. Download en installeer [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) om dit probleem op te lossen.
Nadat Java SDK 11 is geïnstalleerd, controleert u of dit de actieve versie is door het volgende via de opdrachtregel uit te voeren.

Zodra Java 11 SDK wordt geïnstalleerd, verifieer het de actieve versie door het bevel van de bevellijn in werking te stellen is:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Aanvullende bronnen

+ [SDK AEM downloaden](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker downloaden](https://www.docker.com/)
+ [Documentatie Experience Manager Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/dispatcher.html)
