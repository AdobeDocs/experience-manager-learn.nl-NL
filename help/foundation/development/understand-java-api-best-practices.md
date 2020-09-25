---
title: Aanbevolen procedures voor Java API in AEM
description: AEM is gebaseerd op een rijke open-source softwarestack die veel Java API's beschikbaar maakt voor gebruik tijdens de ontwikkeling. In dit artikel worden de belangrijkste API's besproken en wordt aangegeven wanneer en waarom deze moeten worden gebruikt.
version: 6.2, 6.3, 6.4, 6.5
sub-product: stichting, middelen, sites
feature: null
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
translation-type: tm+mt
source-git-commit: fcb47ee3878f6a789b2151e283431c4806e12564
workflow-type: tm+mt
source-wordcount: '2028'
ht-degree: 0%

---


# Tips en trucs voor Java API

Adobe Experience Manager (AEM) is gebaseerd op een rijke open-source softwarestack die veel Java API&#39;s beschikbaar maakt voor gebruik tijdens de ontwikkeling. In dit artikel worden de belangrijkste API&#39;s besproken en wordt aangegeven wanneer en waarom deze moeten worden gebruikt.

AEM is gebaseerd op vier primaire Java API-sets.

* **Adobe Experience Manager (AEM)**

   * Productabstracties zoals pagina&#39;s, middelen, workflows, enz.

* **[!DNL Apache Sling]Web Framework**

   * REST en op bron-gebaseerde abstracties zoals middelen, waardekaarten, en HTTP- verzoeken.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * Abstracties van gegevens en inhoud, zoals knooppunten, eigenschappen en sessies.

* **[!DNL OSGi (Apache Felix)]**

   * OSGi de abstracties van de toepassingscontainer zoals de diensten en (OSGi) componenten.

## Java API-voorkeursregel &quot;thumb-regel&quot;

De algemene regel is om de voorkeur te geven aan API&#39;s/abstracties in de volgende volgorde:

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

Als een API door AEM wordt verstrekt, verkies het over [!DNL Sling], JCR, en OSGi. Als AEM geen API biedt, heeft u de voorkeur [!DNL Sling] boven JCR en OSGi.

Deze volgorde is een algemene regel, wat betekent dat er uitzonderingen bestaan. Aanvaardbare redenen om van deze regel af te wijken zijn:

* Bekende uitzonderingen, zoals hieronder beschreven.
* De vereiste functionaliteit is niet beschikbaar in een API van een hoger niveau.
* Werken in de context van bestaande code (aangepaste code of AEM productcode) die zelf een minder voorkeursAPI gebruikt, en de kosten om naar de nieuwe API te gaan zijn niet te rechtvaardigen.

   * Het is beter de API op een lager niveau consequent te gebruiken dan een combinatie te maken.

## API&#39;s AEM

