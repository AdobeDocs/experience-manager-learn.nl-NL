---
title: Eenheid testen
description: Voer een eenheidstest uit die het gedrag van het het Verzamelen Model van de component van de Byte bevestigt, dat in het leerprogramma van de Component van de Douane wordt gecreeerd.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4089
mini-toc-levels: 1
thumbnail: 30207.jpg
doc-type: Tutorial
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
duration: 706
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# Eenheid testen {#unit-testing}

Dit leerprogramma behandelt de implementatie van een eenheidstest die het gedrag van het het Schuiven Model van de component van de Byline bevestigt, dat in het [&#128279;](./custom-component.md) leerprogramma van de Component van de 0&rbrace; Douane &lbrace;wordt gecreeerd.

## Vereisten {#prerequisites}

Herzie het vereiste tooling en de instructies voor vestiging a [ lokale ontwikkelomgeving ](overview.md#local-dev-environment).

_als zowel Java™ 8 als Java™ 11 op het systeem geïnstalleerd zijn, kan de de testlooper van de Code van VS lagere runtime van Java™ selecteren wanneer het uitvoeren van de tests, resulterend in testmislukkingen. Als dit voorkomt, desinstalleer Java™ 8._

### Starter-project

>[!NOTE]
>
> Als u met succes het vorige hoofdstuk voltooide, kunt u het project opnieuw gebruiken en de stappen overslaan voor het uitchecken van het starterproject.

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Controle uit de `tutorial/unit-testing-start` tak van [ GitHub ](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Stel codebasis aan een lokale instantie van AEM op gebruikend uw Maven vaardigheden:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Als u AEM 6.5 of 6.4 gebruikt, voegt u het `classic` -profiel toe aan Maven-opdrachten.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

U kunt de gebeëindigde code op [ GitHub ](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) altijd bekijken of de code plaatselijk controleren door aan de tak `tutorial/unit-testing-start` te schakelen.

## Doelstelling

1. Begrijp de grondbeginselen van eenheidstests.
1. Meer informatie over raamwerken en gereedschappen die vaak worden gebruikt om AEM-code te testen.
1. Begrijp opties voor het verplaatsen of simuleren van AEM-bronnen bij het schrijven van eenheidstests.

## Achtergrond {#unit-testing-background}

In dit leerprogramma, zullen wij onderzoeken hoe te om [ Tests van de Eenheid ](https://en.wikipedia.org/wiki/Unit_testing) voor 2&rbrace; Sling Model van onze component Byline [&#128279;](https://sling.apache.org/documentation/bundles/models.html) te schrijven (gecreeerd in [ Creërend een Component van douaneAEM ](custom-component.md)).  Eenheidstests zijn in Java™ geschreven ontwikkeltijdstests die het verwachte gedrag van Java™-code controleren. Elke eenheidstest is doorgaans klein en valideert de uitvoer van een methode (of werkeenheden) aan de hand van de verwachte resultaten.

We gebruiken best practices van AEM en maken gebruik van:

* [ JUnit 5 ](https://junit.org/junit5/)
* [ Mockito het Testen Kader ](https://site.mockito.org/)
* [ wcm.io het Kader van de Test ](https://wcm.io/testing/) (dat op [ Apache bouwt Sling Mocks ](https://sling.apache.org/documentation/development/sling-mock.html))

## Unit Testing and Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[ Adobe Cloud Manager ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html) integreert de uitvoering van de eenheidstest en [ codedekking rapporterend ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html) in zijn pijpleiding CI/CD helpen de beste praktijken van eenheid bevorderen die de code van AEM testen.

Hoewel het testen van eenheden een goede praktijk voor om het even welke codebasis is, is het belangrijk om bij het gebruiken van Cloud Manager uit zijn het testen en rapporteringsfaciliteiten van de codekwaliteit voordeel te halen door eenheidstests voor Cloud Manager te verstrekken om te lopen.

## Werk de test Geweven gebiedsdelen bij {#inspect-the-test-maven-dependencies}

De eerste stap is Geweven gebiedsdelen te inspecteren om het schrijven en het runnen van de tests te steunen. Er zijn vier vereiste afhankelijkheden:

1. JUnit5
1. Mockito Test Framework
1. Apache Sling Mocks
1. AEM Mocks Test Framework (door io.wcm)

**JUnit5**, **Mockito, en &#x200B;** AEM Mocks** testgebiedsdelen worden automatisch toegevoegd aan het project tijdens opstelling gebruikend [ AEM Maven archetype ](project-setup.md).

1. Om deze gebiedsdelen te bekijken, open POM van de Bovenliggende Reactor in **aem-guides-wknd/pom.xml**, navigeer aan `<dependencies>..</dependencies>` en bekijk de gebiedsdelen voor JUnit, Mockito, Apache Sling Mocks, en de Tests van het Mock van AEM door io.wcm onder `<!-- Testing -->`.
1. Zorg ervoor dat `io.wcm.testing.aem-mock.junit5` aan **4.1.0** wordt geplaatst:

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > Archetype **35** produceert het project met `io.wcm.testing.aem-mock.junit5` versie **4.1.8**. Gelieve te degraderen aan **4.1.0** om de rest van dit hoofdstuk te volgen.

1. Open **aem-guides-wknd/core/pom.xml** en mening dat de overeenkomstige het testen gebiedsdelen beschikbaar zijn.

   Een parallelle bronomslag in het **kern** project zal de eenheidstests en om het even welke ondersteunende testdossiers bevatten. Deze **test** omslag verstrekt scheiding van testklassen van de broncode maar staat de tests toe om te handelen alsof zij in de zelfde pakketten zoals de broncode leven.

## De JUnit-test maken {#creating-the-junit-test}

De tests van de eenheid brengen typisch 1-aan-1 met klassen Java™ in kaart. In dit hoofdstuk, zullen wij een test JUnit voor **BylineImpl.java** schrijven, die het het Verlenen Model is dat de component van de Naamregel steunt.

![ de testsrc van de Eenheid omslag ](assets/unit-testing/core-src-test-folder.png)

*Plaats waar de tests van de Eenheid worden opgeslagen.*

1. Maak een eenheidstest voor de `BylineImpl.java` door een nieuwe Java™-klasse onder `src/test/java` te maken in een Java™-pakketmapstructuur die de locatie van de te testen Java™-klasse weerspiegelt.

   ![ creeer een nieuw BylineImplTest.java- dossier ](assets/unit-testing/new-bylineimpltest.png)

   Aangezien we testen

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   een overeenkomende eenheidstest voor een Java™-klasse maken op

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   Het achtervoegsel `Test` in het testbestand voor eenheden, `BylineImplTest.java` , is een conventie, waarmee we

   1. Identificeer het gemakkelijk als testdossier _voor_ `BylineImpl.java`
   1. Maar ook, onderscheidt het testdossier _van_ de klasse die wordt getest, `BylineImpl.java`

## BylineImplTest.java evalueren {#reviewing-bylineimpltest-java}

Het JUnit-testbestand is nu een lege Java™-klasse.

1. Werk het bestand bij met de volgende code:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import static org.junit.jupiter.api.Assertions.*;
   
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   
   public class BylineImplTest {
   
       @BeforeEach
       void setUp() throws Exception {
   
       }
   
       @Test 
       void testGetName() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testGetOccupations() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testIsEmpty() { 
           fail("Not yet implemented");
       }
   }
   ```

1. De eerste methode `public void setUp() { .. }` wordt voorzien van een annotatie met JUnit `@BeforeEach` , die de JUnit-testruntime de instructie geeft deze methode uit te voeren voordat elke testmethode in deze klasse wordt uitgevoerd. Dit verstrekt een handige plaats om gemeenschappelijke het testen staat te initialiseren die door alle tests wordt vereist.

1. De volgende methoden zijn de testmethoden, waarvan de namen worden voorafgegaan door `test` volgens conventie en worden gemarkeerd met de `@Test` -annotatie. U ziet dat al onze tests standaard mislukken, omdat we ze nog niet hebben uitgevoerd.

   Om te beginnen, beginnen wij met één enkele testmethode voor elke openbare methode op de klasse wij testen, zo:

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | wordt getest door | testGetName() |
   | getOccupations() | wordt getest door | testGetOccupations() |
   | isEmpty() | wordt getest door | testIsEmpty() |

   Deze methoden kunnen zo nodig worden uitgebreid, zoals verderop in dit hoofdstuk wordt weergegeven.

   Wanneer deze JUnit-testklasse (ook wel JUnit Test Case genoemd) wordt uitgevoerd, wordt elke met `@Test` gemarkeerde methode uitgevoerd als een test die kan slagen of afbreken.

![ geproduceerde BylineImplTest ](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Stel het Geval van de Test JUnit in werking door op het `BylineImplTest.java` dossier met de rechtermuisknop te klikken, en het tikken **Looppas**.
Zoals verwacht, mislukken alle tests, omdat ze nog niet zijn uitgevoerd.

   ![ Looppas als junit test ](assets/unit-testing/run-junit-tests.png)

   *klik op BylineImplTests.java met de rechtermuisknop aan > Looppas*

## BylineImpl.java evalueren {#reviewing-bylineimpl-java}

Bij het schrijven van eenheidstests zijn er twee primaire benaderingen:

* [ TDD of de Gedreven Ontwikkeling van de Test ](https://en.wikipedia.org/wiki/Test-driven_development), die impliceert het schrijven van de eenheidstests incrementeel, onmiddellijk alvorens de implementatie wordt ontwikkeld; schrijf een test, schrijf de implementatie om de testpas te maken.
* Implementatie-eerste Ontwikkeling, die het ontwikkelen van werkende code eerst en dan het schrijven tests impliceert die genoemde code bevestigen.

In dit leerprogramma, wordt de laatstgenoemde benadering gebruikt (aangezien wij reeds een werkende **BylineImpl.java** in een vorig hoofdstuk hebben gecreeerd). Daarom moeten we het gedrag van de openbare methoden van het systeem, maar ook een aantal van de toepassingsdetails ervan, herzien en begrijpen. Dit kan tegendeel klinken, aangezien een goede test alleen de inputs en outputs moet betreffen, maar wanneer men in AEM werkt, zijn er verschillende implementatieoverwegingen die moeten worden begrepen voor het opstellen van werktests.

TDD in de context van AEM vereist een expertiseniveau en kan het best worden toegepast door AEM-ontwikkelaars die deskundig zijn op het gebied van de ontwikkeling van AEM en het testen van eenheden van AEM-code.

## AEM-testcontext instellen  {#setting-up-aem-test-context}

De meeste code die voor AEM wordt geschreven, is gebaseerd op JCR-, Sling- of AEM-API&#39;s, waarvoor op hun beurt de context van een AEM die wordt uitgevoerd, correct moet worden uitgevoerd.

Aangezien eenheidstests bij bouwstijl, buiten de context van een lopende instantie van AEM worden uitgevoerd, is er geen dergelijke context. Om dit te vergemakkelijken, [&#128279;](https://wcm.io/testing/aem-mock/usage.html) van AEM Mocks van 0&rbrace; wcm.io creeert mock context die deze APIs __ toestaat dienst alsof zij in AEM lopen.

1. Creeer een context van AEM gebruikend **wcm.io `AemContext` in** BylineImplTest.java **door het toe te voegen als uitbreiding JUnit die met `@ExtendWith` aan het {** dossier 6} wordt versierd BylineImplTest.java. **&#x200B;**&#x200B;De extensie zorgt voor alle vereiste initialisatie- en opschoningstaken. Maak een klassevariabele voor `AemContext` die voor alle testmethoden kan worden gebruikt.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Deze variabele, `ctx` , maakt een modelcontext van AEM beschikbaar die enkele abstracties van AEM en Sling biedt:

   * Het BylineImpl-opmaakmodel is in deze context geregistreerd
   * In deze context worden structuur voor JCR-inhoud van Mock gemaakt
   * Aangepaste OSGi-services kunnen in deze context worden geregistreerd
   * Verschillende veelvoorkomende vereiste modelobjecten en hulpfuncties, zoals SlingHttpServletRequest-objecten, diverse modellen Sling en AEM OSGi-services, zoals ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag, enzovoort.
      * *niet alle methodes voor deze voorwerpen worden uitgevoerd!*
   * En [ veel meer ](https://wcm.io/testing/aem-mock/usage.html)!

   Het **`ctx`** -object fungeert als ingangspunt voor het grootste deel van onze modelcontext.

1. Definieer in de methode `setUp(..)` , die wordt uitgevoerd vóór elke methode `@Test` , een algemene status voor het testen van modellen:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registreert het te testen Sling Model in de modelcontext van AEM, zodat kan het in de `@Test` methodes worden geconcretiseerd.
   * **`load().json`** laadt bronstructuren in de modelcontext, die de code toestaat om met deze middelen in wisselwerking te staan alsof zij door een echte bewaarplaats werden verstrekt. De middeldefinities in het dossier **`BylineImplTest.json`** worden geladen in de mockJCR context onder **/content**.
   * **`BylineImplTest.json`** bestaat nog niet, dus laten we het maken en de JCR-bronstructuren definiëren die nodig zijn voor de test.

1. De JSON- dossiers die de modelmiddelstructuren vertegenwoordigen worden opgeslagen onder **kern/src/test/resources** na het zelfde pakket dat als JUnit Java™ testdossier plakt.

   Creeer een JSON- dossier bij `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` genoemd **BylineImplTest.json** met de volgende inhoud:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![ BylineImplTest.json ](assets/unit-testing/bylineimpltest-json.png)

   Deze JSON definieert een modelbron (JCR-knooppunt) voor de eenheidstest van de Byline-component. Op dit punt heeft de JSON de minimale set eigenschappen die vereist is om een inhoudsbron van een component Byline, de `jcr:primaryType` en `sling:resourceType`, te vertegenwoordigen.

   Een algemene regel bij het werken met eenheidstests is het maken van de minimale set met mock-inhoud, -context en -code die nodig is om aan elke test te voldoen. Vermijd de verleiding om een complete mock-context op te bouwen voordat de tests worden geschreven, aangezien dit vaak tot ongewenste artefacten leidt.

   Nu met het bestaan van **BylineImplTest.json**, wanneer `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` wordt uitgevoerd, worden de modelmiddeldefinities geladen in de context bij de weg **/content.**

## getName() testen {#testing-get-name}

Nu wij een basismodelcontextopstelling hebben, schrijven wij onze eerste test voor **BylineImpl getName ()**. Deze test moet de methode **getName () verzekeren** keert de correct-geschreven naam terug die bij het 2&rbrace; wordt opgeslagen naam van het middel &quot;**bezit.**

1. Werk **testGetName** () methode in **BylineImplTest.java** als volgt bij:

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** stelt de verwachte waarde in. Wij zullen dit aan &quot;**Gedaan Jane** plaatsen.
   * **`ctx.currentResource`** plaatst de context van het modelmiddel om de code tegen te evalueren, zodat wordt dit geplaatst aan **/content/byline** aangezien dat is waar de bron van de modelinhoud wordt geladen.
   * **`Byline byline`** instantieert het Byline Sling Model door het van het modelvoorwerp van het Verzoek aan te passen.
   * **`String actual`** roept de methode aan die we testen, `getName()` , op het Byline Sling Model-object.
   * **`assertEquals`** geeft aan dat de verwachte waarde overeenkomt met de waarde die wordt geretourneerd door het byline Sling Model-object. Als deze waarden niet gelijk zijn, zal de test mislukken.

1. Voer de test uit... en deze mislukt met een `NullPointerException` .

   Deze test mislukt NIET omdat we nooit een eigenschap `name` hebben gedefinieerd in de mock JSON, waardoor de test mislukt, maar de uitvoering van de test is op dat punt niet gekomen! Deze test mislukt als gevolg van een `NullPointerException` op het byline-object zelf.

1. Als `@PostConstruct init()` in `BylineImpl.java` een uitzondering genereert, voorkomt dit dat het Sling Model wordt geïnstantieerd en wordt de waarde van dat Sling Model-object null.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Hoewel de OSGi-service ModelFactory via `AemContext` (als Apache Sling-context) wordt aangeboden, worden niet alle methoden geïmplementeerd, inclusief `getModelFromWrappedRequest(...)` die in de BylineImpl-methode `init()` wordt aangeroepen. Dit resulteert in een [ AbstractMethodError ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), die in termijn `init()` om veroorzaakt te ontbreken, en de resulterende aanpassing van `ctx.request().adaptTo(Byline.class)` is een ongeldig voorwerp.

   Aangezien de geleverde modellen onze code niet kunnen aanpassen, moeten wij de modelcontext zelf uitvoeren voor dit, kunnen wij Mockito gebruiken om een modelvoorwerp te creëren ModelFactory, dat een modelvoorwerp van het Beeld terugkeert wanneer `getModelFromWrappedRequest(...)` op het wordt aangehaald.

   Omdat deze modelcontext aanwezig moet zijn om zelfs maar een instantie van het Byline Sling-model te kunnen maken, kunnen we deze toevoegen aan de methode `@Before setUp()` . Wij moeten ook `MockitoExtension.class` aan de `@ExtendWith` annotation boven de **BylineImplTest** klasse toevoegen.

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** merkt de klasse van het Geval van de Test die met de [ Uitbreiding Jupiter van het Mockito JUnit ](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html) moet worden in werking gesteld die voor het gebruik van de @Mock annotaties toestaat om mock voorwerpen op het niveau van de Klasse te bepalen.
   * **`@Mock private Image`** maakt een mock-object van het type `com.adobe.cq.wcm.core.components.models.Image` . Dit is gedefinieerd op klasseniveau, zodat `@Test` -methoden zo nodig hun gedrag kunnen wijzigen.
   * **`@Mock private ModelFactory`** maakt een modelobject van het type ModelFactory. Dit is een puur Mockito-mock en er zijn geen methoden op toegepast. Dit wordt bepaald op het klassenniveau zodat, zoals nodig, `@Test` de methodes zijn gedrag kunnen veranderen zoals nodig.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registreert het modelgedrag voor wanneer `getModelFromWrappedRequest(..)` wordt aangeroepen voor het modelobject ModelFactory. Het resultaat dat in `thenReturn (..)` wordt gedefinieerd, is het retourneren van het modelafbeeldingsobject. Dit gedrag wordt alleen aangeroepen wanneer: de eerste parameter is gelijk aan het request-object van `ctx` , de tweede parameter een Resource-object is en de derde parameter de Core Components Image-klasse moet zijn. Wij aanvaarden om het even welk Middel omdat door onze tests wij `ctx.currentResource(...)` aan diverse modelmiddelen plaatsen die in **worden bepaald BylineImplTest.json**. Merk op dat wij **lenient ()** strengheid toevoegen omdat wij later dit gedrag van ModelFactory zullen willen met voeten treden.
   * **`ctx.registerService(..)`.** registreert het modelobject ModelFactory in AemContext, met de hoogste de dienstrangschikking. Dit is vereist omdat de ModelFactory die in BylineImpl `init()` wordt gebruikt via het `@OSGiService ModelFactory model` gebied wordt ingespoten. Voor AemContext om **ons** modelvoorwerp te injecteren, dat vraag aan `getModelFromWrappedRequest(..)` behandelt, moeten wij het als hoogste rangschikkende Dienst van dat type (ModelFactory) registreren.

1. Voer de test opnieuw uit, en nogmaals, het mislukt, maar dit keer is de boodschap duidelijk waarom de test is mislukt.

   ![ bevestiging van de mislukking van de testnaam ](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName () mislukking toe te schrijven aan bewering*

   Wij ontvangen een **AssertionError** wat de bevestigingsvoorwaarde in de ontbroken test betekent, en het vertelt ons de **verwachte waarde is &quot;Jansen&quot;** maar de **daadwerkelijke waarde is ongeldig**. Dit maakt steek omdat het &quot;**naam&quot;** bezit niet aan mock **/content/byline** middeldefinitie in **BylineImplTest.json** is toegevoegd, zo laat het toevoegen:

1. Update **BylineImplTest.json** om te bepalen `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Voer de test opnieuw uit en **`testGetName()`** gaat nu door!

   ![ de omslag van de testnaam ](assets/unit-testing/testgetname-pass.png)


## getOccupations() testen {#testing-get-occupations}

Ok, geweldig! De eerste test is geslaagd! Laten we verder gaan en `getOccupations()` testen. Aangezien de initialisatie van de modelcontext in de `@Before setUp()` methode werd gedaan, is dit beschikbaar aan alle `@Test` methodes in dit Geval van de Test, met inbegrip van `getOccupations()`.

Onthoud dat deze methode een alfabetisch gesorteerde lijst met beroepen (aflopend) moet retourneren die is opgeslagen in de eigenschap bezettingen.

1. **`testGetOccupations()`** als volgt bijwerken:

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** definieert het verwachte resultaat.
   * **`ctx.currentResource`** plaatst het huidige middel om de context tegen de modelmiddeldefinitie bij /content/byline te evalueren. Dit verzekert **BylineImpl.java** in de context van ons modelmiddel uitvoert.
   * **`ctx.request().adaptTo(Byline.class)`** instantieert het Byline Sling Model door het van het modelvoorwerp van het Verzoek aan te passen.
   * **`byline.getOccupations()`** roept de methode aan die we testen, `getOccupations()` , op het Byline Sling Model-object.
   * **`assertEquals(expected, actual)`** beweert dat de verwachte lijst gelijk is aan de werkelijke lijst.

1. Herinner me, enkel als **`getName()`** hierboven, **BylineImplTest.json** geen beroepen bepaalt, zodat zal deze test ontbreken als wij het in werking stellen, aangezien `byline.getOccupations()` een lege lijst zal terugkeren.

   Werk **BylineImplTest.json** bij om een lijst van beroepen te omvatten, en zij worden geplaatst in niet alfabetische orde om ervoor te zorgen dat onze tests bevestigen dat de beroepen alfabetisch door **`getOccupations()`** worden gesorteerd.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. Voer de test uit, en nogmaals, we slagen! Het lijkt alsof de gesorteerde beroepen werken!

   ![ krijgt de Bezettingspas ](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations () gaat over*

## Testing isEmpty() {#testing-is-empty}

De laatste methode om **`isEmpty()`** te testen.

Het testen `isEmpty()` is interessant omdat het testen op verschillende voorwaarden vereist. Het herzien van **BylineImpl.java** `isEmpty()` methode de volgende voorwaarden moeten worden getest:

* Retourneer true wanneer de naam leeg is
* Retourneer true wanneer de bezettingen null of leeg zijn
* Retourneer true wanneer de afbeelding null is of geen src-URL heeft
* Retourneer false wanneer de naam, de bezetting en de afbeelding (met een URL voor de bron) aanwezig zijn

Hiervoor moeten we testmethoden maken, waarbij elke test een specifieke voorwaarde en nieuwe modelbronstructuren in `BylineImplTest.json` test om deze tests te starten.

Met deze controle konden we het testen overslaan als `getName()` , `getOccupations()` en `getImage()` leeg zijn, omdat het verwachte gedrag van die status via `isEmpty()` wordt getest.

1. De eerste test zal de voorwaarde van een gloednieuwe component testen, die geen eigenschappen heeft geplaatst.

   Voeg een nieuwe middeldefinitie aan `BylineImplTest.json` toe, die het de semantische naam &quot;**leeg** geeft&quot;

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** definieert een nieuwe brondefinitie met de naam &#39;empty&#39; die alleen een `jcr:primaryType` en `sling:resourceType` heeft.

   Vergeet niet dat we `BylineImplTest.json` in `ctx` laden voordat elke testmethode in `@setUp` wordt uitgevoerd. Deze nieuwe brondefinitie is dus direct beschikbaar voor ons in tests op **/content/empty.**

1. Werk `testIsEmpty()` als volgt bij, plaatsend het huidige middel aan nieuwe &quot;**lege**&quot;modelmiddeldefinitie.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Voer de test uit en controleer of deze slaagt.

1. Maak vervolgens een set methoden om ervoor te zorgen dat `isEmpty()` true retourneert wanneer een van de vereiste gegevenspunten (naam, bezinking of afbeelding) leeg is.

   Voor elke test, wordt een discrete modelmiddeldefinitie gebruikt, update **BylineImplTest.json** met de extra middeldefinities voor **zonder-naam** en **zonder-bezettingen**.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   Maak de volgende testmethoden om deze statussen te testen.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** test de lege modelbrondefinitie en stelt dat `isEmpty()` waar is.

   **`testIsEmpty_WithoutName()`** test op een modelbrondefinitie die beroepen maar geen naam heeft.

   **`testIsEmpty_WithoutOccupations()`** test tegen een modelbrondefinitie die een naam maar geen beroepen heeft.

   **`testIsEmpty_WithoutImage()`** test tegen een mock middeldefinitie met een naam en bezinkingen maar plaatst het mockBeeld om aan ongeldig terug te keren. Merk op dat wij het `modelFactory.getModelFromWrappedRequest(..)` gedrag met voeten willen treden dat in `setUp()` wordt bepaald om het voorwerp van het Beeld te verzekeren door deze vraag is teruggekeerd ongeldig is. De functie Mockito-stubs is strikt en wil geen dubbele code. Daarom stellen we het model met **`lenient`** -instellingen zo in dat expliciet wordt aangegeven dat we het gedrag in de methode `setUp()` overschrijven.

   **`testIsEmpty_WithoutImageSrc()`** test een modelbrondefinitie met een naam en een bezigheid, maar plaatst het modelBeeld om een leeg koord terug te keren wanneer `getSrc()` wordt aangehaald.

1. Tot slot schrijf een test om ervoor te zorgen dat **isEmpty ()** vals terugkeert wanneer de component behoorlijk wordt gevormd. Voor deze voorwaarde, kunnen wij **opnieuw gebruiken/content/byline** die een volledig gevormde component van de Byline vertegenwoordigt.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Voer nu alle eenheidstests in het bestand BylineImplTest.java uit en bekijk de uitvoer van het Java™-testrapport.

![ Alle tests overgaan ](./assets/unit-testing/all-tests-pass.png)

## Tests van de eenheid als onderdeel van de constructie {#running-unit-tests-as-part-of-the-build}

Eenheidstests worden uitgevoerd en moeten als onderdeel van de gefabriceerde build slagen. Dit zorgt ervoor dat alle tests met succes overgaan alvorens een toepassing wordt opgesteld. Het uitvoeren van Gemaakte doelstellingen zoals pakket of installeert roept automatisch en vereist de overgaan van alle eenheidstests in het project.

```shell
$ mvn package
```

![ mvn pakketsucces ](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Op dezelfde manier, als wij een testmethode veranderen om te ontbreken, ontbreekt de bouwstijl en rapporten die test ontbrak en waarom.

![ mvn pakket ontbreekt ](assets/unit-testing/mvn-package-fail.png)

## De code controleren {#review-the-code}

Bekijk de gebeëindigde code op [ GitHub ](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de tak van het Git `tutorial/unit-testing-solution` op.
