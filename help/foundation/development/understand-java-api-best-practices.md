---
title: Java&trade; API Best practices in AEM
description: AEM is gebaseerd op een rijke open-source softwarestack die veel Java&trade; API's voor gebruik tijdens de ontwikkeling beschikbaar maakt. In dit artikel worden de belangrijkste API's besproken en wordt aangegeven wanneer en waarom deze moeten worden gebruikt.
version: Experience Manager 6.4, Experience Manager 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 416
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1726'
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

Als een API door AEM wordt verstrekt, verkies het over [!DNL Sling], JCR, en OSGi. Als AEM geen API aanbiedt, geeft u de voorkeur aan [!DNL Sling] boven JCR en OSGi.

Deze volgorde is een algemene regel, wat betekent dat er uitzonderingen bestaan. Aanvaardbare redenen om van deze regel af te wijken zijn:

* Bekende uitzonderingen, zoals hieronder beschreven.
* De vereiste functionaliteit is niet beschikbaar in een API op een hoger niveau.
* Werken in de context van bestaande code (aangepaste code of AEM-productcode) die zelf een minder voorkeursAPI gebruikt, en de kosten voor de overstap naar de nieuwe API zijn niet te rechtvaardigen.

   * Het is beter de API op een lager niveau consequent te gebruiken dan een combinatie te maken.

## AEM API&#39;s

