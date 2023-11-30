---
title: Java&trade; API Best practices in AEM
description: AEM is gebaseerd op een rijke open-source softwarestack die veel Java&trade; API's voor gebruik tijdens de ontwikkeling beschikbaar maakt. In dit artikel worden de belangrijkste API's besproken en wordt aangegeven wanneer en waarom deze moeten worden gebruikt.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2079'
ht-degree: 0%

---

# Aanbevolen werkwijzen voor Java™ API

Adobe Experience Manager (AEM) is gebaseerd op een rijke open-source softwarestack die veel Java™ API&#39;s beschikbaar maakt voor gebruik tijdens de ontwikkeling. In dit artikel worden de belangrijkste API&#39;s besproken en wordt aangegeven wanneer en waarom deze moeten worden gebruikt.

AEM is gebaseerd op vier primaire Java™ API-sets.

* **Adobe Experience Manager (AEM)**

   * Productabstracties zoals pagina&#39;s, middelen, workflows, enz.

* **Apache Sling Web Framework**

   * REST en op bron-gebaseerde abstracties zoals middelen, waardekaarten, en HTTP- verzoeken.

* **JCR (Apache Jackrabbit Oak)**

   * Abstracties van gegevens en inhoud, zoals knooppunten, eigenschappen en sessies.

* **OSGi (Apache Felix)**

   * OSGi de abstracties van de toepassingscontainer zoals de diensten en (OSGi) componenten.

## Java™ API-voorkeursregel &quot;Miniatuur&quot;

De algemene regel is om de voorkeur te geven aan API&#39;s/abstracties in de volgende volgorde:

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

Als een API door AEM wordt verstrekt, verkies het over [!DNL Sling], JCR en OSGi. Als AEM geen API biedt, geeft u de voorkeur [!DNL Sling] over JCR en OSGi.

Deze volgorde is een algemene regel, wat betekent dat er uitzonderingen bestaan. Aanvaardbare redenen om van deze regel af te wijken zijn:

* Bekende uitzonderingen, zoals hieronder beschreven.
* De vereiste functionaliteit is niet beschikbaar in een API op een hoger niveau.
* Werken in de context van bestaande code (aangepaste code of AEM productcode) die zelf een minder voorkeursAPI gebruikt, en de kosten om naar de nieuwe API te gaan zijn niet te rechtvaardigen.

   * Het is beter de API op een lager niveau consequent te gebruiken dan een combinatie te maken.

## API&#39;s AEM