* [**JavaDocs AEM API**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API&#39;s bieden abstracties en functionaliteit die specifiek zijn voor gebruikssituaties die in productie zijn.

AEM [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) - en [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) -API&#39;s bieden bijvoorbeeld abstracties voor `cq:Page` knooppunten in AEM die webpagina&#39;s vertegenwoordigen.

Hoewel deze knooppunten beschikbaar zijn via [!DNL Sling] API&#39;s als bronnen en JCR API&#39;s als knooppunten, bieden AEM API&#39;s abstracties voor veelvoorkomende gebruiksgevallen. Het gebruik van de AEM-API&#39;s zorgt voor een consistent gedrag tussen AEM product en aanpassingen en uitbreidingen die moeten worden AEM.

### com.adobe.* vs com.day.* API&#39;s

AEM API&#39;s hebben een voorkeur voor een pakket, die wordt aangegeven door de volgende Java-pakketten, in volgorde van voorkeur:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` ondersteunt productgebruiksgevallen en `com.adobe.granite` ondersteunt toepassingen waarbij meerdere producten worden gebruikt, zoals een workflow of taken (die voor alle producten worden gebruikt): AEM Assets, sites, enz.).

`com.day.cq` bevat &quot;originele&quot; API&#39;s. Deze API&#39;s bieden oplossingen voor basisabstracties en -functies die bestonden vóór en/of rondom de verwerving van Adobe van [!DNL Day CQ]. Deze API&#39;s worden ondersteund en mogen niet worden vermeden, tenzij `com.adobe.cq` of een (nieuwer) alternatief `com.adobe.granite` wordt geboden.

Nieuwe abstracties zoals [!DNL Content Fragments] en [!DNL Experience Fragments] worden in de `com.adobe.cq` ruimte opgebouwd in plaats van `com.day.cq` hieronder beschreven.

### Query-API&#39;s

AEM ondersteunt meerdere querytalen. De drie hoofdtalen zijn [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath en [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

De belangrijkste zorg is het handhaven van een verenigbare vraagtaal over de codebasis, om ingewikkeldheid en kosten te verminderen om te begrijpen.

Alle vraagtalen hebben in feite de zelfde prestatiesprofielen, aangezien [!DNL Apache Oak] overstapt hen aan JCR-SQL2 voor definitieve vraaguitvoering, en de omzettingstijd aan JCR-SQL2 is verwaarloosbaar in vergelijking met de vraagtijd zelf.

De voorkeurs-API is [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), die de abstractie op het hoogste niveau is en een robuuste API biedt voor het samenstellen, uitvoeren en ophalen van resultaten voor query&#39;s, en die het volgende biedt:

* Eenvoudige, parameterized vraagbouw (vraagparams die als Kaart worden gemodelleerd)
* Eigen API&#39; [s van Java en HTTP](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [Foutopsporing OTB-query](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [OOTB voorspelt](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) het steunen van gemeenschappelijke vraagvereisten

* Uitbreidbare API, die voor de ontwikkeling van douane [vraagpredikaten toestaat](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 en XPath kunnen direct via [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) en [JCR APIs](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)worden uitgevoerd, terugkerend resultaten respectievelijk [[!DNL Sling] Middelen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) of [JCR Nodes](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html).

>[!CAUTION]
>
>AEM QueryBuilder-API lekt een ResourceResolver-object. Volg dit [codevoorbeeld](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)om dit lek te beperken.


## [!DNL Sling] API&#39;s

* [**JavaDocs Apache[!DNL Sling]API**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) is het RESTful-webframework dat AEM ondersteunt. [!DNL Sling] verstrekt HTTP- verzoek het verpletteren, modellenJCR knopen als middelen, verstrekt veiligheidscontext, en veel meer.

[!DNL Sling] API&#39;s hebben het extra voordeel dat ze voor extensies worden gemaakt. Dit betekent dat het vaak eenvoudiger en veiliger is om het gedrag van toepassingen die met API&#39; [!DNL Sling] s zijn gemaakt, te verbeteren dan van minder uitbreidbare JCR API&#39;s.

### Veelvoorkomende toepassingen van [!DNL Sling] API&#39;s

* Toegang tot JCR-knooppunten als [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) en toegang tot hun gegevens via [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Het verstrekken van veiligheidscontext via [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Middelen maken en verwijderen via de methoden [](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)create/move/copy/delete van ResourceResolver.
* Eigenschappen bijwerken via de [ModisibleValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Bouwstenen voor aanvraagverwerking

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet-filters](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Asynchrone bouwstenen voor werkverwerking

   * [Gebeurtenis- en taakhandlers](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planningen](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Verkoopmodellen](https://sling.apache.org/documentation/bundles/models.html)

* [Servicegebruikers](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR-API&#39;s

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

De API&#39;s [van](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) JCR (Java Content Repository) 2.0 maken deel uit van een specificatie voor JCR-implementaties (in het geval van AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Alle JCR-implementatie moet in overeenstemming zijn met deze API&#39;s en deze implementeren. Dit is dus de API op het laagste niveau voor interactie met AEM inhoud.

De JCR zelf is een hiërarchische/op boom-gebaseerde NSQL datastore AEM als zijn inhoudsbewaarplaats gebruikt. De JCR heeft een uitgebreide reeks ondersteunde API&#39;s, variërend van inhoud-CRUD tot het opvragen van inhoud. Ondanks deze robuuste API hebben ze maar zelden de voorkeur boven AEM en [!DNL Sling] abstracties op een hoger niveau.

Geef altijd de voorkeur aan de JCR-API&#39;s boven de Apache Jackrabbit Oak-API&#39;s. De API&#39;s van de JCR zijn bedoeld voor ***interactie*** met een JCR-opslagplaats, terwijl de API&#39;s van de eiken bedoeld zijn voor de ***implementatie*** van een JCR-opslagplaats.

### Algemene misvattingen over JCR API&#39;s

Terwijl de JCR AEM opslagplaats voor inhoud is, hebben de API&#39;s NIET de voorkeursmethode voor interactie met de inhoud. In plaats daarvan geeft u de voorkeur aan de AEM-API&#39;s (pagina, middelen, tag, enz.) of Sling Resource APIs aangezien zij betere abstracties verstrekken.

>[!CAUTION]
>
>Het brede gebruik van de Sessie- en Node-interfaces van de JCR API&#39;s in een AEM toepassing is codegeur. Zorg ervoor dat [!DNL Sling] de API&#39;s niet worden gebruikt.

### Veelvoorkomende toepassingen van JCR API&#39;s

* [Toegangsbeheer](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [Toegestaan beheer (gebruikers/groepen)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR-observatie (luisteren naar JCR-gebeurtenissen)
* Diepknoopstructuren maken

   * Hoewel de verkoop-API&#39;s het maken van bronnen ondersteunen, hebben de JCR-API&#39;s gebruiksgemakmethoden in [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) en [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) die het maken van diepe structuren versnellen.

## OSGi API&#39;s

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2 Component Annotations JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Metatype Annotations JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Er is weinig overlapping tussen OSGi APIs en hoger niveau APIs (AEM, [!DNL Sling], en JCR), en de behoefte om OSGi APIs te gebruiken is zeldzaam en vereist een hoog niveau van AEM ontwikkelingsdeskundigheid.

### OSGi vs Apache Felix API&#39;s

OSGi bepaalt een specificatie alle containers OSGi moeten uitvoeren en met in overeenstemming zijn. AEM OSGi-implementatie, Apache Felix, biedt ook verschillende van zijn eigen API&#39;s.

* Voorkeur voor OSGi API&#39;s (`org.osgi`) boven Apache Felix API&#39;s (`org.apache.felix`).

### Veelvoorkomende toepassingen van OSGi API&#39;s

* OSGi-annotaties voor het declareren van OSGi-diensten en -componenten.

   * Voorkeur voor [OSGi Declarative Services (DS) 1.2 Annotaties](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Felix SCR Annotaties](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) voor declaratie van OSGi-services en -componenten

* OSGi API&#39;s voor dynamisch in-code [niet/registratie van OSGi-services/componenten](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Het gebruik van OSGi DS 1.2-annotaties verdient de voorkeur wanneer voorwaardelijk OSGi Service/Component management niet nodig is (wat meestal het geval is).

## Uitzonderingen op de regel

Hieronder volgen algemene uitzonderingen op de hierboven gedefinieerde regels.

### Elementen-API&#39;s AEM

* Voorkeur [ over `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Hoewel de API&#39;s voor `com.day.cq` middelen meer gratis tools bieden voor het AEM van gebruiksscenario&#39;s voor middelenbeheer.
   * De API&#39;s van Granite Assets ondersteunen gebruiksscenario&#39;s voor middelenbeheer op laag niveau (versie, relaties).

### Query-API&#39;s

* AEM QueryBuilder ondersteunt bepaalde queryfuncties, zoals [suggesties](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), spellingcontrole en indexhints, niet voor minder algemene functies. Om met deze functies te vragen wordt JCR-SQL2 geprefereerd.

### [!DNL Sling] Servlet-registratie {#sling-servlet-registration}

* [!DNL Sling] servlet-registratie, voorkeur [OSGi DS 1.2-annotaties met @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) boven `@SlingServlet`

### [!DNL Sling] Filterregistratie {#sling-filter-registration}

* [!DNL Sling] filterregistratie, voorkeur [OSGi DS 1.2 annotaties met @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) boven `@SlingFilter`

## Nuttige codefragmenten

Hieronder vindt u nuttige Java-codefragmenten die tips en trucs weergeven voor veelvoorkomende toepassingen met behulp van besproken API&#39;s. Deze fragmenten tonen ook hoe u van API&#39;s met minder voorkeuren naar API&#39;s met meer voorkeuren kunt gaan.

### JCR-sessie naar [!DNL Sling] ResourceResolver

#### Auto-closing Sling ResourceResolver

Sinds AEM 6.2, is [!DNL Sling] ResourceResolver `AutoClosable` in een [poging-met-middelen](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) verklaring. Met behulp van deze syntaxis is een expliciete aanroep naar `resourceResolver .close()` niet nodig.

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

ResourceResolvers kan manueel in een `finally` blok worden gesloten, als de auto-sluitende hierboven getoonde techniek niet kan worden gebruikt.

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

`DamUtil.resolveToAsset(..)`lost om het even welke middelen onder het `dam:Asset` aan het voorwerp van Activa door de boom te lopen zoals nodig.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Alternatieve benadering

Als u een bron aanpast aan een element, moet de bron zelf het `dam:Asset` knooppunt zijn.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Bron naar AEM pagina

#### Aanbevolen aanpak

`pageManager.getContainingPage(..)` Hiermee worden alle bronnen onder het paginaobject `cq:Page` opgelost door de structuur zo nodig naar boven te doorlopen.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Alternatieve benadering {#alternative-approach-1}

Als u een bron wilt aanpassen aan een pagina, moet de bron zelf het `cq:Page` knooppunt zijn.

```java
Page page = resource.adaptTo(Page.class);
```

### Eigenschappen van AEM pagina lezen

Met de getters van het object Pagina krijgt u bekende eigenschappen (`getTitle()`, `getDescription()`enz.) en `page.getProperties()` om `[cq:Page]/jcr:content` ValueMap te verkrijgen voor het ophalen van andere eigenschappen.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Eigenschappen van metagegevens van AEM element lezen

De Asset API biedt handige methoden voor het lezen van eigenschappen van het `[dam:Asset]/jcr:content/metadata` knooppunt. Merk op dat dit geen ValueMap is, wordt de tweede parameter (standaardwaarde, en auto-type het gieten) niet gesteund.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lezen, [!DNL Sling] [!DNL Resource] eigenschappen {#read-sling-resource-properties}

Wanneer eigenschappen worden opgeslagen op locaties (eigenschappen of relatieve bronnen) waartoe de AEM-API&#39;s (Pagina, Element) niet rechtstreeks toegang hebben, kunnen de [!DNL Sling] bronnen en ValueMaps worden gebruikt om de gegevens te verkrijgen.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In dit geval moet het AEM mogelijk worden omgezet in een [!DNL Sling] [!DNL Resource] bestand om de gewenste eigenschap of subbron efficiënt te vinden.

#### Pagina AEM naar [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Element AEM aan [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Eigenschappen schrijven met [!DNL Sling]s ModisibleValueMap

Gebruik [!DNL Sling]&#39;s [ModisibleValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) om eigenschappen naar knooppunten te schrijven. Dit kan alleen naar het huidige knooppunt worden geschreven (relatieve paden van eigenschappen worden niet ondersteund).

Merk op de vraag om schrijftoestemmingen aan het middel te `.adaptTo(ModifiableValueMap.class)` vereisen, anders zal het ongeldig terugkeren.

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

### Een [!DNL Sling] bron maken

ResourceResolver steunt basisverrichtingen voor het creëren van middelen. Bij het maken van abstracties op een hoger niveau (AEM pagina&#39;s, elementen, tags, enzovoort) de methoden gebruiken die door hun respectieve managers zijn verstrekt.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Een [!DNL Sling] bron verwijderen

ResourceResolver ondersteunt het verwijderen van een resource. Bij het maken van abstracties op een hoger niveau (AEM pagina&#39;s, elementen, tags, enzovoort) de methoden gebruiken die door hun respectieve managers zijn verstrekt.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
