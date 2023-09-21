---
title: De lokale AEM SDK instellen voor AEM as a Cloud Service ontwikkeling
description: Stel de lokale AEM SDK-runtime in met de QuickStart Jar van de AEM as a Cloud Service SDK.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: 2a412126ac7a67a756d4101d56c1715f0da86453
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 1%

---

# SDK voor lokale AEM instellen {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Lokale AEM Runtime"
>abstract="Adobe Experience Manager (AEM) kan lokaal worden uitgevoerd met de QuickStart Jar van de AEM as a Cloud Service SDK. Dit staat ontwikkelaars toe om op te stellen aan, en douanecode, configuratie en inhoud te testen alvorens het aan broncontrole vast te leggen, en het op te stellen aan een AEM as a Cloud Service milieu."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEM as a Cloud Service SDK downloaden"

Adobe Experience Manager (AEM) kan lokaal worden uitgevoerd met de QuickStart Jar van de AEM as a Cloud Service SDK. Dit staat ontwikkelaars toe om op te stellen aan, en douanecode, configuratie en inhoud te testen alvorens het aan broncontrole vast te leggen, en het op te stellen aan een AEM as a Cloud Service milieu.

Let op: `~` wordt gebruikt als steno voor de Folder van de Gebruiker. In Windows is dit het equivalent van `%HOMEPATH%`.

## Java installeren

Experience Manager is een Java-toepassing en daarom is de Oracle Java SDK vereist voor ondersteuning van ontwikkelingstools.