* [**JavaDocs AEM API**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM API&#39;s bieden abstracties en functionaliteit die specifiek zijn voor gebruikssituaties die in productie zijn.

AEM [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) en [Pagina](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API&#39;s bieden abstracties voor `cq:Page` knooppunten in AEM die webpagina&#39;s vertegenwoordigen.

Deze knooppunten zijn beschikbaar via [!DNL Sling] API&#39;s als bronnen en JCR API&#39;s als knooppunten AEM API&#39;s bieden abstracties voor veelvoorkomende gebruiksgevallen. Het gebruik van de AEM-API&#39;s zorgt voor een consistent gedrag tussen AEM product en aanpassingen en uitbreidingen die moeten worden AEM.

### com.adobe.&#42; vs com.day.&#42; API&#39;s

AEM API&#39;s hebben een voorkeur voor een pakket, die wordt aangegeven door de volgende Java™-pakketten, in volgorde van voorkeur:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

De `com.adobe.cq` pakket ondersteunt productgebruiksgevallen terwijl `com.adobe.granite` biedt ondersteuning voor platformgebruikscenario&#39;s voor meerdere producten, zoals workflows of taken (die worden gebruikt voor verschillende producten: AEM Assets, sites, enzovoort).

De `com.day.cq` pakket bevat &quot;originele&quot; API&#39;s. Deze API&#39;s hebben betrekking op basisabstracties en -functies die bestonden vóór en/of rond de overname door Adobe van [!DNL Day CQ]. Deze API&#39;s worden ondersteund en moeten worden vermeden, tenzij `com.adobe.cq` of `com.adobe.granite` pakketten bieden GEEN (nieuwer) alternatief.

Nieuwe abstracties, zoals [!DNL Content Fragments] en [!DNL Experience Fragments] zijn ingebouwd in het `com.adobe.cq` ruimte in plaats van `com.day.cq` hieronder beschreven.

### Query-API&#39;s

AEM ondersteunt meerdere querytalen. De drie belangrijkste talen zijn [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath, en [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

De belangrijkste zorg is het handhaven van een verenigbare vraagtaal over de codebasis, om ingewikkeldheid en kosten te verminderen om te begrijpen.

Alle querytalen hebben in feite dezelfde prestatieprofielen als [!DNL Apache Oak] trans-pileert hen aan JCR-SQL2 voor definitieve vraaguitvoering, en de omzettingstijd aan JCR-SQL2 is verwaarloosbaar in vergelijking met de vraagtijd zelf.

De voorkeurs-API is [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html), dat de abstractie op het hoogste niveau is en een robuuste API biedt voor het maken, uitvoeren en ophalen van resultaten voor query&#39;s, en het volgende biedt:

* Eenvoudige, parameterized vraagbouw (vraagparams die als Kaart worden gemodelleerd)
* Oorspronkelijk [Java™ API en HTTP API&#39;s](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* [Foutopsporing AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [AEM voorspellen](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html) ondersteuning van algemene queryvereisten

* Uitbreidbare API, zodat u aangepaste [query-voorspellingen](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* JCR-SQL2 en XPath kunnen rechtstreeks via [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) en [JCR-API&#39;s](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), retourneert u resultaten [[!DNL Sling] Bronnen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) of [JCR-knooppunten](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), respectievelijk.

>[!CAUTION]
>
>AEM QueryBuilder-API lekt een ResourceResolver-object. Om dit lek te beperken, volg dit [codevoorbeeld](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## [!DNL Sling] API&#39;s

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) is het RESTful-webframework dat AEM ondersteunt. [!DNL Sling] verstrekt HTTP- verzoek het verpletteren, modellenJCR knopen als middelen, verstrekt veiligheidscontext, en veel meer.

[!DNL Sling] API&#39;s hebben het extra voordeel dat ze voor extensies worden gemaakt. Dit betekent dat het vaak eenvoudiger en veiliger is om het gedrag van toepassingen die met [!DNL Sling] API&#39;s dan de minder uitbreidbare JCR-API&#39;s.

### Algemeen gebruik van [!DNL Sling] API&#39;s

* JCR-knooppunten benaderen als [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) en toegang tot hun gegevens via [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Beveiligingscontext bieden via de [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Middelen maken en verwijderen via ResourceResolver [methoden maken/verplaatsen/kopiëren/verwijderen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Eigenschappen bijwerken via het dialoogvenster [ModisibleValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Bouwstenen voor aanvraagverwerking maken

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet-filters](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Asynchrone bouwstenen voor werkverwerking

   * [Gebeurtenis- en taakhandlers](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planner](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Verkoopmodellen](https://sling.apache.org/documentation/bundles/models.html)

* [Servicegebruikers](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## JCR-API&#39;s

* **[JCR 2.0 JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

De [JCR (Java™ Content Repository) 2.0 API&#39;s](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) deel uitmaakt van een specificatie voor de uitvoering van het GCO (in het geval van AEM; [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). Alle JCR-implementatie moet in overeenstemming zijn met deze API&#39;s en deze implementeren. Dit is dus de API op het laagste niveau voor interactie met AEM inhoud.

De JCR zelf is een hiërarchische/op boom-gebaseerde NSQL datastore AEM als zijn inhoudsbewaarplaats gebruikt. De JCR heeft een uitgebreide reeks ondersteunde API&#39;s, variërend van inhoud-CRUD tot het opvragen van inhoud. Ondanks deze robuuste API is het zeldzaam dat ze de voorkeur hebben boven de AEM op een hoger niveau en [!DNL Sling] abstracties.

Geef altijd de voorkeur aan de JCR API&#39;s boven de Apache Jackrabbit Oak API&#39;s. De API&#39;s van de JCR zijn bedoeld voor ***interactie*** met een JCR-opslagplaats, terwijl de Eak API&#39;s bedoeld zijn voor ***uitvoeren*** een JCR-opslagplaats.

### Algemene misvattingen over JCR API&#39;s

Terwijl de JCR AEM opslagplaats voor inhoud is, hebben de API&#39;s NIET de voorkeursmethode voor interactie met de inhoud. In plaats daarvan geeft u de voorkeur aan de API&#39;s voor AEM (pagina, middelen, tag, enzovoort) of Sling Resource API&#39;s omdat deze betere abstracties bieden.

>[!CAUTION]
>
>Het brede gebruik van de Sessie- en Node-interfaces van de JCR API&#39;s in een AEM toepassing is codegeur. Zorgen [!DNL Sling] In plaats daarvan moeten API&#39;s worden gebruikt.

### Veelvoorkomende toepassingen van JCR API&#39;s

* [Toegangsbeheer](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Toegestaan beheer (gebruikers/groepen)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR-observatie (luisteren naar JCR-gebeurtenissen)
* Diepknoopstructuren maken

   * De API&#39;s voor verkoop ondersteunen het maken van bronnen, maar de JCR API&#39;s beschikken over gebruiksvriendelijke methoden voor [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) en [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) dat versnelde het creëren van diepe structuren .

## OSGi API&#39;s

* [**OSGi R6 JavaDocs**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2 Component Annotations JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Metatype Annotations JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Er is weinig overlapping tussen de OSGi API&#39;s en de API&#39;s op een hoger niveau (AEM, [!DNL Sling], en JCR), en de noodzaak om OSGi API&#39;s te gebruiken is zeldzaam en vereist een hoog niveau van AEM ontwikkelingsdeskundigheid.

### OSGi vs Apache Felix API&#39;s

OSGi bepaalt een specificatie alle containers OSGi moeten uitvoeren en met in overeenstemming zijn. AEM OSGi-implementatie, Apache Felix, biedt ook verschillende van zijn eigen API&#39;s.

* Voorkeur voor OSGi API&#39;s (`org.osgi`) via Apache Felix API&#39;s (`org.apache.felix`).

### Veelvoorkomende toepassingen van OSGi API&#39;s

* OSGi-annotaties voor het declareren van OSGi-diensten en -componenten.

   * Voorkeur [OSGi Declarative Services (DS) 1.2 Annotaties](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Felix SCR Annotaties](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) voor het aangeven van OSGi-diensten en -componenten

* OSGi API&#39;s voor dynamisch in-code [OSGi-services/onderdelen niet/geregistreerd](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Het gebruik van OSGi DS 1.2-annotaties verdient de voorkeur wanneer voorwaardelijk OSGi Service/Component management niet nodig is (wat meestal het geval is).

## Uitzonderingen op de regel

Hieronder volgen algemene uitzonderingen op de hierboven gedefinieerde regels.

### OSGi API&#39;s

Wanneer het behandelen van lage OSGi abstracties, zoals het bepalen van of het lezen in eigenschappen van de component OSGi, de nieuwere abstracties die door `org.osgi` hebben de voorkeur boven Sling abstractions op een hoger niveau. De concurrerende Sling abstracties zijn niet gemarkeerd als `@Deprecated` en stelt de `org.osgi` alternatief.

Merk ook op de definitie van de OSGi- configuratieknooppunt verkiest `cfg.json` over de `sling:OsgiConfig` gebruiken.

### Elementen-API&#39;s AEM

* Voorkeur [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) over [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Terwijl de `com.day.cq` De elementen-API&#39;s bieden meer gratis tools voor het AEM van gebruiksscenario&#39;s voor middelenbeheer.
   * De API&#39;s van Granite Assets ondersteunen gebruiksscenario&#39;s voor middelenbeheer op laag niveau (versie, relaties).

### Query-API&#39;s

* AEM QueryBuilder ondersteunt bepaalde queryfuncties, zoals [suggesties](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), spellingcontrole en indextips voor minder algemene functies. Om met deze functies te vragen wordt JCR-SQL2 geprefereerd.

### [!DNL Sling] Servlet-registratie {#sling-servlet-registration}

* [!DNL Sling] servlet-registratie, voorkeur [OSGi DS 1.2-annotaties met @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) over `@SlingServlet`

### [!DNL Sling] Filterregistratie {#sling-filter-registration}

* [!DNL Sling] filterregistratie, voorkeur [OSGi DS 1.2-annotaties met @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) over `@SlingFilter`

## Nuttige codefragmenten

Hieronder vindt u handige Java™-codefragmenten die tips en trucs weergeven voor veelvoorkomende toepassingen met behulp van besproken API&#39;s. Deze fragmenten tonen ook hoe u van API&#39;s met minder voorkeuren naar API&#39;s met meer voorkeuren kunt gaan.

### JCR-sessie naar [!DNL Sling] ResourceResolver

#### Auto-closing Sling ResourceResolver

Sinds AEM 6.2 [!DNL Sling] ResourceResolver `AutoClosable` in een [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) instructie. Met behulp van deze syntaxis, een expliciete aanroep van `resourceResolver .close()` is niet nodig.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### Handmatig gesloten Sling ResourceResolver

ResourceResolvers kan handmatig worden gesloten in een `finally` blokkeren, als de hierboven getoonde techniek voor automatisch sluiten niet kan worden gebruikt.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### JCR-pad naar [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR-knooppunt naar [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] aan AEM

#### Aanbevolen aanpak

De `DamUtil.resolveToAsset(..)` functie lost om het even welke bron onder het `dam:Asset` naar het object Asset door de boomstructuur zo nodig omhoog te lopen.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Alternatieve benadering

Als u een resource wilt aanpassen aan een element, moet de resource zelf de `dam:Asset` knooppunt.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Bron naar AEM pagina

#### Aanbevolen aanpak

`pageManager.getContainingPage(..)` lost om het even welke bron onder het `cq:Page` naar het object Pagina door de structuur naar boven te doorlopen.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Alternatieve benadering {#alternative-approach-1}

Als u een bron wilt aanpassen aan een pagina, moet de bron zelf de `cq:Page` knooppunt.

```java
Page page = resource.adaptTo(Page.class);
```

### Eigenschappen van AEM pagina lezen

Gebruik de getters van het object Page om bekende eigenschappen (`getTitle()`, `getDescription()`, enzovoort) en `page.getProperties()` om de `[cq:Page]/jcr:content` ValueMap voor het ophalen van andere eigenschappen.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Eigenschappen van metagegevens van AEM element lezen

De API voor middelen biedt handige methoden voor het lezen van eigenschappen van de `[dam:Asset]/jcr:content/metadata` knooppunt. Dit is geen ValueMap, de tweede parameter (standaardwaarde en automatisch gegoten casting) wordt niet ondersteund.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lezen [!DNL Sling] [!DNL Resource] eigenschappen {#read-sling-resource-properties}

Wanneer eigenschappen worden opgeslagen op locaties (eigenschappen of relatieve bronnen) waartoe de AEM-API&#39;s (Pagina, Element) niet rechtstreeks toegang hebben, wordt [!DNL Sling] De middelen, en ValueMaps kunnen worden gebruikt om de gegevens te verkrijgen.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In dit geval moet het AEM mogelijk worden omgezet in een [!DNL Sling] [!DNL Resource] om de gewenste eigenschap of subbron efficiënt te vinden.

#### Pagina AEM naar [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Element AEM aan [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Eigenschappen schrijven met [!DNL Sling]&#39;s ModisibleValueMap

Gebruiken [!DNL Sling]s [ModisibleValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) eigenschappen naar knooppunten schrijven. Dit kan alleen naar het huidige knooppunt worden geschreven (relatieve paden van eigenschappen worden niet ondersteund).

Noteer de oproep aan `.adaptTo(ModifiableValueMap.class)` schrijft toestemmingen aan het middel vereist, anders keert het ongeldig terug.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Een AEM pagina maken

Gebruik altijd PageManager om pagina&#39;s tot stand te brengen aangezien het een Malplaatje van de Pagina neemt, wordt vereist om Pagina&#39;s in AEM behoorlijk te bepalen en te initialiseren.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Een [!DNL Sling] Bron

ResourceResolver steunt basisverrichtingen voor het creëren van middelen. Bij het maken van abstracties op een hoger niveau (AEM Pagina&#39;s, Elementen, Codes, enzovoort) gebruikt u de methoden die worden geleverd door de respectievelijke managers.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Een [!DNL Sling] Bron

ResourceResolver ondersteunt het verwijderen van een resource. Bij het maken van abstracties op een hoger niveau (AEM Pagina&#39;s, Elementen, Codes, enzovoort) gebruikt u de methoden die worden geleverd door de respectievelijke managers.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