* [**AEM API JavaDocs** ](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM API&#39;s bieden abstracties en functionaliteit die specifiek zijn voor gebruikssituaties die in productie zijn.

Bijvoorbeeld, verstrekken AEM [ PageManager ](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) en [ Pagina ](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) APIs abstracties voor `cq:Page` knopen in AEM die Web-pagina&#39;s vertegenwoordigen.

Hoewel deze knooppunten beschikbaar zijn via [!DNL Sling] API&#39;s als bronnen en JCR API&#39;s als knooppunten, bieden AEM API&#39;s abstracties voor veelvoorkomende gebruiksgevallen. Het gebruik van de AEM API&#39;s zorgt voor een consistent gedrag tussen AEM en het product, en aanpassingen en extensies voor AEM.

### com.adobe.&#42; vs com.day.&#42; API&#39;s

AEM API&#39;s hebben een voorkeur voor andere pakketten, die wordt aangegeven door de volgende Java™-pakketten, in volgorde van voorkeur:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

Het `com.adobe.cq` -pakket ondersteunt productgebruikscenario&#39;s, terwijl `com.adobe.granite` gebruiksscenario&#39;s voor verschillende productplatforms ondersteunt, zoals workflows of taken (die voor alle producten worden gebruikt: AEM Assets, Sites, enzovoort).

Het `com.day.cq` -pakket bevat &#39;originele&#39; API&#39;s. Deze API&#39;s bieden oplossingen voor basisabstracties en -functies die bestonden vóór en/of rondom de overname van [!DNL Day CQ] door Adobe. Deze API&#39;s worden ondersteund en moeten worden vermeden, tenzij `com.adobe.cq` - of `com.adobe.granite` -pakketten GEEN (nieuwer) alternatief bieden.

Nieuwe abstracties zoals [!DNL Content Fragments] en [!DNL Experience Fragments] worden in de `com.adobe.cq` -ruimte opgebouwd in plaats van `com.day.cq` hieronder.

### Query-API&#39;s

AEM ondersteunt meerdere querytalen. De drie belangrijkste talen zijn [ JCR-SQL2 ](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath, en [ de Bouwer van de Vraag van AEM ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=nl-NL).

De belangrijkste zorg is het handhaven van een verenigbare vraagtaal over de codebasis, om ingewikkeldheid en kosten te verminderen om te begrijpen.

Alle vraagtalen hebben in feite de zelfde prestatiesprofielen, aangezien [!DNL Apache Oak] hen aan JCR-SQL2 voor definitieve vraaguitvoering overstapt, en de omzettingstijd aan JCR-SQL2 verwaarloosbaar in vergelijking met de vraagtijd zelf is.

De aangewezen API is [ de Bouwer van de Vraag van AEM ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=nl-NL), die de hoogste niveauabstractie is en robuuste API voor het construeren, het uitvoeren van, en het terugwinnen van resultaten voor vragen verstrekt, en het volgende verstrekt:

* Eenvoudige, parameterized vraagbouw (vraagparams die als Kaart worden gemodelleerd)
* Eigen [ Java™ API en HTTP APIs ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=nl-NL)
* [ Debugger van de Vraag van AEM ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=nl-NL)
* [ AEM voorspelt ](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html?lang=nl-NL) ondersteunend gemeenschappelijke vraagvereisten

* Uitbreidbare API, die voor de ontwikkeling van douane [ vraagpredikaten ](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=nl-NL) toestaat
* JCR-SQL2 en XPath kunnen direct via [[!DNL Sling] worden uitgevoerd ](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) en [ JCR APIs ](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), terugkerend resultaten a [[!DNL Sling]  Middelen ](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) of [ Knooppunten JCR ](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), respectievelijk.

>[!CAUTION]
>
>AEM QueryBuilder-API lekt een ResourceResolver-object. Om dit lek te verlichten, volg dit [ codesteekproef ](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## [!DNL Sling] API&#39;s

* [**Apache [!DNL Sling] API JavaDocs** ](https://sling.apache.org/apidocs/sling10/)

[ Apache  [!DNL Sling] ](https://sling.apache.org/) is het RESTful Webkader dat AEM steunt. [!DNL Sling] verstrekt HTTP- verzoek het verpletteren, modelleert knopen JCR als middelen, verstrekt veiligheidscontext, en veel meer.

API&#39;s van [!DNL Sling] hebben het extra voordeel dat ze kunnen worden gemaakt voor extensies. Dit betekent dat het vaak eenvoudiger en veiliger is om het gedrag van toepassingen die met [!DNL Sling] API&#39;s zijn gemaakt, te verbeteren dan van minder uitbreidbare JCR API&#39;s.

### Veelvoorkomende toepassingen van API&#39;s van [!DNL Sling]

* De toegang tot van knopen JCR als [[!DNL Sling Resources] ](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) en de toegang tot van hun gegevens via [ ValueMaps ](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Het verstrekken van veiligheidscontext via [ ResourceResolver ](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Creërend en verwijderend middelen via ResourceResolver [ creeer/beweeg/exemplaar/schrapt methodes ](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Het bijwerken eigenschappen via [ ModifyingValueMap ](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Bouwstenen voor aanvraagverwerking maken

   * [ Servlets ](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [ de Filters van Servlet ](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Asynchrone bouwstenen voor werkverwerking

   * [ Gebeurtenis en de Handlers van de Baan ](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [ Planner ](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [ Sling Models ](https://sling.apache.org/documentation/bundles/models.html)

* [ de gebruikers van de Dienst ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=nl-NL)

## JCR-API&#39;s

* **[JCR 2.0 JavaDocs ](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[ JCR (de Bewaarplaats van de Inhoud Java™) 2.0 APIs ](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) maakt deel uit van een specificatie voor de implementaties van JCR (in het geval van AEM, [ Apache Jackrabbit Oak ](https://jackrabbit.apache.org/oak/docs/)). Alle JCR-implementatie moet in overeenstemming zijn met deze API&#39;s en deze implementeren. Dit is dus de API op het laagste niveau voor interactie met AEM-inhoud.

De JCR zelf is een hiërarchische/boomgebaseerde NoSQL datastore AEM gebruikt als opslagplaats voor inhoud. De JCR heeft een uitgebreide reeks ondersteunde API&#39;s, variërend van inhoud-CRUD tot het opvragen van inhoud. Ondanks deze robuuste API hebben ze maar zelden de voorkeur boven AEM en [!DNL Sling] -abstracties op een hoger niveau.

Geef altijd de voorkeur aan de JCR API&#39;s boven de Apache Jackrabbit Oak API&#39;s. JCR APIs is voor ***het in wisselwerking staan*** met een bewaarplaats JCR, terwijl Oak APIs voor ***het uitvoeren van*** een bewaarplaats JCR is.

### Algemene misvattingen over JCR API&#39;s

Hoewel de JCR een AEM-opslagplaats voor inhoud is, hebben de API&#39;s NIET de voorkeursmethode voor interactie met de inhoud. In plaats daarvan geeft u liever de API&#39;s van AEM (Pagina, Assets, Tag enzovoort) of Sling Resource API&#39;s, omdat deze een betere abstractie bieden.

>[!CAUTION]
>
>Het brede gebruik van de Sessie- en Node-interfaces van de JCR API&#39;s in een AEM-toepassing is codegeur. Zorg ervoor dat in plaats daarvan [!DNL Sling] -API&#39;s worden gebruikt.

### Veelvoorkomende toepassingen van JCR API&#39;s

* [ Toegangsbeheer ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=nl-NL)
* [ Toegelaten beheer (gebruikers/groepen) ](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR-observatie (luisteren naar JCR-gebeurtenissen)
* Diepknoopstructuren maken

   * Terwijl het Verdelen APIs de verwezenlijking van middelen steunt, heeft JCR APIs gemakmethodes in [ JcrUtils ](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) en [ JcrUtil ](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) die het creëren van diepe structuren versnellen.

## OSGi API&#39;s

* [**OSGi R6 JavaDocs** ](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Verklarende Diensten 1.2 de Annotaties JavaDocs van de Component ](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi de Verklarende Diensten 1.2 de Annotaties JavaDocs van Metatype ](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Kader JavaDocs** ](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Er is weinig overlapping tussen de OSGi API&#39;s en de API&#39;s op hoger niveau (AEM, [!DNL Sling] en JCR), en de noodzaak om OSGi API&#39;s te gebruiken is zeldzaam en vereist een hoog niveau van AEM-ontwikkelingsexpertise.

### OSGi vs Apache Felix API&#39;s

OSGi bepaalt een specificatie alle containers OSGi moeten uitvoeren en met in overeenstemming zijn. Apache Felix, een AEM OSGi-implementatie, biedt ook verschillende eigen API&#39;s.

* Voorkeur voor OSGi APIs (`org.osgi`) over Apache Felix APIs (`org.apache.felix`).

### Veelvoorkomende toepassingen van OSGi API&#39;s

* OSGi-annotaties voor het declareren van OSGi-diensten en -componenten.

   * Voorkeur [ OSGi de Verklarende Diensten (DS) 1.2 Annotaties ](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [ Felix SCR Annotations ](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) voor het verklaren van de diensten OSGi en componenten

* OSGi APIs voor dynamisch in-code [ un/registering OSGi diensten/componenten ](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Het gebruik van OSGi DS 1.2-annotaties verdient de voorkeur wanneer voorwaardelijk OSGi Service/Component management niet nodig is (wat meestal het geval is).

## Uitzonderingen op de regel

Hieronder volgen algemene uitzonderingen op de hierboven gedefinieerde regels.

### OSGi API&#39;s

Wanneer u werkt met OSGi-abstracties op laag niveau, zoals het definiëren of lezen in eigenschappen van OSGi-componenten, hebben de nieuwere abstracties die door `org.osgi` worden geboden de voorkeur boven Sling-abstracties op hoger niveau. De concurrerende Abstracties van het Sling zijn niet gemerkt als `@Deprecated` en suggereren het `org.osgi` alternatief.

Let ook op dat de definitie van de OSGi-configuratienode de voorkeur geeft aan `cfg.json` boven de `sling:OsgiConfig` -indeling.

### AEM Asset-API&#39;s

* De voorkeur [`com.day.cq.dam.api` ](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) over [`com.adobe.granite.asset.api` ](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Hoewel de Assets API&#39;s van `com.day.cq` meer gratis tools bieden voor het gebruik van AEM-middelen voor middelenbeheer.
   * De Assets API&#39;s van Granite ondersteunen gebruiksscenario&#39;s voor middelenbeheer op laag niveau (versie, relaties).

### Query-API&#39;s

* AEM QueryBuilder steunt bepaalde vraagfuncties zoals [ suggesties ](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), spellcheck, en indexwenken onder andere minder gemeenschappelijke functies niet. Om met deze functies te vragen wordt JCR-SQL2 geprefereerd.

### [!DNL Sling] Serverregistratie {#sling-servlet-registration}

* [!DNL Sling] servlet registratie, verkies [ OSGi DS 1.2 aantekeningen met @SlingServletResourceTypes ](https://sling.apache.org/documentation/the-sling-engine/servlets.html) over `@SlingServlet`

### [!DNL Sling] Filterregistratie {#sling-filter-registration}

* [!DNL Sling] filterregistratie, verkies [ OSGi DS 1.2 aantekeningen met @SlingServletFilter ](https://sling.apache.org/documentation/the-sling-engine/filters.html) over `@SlingFilter`

## Nuttige codefragmenten

Hieronder vindt u handige Java™-codefragmenten die tips en trucs weergeven voor veelvoorkomende toepassingen met behulp van besproken API&#39;s. Deze fragmenten tonen ook hoe u van API&#39;s met minder voorkeuren naar API&#39;s met meer voorkeuren kunt gaan.

### JCR-sessie naar [!DNL Sling] ResourceResolver

#### Auto-closing Sling ResourceResolver

Sinds AEM 6.2, is [!DNL Sling] ResourceResolver `AutoClosable` in a [ probeer-met-middelen ](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) verklaring. Met deze syntaxis is een expliciete aanroep van `resourceResolver .close()` niet nodig.

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

ResourceResolvers kan handmatig worden gesloten in een `finally` -blok als de hierboven getoonde techniek voor automatisch sluiten niet kan worden gebruikt.

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

### [!DNL Sling] [!DNL Resource] naar AEM-element

#### Aanbevolen aanpak

De functie `DamUtil.resolveToAsset(..)` lost om het even welke middelen onder `dam:Asset` aan het voorwerp van Activa op door de boom naar boven te lopen zoals nodig.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Alternatieve benadering

Als u een bron wilt aanpassen aan een element, moet de bron zelf het knooppunt `dam:Asset` zijn.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Bron naar AEM-pagina

#### Aanbevolen aanpak

`pageManager.getContainingPage(..)` lost om het even welke middelen onder `cq:Page` aan het voorwerp van de Pagina door de boom te lopen zoals nodig.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Alternatieve benadering {#alternative-approach-1}

Als u een bron wilt aanpassen aan een pagina, moet de bron zelf het knooppunt `cq:Page` zijn.

```java
Page page = resource.adaptTo(Page.class);
```

### Eigenschappen van AEM-pagina lezen

Gebruik de getters van het object Page om bekende eigenschappen (`getTitle()` , `getDescription()` , enzovoort) en `page.getProperties()` op te halen om `[cq:Page]/jcr:content` ValueMap te verkrijgen voor het ophalen van andere eigenschappen.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Eigenschappen van metagegevens van AEM-middelen lezen

De API voor middelen biedt handige methoden voor het lezen van eigenschappen van het knooppunt `[dam:Asset]/jcr:content/metadata` . Dit is geen ValueMap, de tweede parameter (standaardwaarde en automatisch gegoten casting) wordt niet ondersteund.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Eigenschappen van het type Read [!DNL Sling] [!DNL Resource] {#read-sling-resource-properties}

Wanneer eigenschappen worden opgeslagen op locaties (eigenschappen of relatieve bronnen) waartoe de AEM API&#39;s (Pagina, Element) niet rechtstreeks toegang hebben, kunnen de bronnen van [!DNL Sling] en ValueMaps worden gebruikt om de gegevens te verkrijgen.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In dit geval moet het AEM-object mogelijk worden omgezet in een [!DNL Sling] [!DNL Resource] om de gewenste eigenschap of subbron efficiënt te kunnen vinden.

#### AEM-pagina naar [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM-element naar [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Eigenschappen schrijven met de functie ModisibleValueMap van [!DNL Sling]

Het gebruik [ ModifyingValueMap ](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) van [!DNL Sling] &lbrace;om eigenschappen aan knopen te schrijven. Dit kan alleen naar het huidige knooppunt worden geschreven (relatieve paden van eigenschappen worden niet ondersteund).

De aanroep van `.adaptTo(ModifiableValueMap.class)` vereist schrijfmachtigingen naar de bron, anders wordt null geretourneerd.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Een AEM-pagina maken

Gebruik altijd PageManager om pagina&#39;s tot stand te brengen aangezien het een Malplaatje van de Pagina neemt, wordt vereist om Pagina&#39;s in AEM behoorlijk te bepalen en te initialiseren.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Een [!DNL Sling] -bron maken

ResourceResolver steunt basisverrichtingen voor het creëren van middelen. Bij het maken van abstracties op een hoger niveau (AEM Pages, Assets, Tags, enzovoort) gebruikt u de methoden die door de respectievelijke managers worden geboden.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Een [!DNL Sling] -bron verwijderen

ResourceResolver ondersteunt het verwijderen van een resource. Bij het maken van abstracties op een hoger niveau (AEM Pages, Assets, Tags, enzovoort) gebruikt u de methoden die door de respectievelijke managers worden geboden.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