1. [Download en installeer de nieuwste Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Controleer of Oracle Java 11 SDK is geïnstalleerd met de opdracht:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## Download de AEM as a Cloud Service SDK

De AEM as a Cloud Service SDK, of AEM SDK, bevat de Quickstart Jar die wordt gebruikt om AEM Auteur en Publish plaatselijk voor ontwikkeling in werking te stellen, evenals de compatibele versie van de Hulpmiddelen van de Verzender.

1. Aanmelden bij [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) met uw Adobe ID
   + Merk op dat uw organisatie van de Adobe __moet__ is ingericht voor AEM as a Cloud Service om de AEM as a Cloud Service SDK te downloaden.
1. Ga naar de __AEM as a Cloud Service__ tab
1. Sorteren op __Gepubliceerde datum__ in __Aflopend__ bestellen
1. Klik op de nieuwste __AEM SDK__ resultatenrij
1. Reviseer en accepteer de EULA en tik op de __Downloaden__ knop

## Haal de QuickStart Jar uit het ZIP van de AEM SDK

1. De gedownloade gegevens decomprimeren `aem-sdk-XXX.zip` file

## Lokale AEM Auteur-service instellen{#set-up-local-aem-author-service}

De lokale AEM Auteur Service biedt ontwikkelaars een lokale ervaring met ontwerpers van digitale markten/content die ze delen om inhoud te maken en te beheren.  AEM Auteursdienst wordt ontworpen zowel als creatie als voorproefmilieu, toestaand de meeste bevestigingen van eigenschapontwikkeling kunnen tegen het worden uitgevoerd, die tot het een essentieel element van het lokale ontwikkelingsproces maken.

1. De map maken `~/aem-sdk/author`
1. De __QuickStart JAR__ bestand naar  `~/aem-sdk/author` en hernoemen `aem-author-p4502.jar`
1. Start de lokale AEM Auteur Service door het volgende uit te voeren vanaf de opdrachtregel:
   + `java -jar aem-author-p4502.jar`
      + Geef het beheerderswachtwoord op als `admin`. Om het even welk admin wachtwoord is aanvaardbaar, nochtans adviseert zijn om het gebrek voor lokale ontwikkeling te gebruiken om de behoefte te verminderen om te vormen.

   U *kan* start de AEM als Cloud Service QuickStart Jar [door te dubbelklikken](#troubleshooting-double-click).
1. Ga naar de lokale AEM Auteur-service op [http://localhost:4502](http://localhost:4502) in een webbrowser

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## Lokale AEM publicatieservice instellen

De lokale AEM publicatieservice biedt ontwikkelaars de lokale ervaring die eindgebruikers van de AEM zullen hebben, zoals bladeren door de website die op AEM wordt gehost. Een lokale AEM publicatieservice is belangrijk omdat deze integreert met AEM SDK&#39;s [Verzendgereedschappen](./dispatcher-tools.md) en stelt ontwikkelaars in staat de ervaring voor eindgebruikers te testen en te verfijnen.

1. De map maken `~/aem-sdk/publish`
1. De __QuickStart JAR__ bestand naar  `~/aem-sdk/publish` en hernoemen `aem-publish-p4503.jar`
1. Start de lokale AEM publicatieservice door het volgende vanaf de opdrachtregel uit te voeren:
   + `java -jar aem-publish-p4503.jar`
      + Geef het beheerderswachtwoord op als `admin`. Om het even welk admin wachtwoord is aanvaardbaar, nochtans adviseert zijn om het gebrek voor lokale ontwikkeling te gebruiken om de behoefte te verminderen om te vormen.

   U *kan* start de AEM als Cloud Service QuickStart Jar [door te dubbelklikken](#troubleshooting-double-click).
1. Heb toegang tot de lokale AEM publicatieservice bij [http://localhost:4503](http://localhost:4503) in een webbrowser

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## Lokale AEM instellen in de pre-releasemodus

De lokale AEM-runtime kan worden gestart in [pre-releasemodus](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html) zodat een ontwikkelaar kan bouwen op basis van de functies van de volgende release van de AEM as a Cloud Service. Prerelease wordt ingeschakeld door het `-r prerelease` argument op het eerste begin van de lokale AEM runtime. Dit kan zowel met lokale AEM Auteur als AEM de diensten van de Publicatie worden gebruikt.


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## Inhoudsdistributie simuleren {#content-distribution}

In een ware milieu van de Cloud Service wordt de inhoud van de Auteur aan de Publish Dienst verdeeld gebruikend [Distributie van inhoud verkopen](https://sling.apache.org/documentation/bundles/content-distribution.html) en de Adobe Pipeline. De [Adobe Pipet](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) is een geïsoleerde microservice die alleen beschikbaar is in de cloud-omgeving.

Tijdens de ontwikkeling, kan het wenselijk zijn om de distributie van inhoud te simuleren gebruikend de lokale auteur en de Publish dienst. Dit kan worden bereikt door de agenten van de erfenisReplicatie toe te laten.

>[!NOTE]
>
> De agenten van de replicatie zijn slechts beschikbaar aan gebruik in lokale QuickStart JAR en verstrekken slechts een simulatie van inhoudsdistributie.

1. Aanmelden bij de **Auteur** service en navigeer naar [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Klikken **Standaardagent (publiceren)** om de standaardagent van de Replicatie te openen.
1. Klikken **Bewerken** om de configuratie van de agent te openen.
1. Onder de **Instellingen** werkt u de volgende velden bij:

   + **Ingeschakeld** - true controleren
   + **Gebruiker-id agent** - Dit veld leeg laten

   ![Configuratie van replicatieagent - instellingen](assets/aem-runtime/settings-config.png)

1. Onder de **Vervoer** werkt u de volgende velden bij:

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Gebruiker** - `admin`
   + **Wachtwoord** - `admin`

   ![Configuratie replicatieagent - Vervoer](assets/aem-runtime/transport-config.png)

1. Klikken **OK** om de configuratie op te slaan en de **Standaard** Replication Agent.
1. U kunt nu wijzigingen aanbrengen in de inhoud van de service Auteur en deze publiceren naar de service Publiceren.

![Pagina publiceren](assets/aem-runtime/publish-page-changes.png)

## Snelstartmodi voor Jar

De naam van de QuickStart-jar, `aem-<tier>_<environment>-p<port number>.jar` geeft aan hoe de toepassing wordt gestart. Wanneer AEM zoals begonnen in een specifieke rij, auteur of publiceert, kan het niet in de afwisselende rij worden veranderd. Om dit te doen, `crx-Quickstart` de map die tijdens de eerste uitvoering wordt gegenereerd, moet worden verwijderd en QuickStart Jar moet opnieuw worden uitgevoerd. Het milieu en de Havens kunnen worden veranderd, nochtans vereisen zij einde/begin van de lokale AEM instantie.

Omgevingen wijzigen, `dev`, `stage` en `prod`, kan voor ontwikkelaars nuttig zijn om ervoor te zorgen dat de milieu-specifieke configuraties correct worden bepaald en door AEM worden opgelost. Aanbevolen wordt om lokale ontwikkeling in de eerste plaats tegen de standaardinstelling uit te voeren `dev` de uitvoermodus van de omgeving.

De beschikbare permutaties zijn als volgt:

| Quickstart Jar-bestandsnaam | Beschrijving van modus |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | Als Auteur in Dev loopwijze op haven 4502 |
| `aem-author_dev-p4502.jar` | Als Auteur in Dev loopwijze op haven 4502 (het zelfde als `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | Als Auteur in Staging run mode op poort 4502 |
| `aem-author_prod-p4502.jar` | Als Auteur in de run mode van de Productie op haven 4502 |
| `aem-publish-p4503.jar` | Als Publiceren in Dev loopwijze op haven 4503 |
| `aem-publish_dev-p4503.jar` | Zoals publiceren op Dev loopwijze op haven 4503 (zelfde als `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | Als Publiceren in Staging-uitvoeringsmodus op poort 4503 |
| `aem-publish_prod-p4503.jar` | Als Publish op de loopwijze van de Productie op haven 4503 |

Merk op dat het havenaantal om het even welke beschikbare haven op de lokale ontwikkelingsmachine kan zijn, echter door overeenkomst:

+ Poort __4502__ wordt gebruikt voor de __lokale AEM Auteur-service__
+ Poort __4503__ wordt gebruikt voor de __lokale AEM publicatieservice__

Het wijzigen van deze instellingen kan aanpassingen in AEM SDK-configuraties vereisen

## Een lokale AEM-runtime stoppen

Als u een lokale AEM-runtime wilt stoppen, AEM de service Auteur of Publiceren, opent u het opdrachtregelvenster dat is gebruikt om de AEM-runtime te starten en tikt u op `Ctrl-C`. Wacht tot AEM is afgesloten. Wanneer het sluitingsproces volledig is, is de herinnering van de bevellijn beschikbaar.

## Optionele lokale AEM-runtime instellingstaken

+ __OSGi variabelen van het configuratiemilieu en geheim variabelen__ zijn [speciaal ingesteld voor de AEM lokale runtime](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development)in plaats van deze te beheren met behulp van de AIR CLI.

## Wanneer werkt u de QuickStart-jar bij

Werk de AEM SDK ten minste maandelijks bij op of kort na de laatste donderdag van elke maand. Dit is de releasekadentie voor AEM as a Cloud Service &quot;feature releases&quot;.

>[!WARNING]
>
> Als u de QuickStart-jar wilt bijwerken naar een nieuwe versie, moet u de volledige lokale ontwikkelomgeving vervangen. Dit leidt tot verlies van alle code, configuratie en inhoud in de lokale AEM. Zorg ervoor dat om het even welke code, config of inhoud die niet zou moeten worden vernietigd veilig aan Git wordt begaan, of uit de lokale AEM instantie als AEM Pakketten wordt uitgevoerd.

### Hoe te om inhoudsverlies te vermijden wanneer het bevorderen van de AEM SDK

Door de upgrade van de AEM SDK wordt in feite een geheel nieuwe AEM-runtime gemaakt, waaronder een nieuwe opslagplaats. Dit betekent dat eventuele wijzigingen in de opslagplaats van een eerdere AEM SDK verloren gaan. Hieronder volgen levensvatbare strategieën voor het ondersteunen van blijvende inhoud tussen AEM SDK-upgrades en u kunt deze op discrete wijze of in overleg gebruiken:

1. Maak een inhoudspakket dat gewijd is aan het bevatten van &#39;sample&#39;-inhoud voor hulp bij ontwikkeling en onderhoud dit in Git. Alle inhoud die via AEM SDK-upgrades moet worden voortgezet, blijft in dit pakket aanwezig en wordt opnieuw geïmplementeerd nadat de AEM SDK is bijgewerkt.
1. Gebruiken [eik-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) met de `includepaths` gebruiken, om inhoud van de vorige AEM SDK-opslagplaats te kopiëren naar de nieuwe AEM SDK-opslagplaats.
1. Maak een back-up van alle inhoud met AEM pakketbeheer en inhoudspakketten op de vorige AEM SDK en installeer deze opnieuw op de nieuwe AEM SDK.

Herinner me, gebruikend de bovengenoemde benaderingen om code tussen AEM verbeteringen van SDK te handhaven, wijst op een ontwikkelantipatroon. De niet-besteedbare code zou in uw Ontwikkeling winde moeten voortkomen en in AEM SDK via plaatsingen stromen.

## Problemen oplossen

### Als u dubbelklikt op het QuickStart Jar-bestand, treedt er een fout op{#troubleshooting-double-click}

Wanneer u dubbelklikt op de Quickstart Jar om te starten, wordt een modaal foutbericht weergegeven om te voorkomen dat AEM lokaal wordt gestart.

![Problemen oplossen - Dubbelklik op het QuickStart Jar-bestand](./assets/aem-runtime/troubleshooting__double-click.png)

Dit komt doordat AEM as a Cloud Service QuickStart Jar dubbelklikken van de QuickStart Jar om AEM lokaal te starten niet ondersteunt. In plaats daarvan moet u het Jar-bestand vanaf die opdrachtregel uitvoeren.

Ga als volgt te werk om AEM Auteur te starten: `cd` in de directory met de Quickstart Jar en voer de opdracht uit:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

of, om AEM publicatieservice te starten, `cd` in de directory met de Quickstart Jar en voer de opdracht uit:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4503.jar
```

>[!TAB Linux]

```shell
$ java -jar aem-author-p4503.jar
```

>[!ENDTABS]

### Het beginnen van Jar Quickstart van de bevellijn aborteert onmiddellijk{#troubleshooting-java-8}

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

Dit komt omdat AEM as a Cloud Service Java SDK 11 vereist en u een andere versie gebruikt, waarschijnlijk Java 8. Download en installeer om dit probleem op te lossen [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2 Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).

Zodra Oracle Java 11 SDK wordt geïnstalleerd, verifieer het de actieve versie door het bevel van de bevellijn in werking te stellen:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

## Aanvullende bronnen

+ [SDK AEM downloaden](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker downloaden](https://www.docker.com/)
+ [Documentatie Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html)
