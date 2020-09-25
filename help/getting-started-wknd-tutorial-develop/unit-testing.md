---
title: Eenheid testen
description: Deze zelfstudie behandelt de implementatie van een eenheidstest die het gedrag van het Sling Model van de component Byline bevestigt, dat in de zelfstudie van de Component van de Douane wordt gecreeerd.
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '3544'
ht-degree: 0%

---


# Eenheid testen {#unit-testing}

Deze zelfstudie behandelt de implementatie van een eenheidstest die het gedrag van het Sling Model van de component Byline bevestigt, dat in de zelfstudie van de Component [van de](./custom-component.md) Douane wordt gecreeerd.

## Vereisten {#prerequisites}

Bekijk de basislijncode waarop de zelfstudie is gebaseerd:

1. Clone the [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repository.
1. De `unit-testing/start` vertakking uitchecken

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
$ cd ~/code/aem-guides-wknd
$ git checkout unit-testing/start
```

U kunt de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd/tree/unit-testing/solution) altijd bekijken of de code plaatselijk controleren door aan de tak te schakelen `unit-testing/solution`.

## Doelstelling

1. Begrijp de grondbeginselen van eenheidstests.
1. Leer over kaders en hulpmiddelen algemeen worden gebruikt om AEM code te testen die.
1. Begrijp opties voor het verplaatsen of simuleren van AEM middelen wanneer het schrijven van eenheidstests.

## Achtergrond {#unit-testing-background}

In dit leerprogramma, zullen wij onderzoeken hoe te om de Tests [van de](https://en.wikipedia.org/wiki/Unit_testing) Eenheid voor het [Verkopen Model](https://sling.apache.org/documentation/bundles/models.html) van onze component Byline te schrijven (die in het [Creëren van een douane AEM Component](custom-component.md)wordt gecreeerd). De tests van de eenheid zijn bouwstijltijdtests die in Java worden geschreven die verwacht gedrag van code van Java verifiëren. Elke eenheidstest is doorgaans klein en valideert de uitvoer van een methode (of werkeenheden) aan de hand van de verwachte resultaten.

We gebruiken AEM best practices en gebruiken:

* [JUnit 5](https://junit.org/junit5/)
* [Mockito Testing Framework](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/) (dat voortbouwt op [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

>[!VIDEO](https://video.tv.adobe.com/v/30207/?quality=12&learn=on)

## Unit Testing and Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) integreert de uitvoering van eenheidstests en de rapportering [van](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) codedekking in zijn CBI/CD pijpleiding om de beste praktijken van eenheid het testen AEM code te bevorderen en te bevorderen.

Hoewel code voor eenheidstests een goede praktijk is voor elke codebasis, is het bij het gebruik van Cloud Manager belangrijk om te profiteren van de mogelijkheden voor het testen en rapporteren van de codekwaliteit door eenheidstests op te geven voor Cloud Manager om te worden uitgevoerd.

## Inspect de test Geweven afhankelijkheden {#inspect-the-test-maven-dependencies}

De eerste stap is Geweven gebiedsdelen te inspecteren om het schrijven en het runnen van de tests te steunen. Er zijn vier vereiste afhankelijkheden:

1. JUnit5
1. Mockito Test Framework
1. Apache Sling Mocks
1. AEM Mocks Test Framework (per io.wcm)

De **JUnit5**, **Mockito** en **AEM de testgebiedsdelen van Masks** worden automatisch toegevoegd aan het project tijdens opstelling gebruikend [AEM Maven archetype](project-setup.md).

1. Om deze gebiedsdelen te bekijken, open POM van de Bovenliggende Reactor op **aem-guides-wknd/pom.xml**, navigeer aan `<dependencies>..</dependencies>` en zorg ervoor de volgende gebiedsdelen worden bepaald:

   ```xml
   <dependencies>
       ...
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.5.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-simple</artifactId>
           <version>1.7.25</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>2.25.1</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>2.25.1</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>2.5.2</version>
           <scope>test</scope>
       </dependency>
       ...
   </dependencies>
   ```

1. Open **aem-guides-wknd/core/pom.xml** en bekijk dat de overeenkomstige testgebiedsdelen beschikbaar zijn:

   ```xml
   ...
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
   </dependency>
   ...
   ```

   Een parallelle bronomslag in het **kernproject** zal de eenheidstests en om het even welke ondersteunende testdossiers bevatten. Deze **testmap** biedt een scheiding tussen testklassen en de broncode, maar de tests kunnen net zo werken als in dezelfde pakketten als de broncode.

## De JUnit-test maken {#creating-the-junit-test}

Eenheidstests wijzen doorgaans 1-op-1 toe met Java-klassen. In dit hoofdstuk, zullen wij een test JUnit voor **BylineImpl.java** schrijven, die het het Verlenen Model is dat de component van de Naamregel steunt.

![verkenner van eenheidstests](assets/unit-testing/core-src-test-folder.png)

*Locatie waar eenheidstests worden opgeslagen.*

1. Dit kunnen we doen in Eclipse door met de rechtermuisknop op de Java-klasse te klikken om te testen en **Nieuw > Overige > Java > JUnit > JUnit Test Case** te selecteren.

   ![Klik met de rechtermuisknop op BylineImpl.java om een eenheidstest te maken](assets/unit-testing/junit-test-case-1.png)

1. In het eerste tovenaar scherm, bevestig het volgende:

   * Het JUnit-testtype is de **New JUnit Jupiter-test** , omdat dit de JUnit Maven-afhankelijkheden zijn die zijn ingesteld in onze **pom.xml&#39;s**.
   * Het **pakket** is het Java-pakket van de klasse die wordt getest (`BylineImpl.java`)
   * De bronomslag richt aan het **kernproject** , (`aem-guides-wknd.core/src/test/java`) dat Eclipse instrueert waar de dossiers van de eenheidstest worden opgeslagen.
   * De `setUp()` methode stub wordt handmatig gemaakt. we zullen zien hoe dit later wordt gebruikt .
   * De te testen klasse is `BylineImpl.java`, omdat dit de Java-klasse is die we willen testen.

   ![stap 2 van de wizard voor eenheidstest](assets/unit-testing/junit-wizard-testcase.png)

   *Wizard Testcase JUnit - stap 2*

1. Klik op de knop **Volgende** onder aan de wizard.

   Deze volgende stap helpt bij het automatisch genereren van testmethoden. Doorgaans heeft elke openbare methode van de Java-klasse ten minste één bijbehorende testmethode, die het gedrag ervan valideert. Vaak zal een eenheidstest veelvoudige testmethodes hebben die één enkele openbare methode testen, elk die een verschillende reeks input of staten vertegenwoordigen.

   In de tovenaar, selecteer alle methodes onder `BylineImpl`, met uitzondering van `init()` die een methode intern die door het het Verdelen Model (via `@PostConstruct`) wordt gebruikt is. We zullen de test effectief uitvoeren `init()` door alle andere methoden te testen, aangezien de andere methoden afhankelijk zijn van een succesvolle `init()` uitvoering.

   Nieuwe testmethoden kunnen op elk moment worden toegevoegd aan de JUnit-testklasse. Deze pagina van de wizard is alleen maar bedoeld voor het gemak.

   ![stap 3 van de wizard voor eenheidstest](assets/unit-testing/junit-test-case-3.png)

   *Wizard Testcase JUnit (vervolg)*

1. Klik op de knop Voltooien onder aan de wizard om het JUnit5-testbestand te genereren.
1. Controleer of het JUnit5-testbestand in de overeenkomende pakketstructuur is gemaakt op **aem-guides-wknd.core** > **/src/test/java** als een bestand met de naam `BylineImplTest.java`.

## BylineImplTest.java evalueren {#reviewing-bylineimpltest-java}

Ons testbestand bevat een aantal automatisch gegenereerde methoden. Op dit punt is er niets AEM specifiek voor dit JUnit-testbestand.

De eerste methode is `public void setUp() { .. }` met een annotatie `@BeforeEach`.

De `@BeforeEach` annotatie is een JUnit-annotatie die de JUnit-test opgeeft deze methode uit te voeren voordat elke testmethode in deze klasse wordt uitgevoerd.

De volgende methoden zijn de testmethoden zelf en zijn als zodanig gemarkeerd met de `@Test` aantekening. Bericht dat door gebrek, al onze tests worden geplaatst om te ontbreken.

Wanneer deze JUnit-testklasse (ook wel JUnit Test Case genoemd) wordt uitgevoerd, `@Test` wordt elke met de testklasse gemarkeerde methode uitgevoerd als een test die geslaagd of gezakt kan worden.

![gegenereerde BylineImplTest](assets/unit-testing/bylineimpltest-new.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Voer de JUnit-testcase uit door met de rechtermuisknop op de klassenaam te klikken en **Uitvoeren als > JUnit-test**.

   ![Uitvoeren als junitest](assets/unit-testing/run-as-junit-test.png)

   *Klik met de rechtermuisknop op BylineImplTests.java > Uitvoeren als > JUnit-test*

1. Zoals verwacht, mislukken alle tests.

   ![mislukken van de tests](assets/unit-testing/all-tests-fail.png)

   *JUnit-weergave op Eclipse > Venster > Weergave tonen > Java > JUnit*

## BylineImpl.java evalueren {#reviewing-bylineimpl-java}

Bij het schrijven van eenheidstests zijn er twee primaire benaderingen:

* [TDD of testontwikkeling](https://en.wikipedia.org/wiki/Test-driven_development), waarbij de eenheidstests stapsgewijs worden geschreven, onmiddellijk voordat de implementatie wordt ontwikkeld; Schrijf een test, schrijf de implementatie om de testpas te maken.
* Implementatie-eerste Ontwikkeling, die het ontwikkelen van werkende code eerst en dan het schrijven tests impliceert die genoemde code bevestigen.

In deze zelfstudie wordt de laatste aanpak gebruikt (aangezien we al een werkende **BylineImpl.java** in een vorig hoofdstuk hebben gemaakt). Daarom moeten we het gedrag van de openbare methoden van het systeem, maar ook een aantal van de toepassingsdetails ervan, herzien en begrijpen. Dit kan tegendeel klinken, aangezien een goede test alleen de inputs en outputs moet behandelen, maar wanneer het werken in AEM, zijn er een verscheidenheid aan implementatieoverwegingen die moeten worden begrepen om de lopende tests te construeren.

TDD in de context van AEM vereist een niveau van deskundigheid en kan het best worden aangenomen door AEM ontwikkelaars die deskundig zijn in AEM ontwikkeling en eenheidstests van AEM code.

>[!VIDEO](https://video.tv.adobe.com/v/30208/?quality=12&learn=on)

## AEM testcontext instellen  {#setting-up-aem-test-context}

De meeste code die voor AEM wordt geschreven, is gebaseerd op API&#39;s van het type JCR, Sling of AEM. Deze API&#39;s vereisen dat de context van een actieve AEM correct wordt uitgevoerd.

Aangezien de eenheidstests bij bouwstijl, buiten de context van een lopende AEM instantie worden uitgevoerd, is er geen zulk middel. Om dit gemakkelijker te maken, [leidt het Mocks van AEM](https://wcm.io/testing/aem-mock/usage.html) van wcm.io tot een mock context die deze APIs toestaat om meestal te handelen alsof zij in AEM lopen.

1. Maak een AEM met **wcm.io&#39;s** `AemContext` in **BylineImplTest.java** door deze toe te voegen als een JUnit-extensie die is ingericht voor `@ExtendWith` het bestand **BylineImplTest.java** . De extensie zorgt voor alle vereiste initialisatie- en opschoningstaken. Maak een klassevariabele voor `AemContext` die voor alle testmethoden kan worden gebruikt.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Deze variabele, `ctx`, stelt een mock AEM context bloot die een aantal AEM en het Schuiven abstracties verstrekt:

   * Het BylineImpl Sling-model wordt in deze context geregistreerd
   * In deze context worden structuur voor JCR-inhoud van Mock gemaakt
   * Aangepaste OSGi-services kunnen in deze context worden geregistreerd
   * Verstrekt een verscheidenheid van gemeenschappelijke vereiste mock voorwerpen en helpers zoals voorwerpen SlingHttpServletRequest, een verscheidenheid van de AEMdiensten van het Sling en OSGi zoals ModelFactory, PageManager, Pagina, Malplaatje, ComponentManager, Component, TagManager, Markering, enz.
      * *Niet alle methoden voor deze objecten zijn geïmplementeerd.*
   * And [much more](https://wcm.io/testing/aem-mock/usage.html)!

   Het **`ctx`** object fungeert als ingangspunt voor het grootste deel van onze modelcontext.

1. Definieer in de `setUp(..)` methode, die wordt uitgevoerd vóór elke `@Test` methode, een algemene teststatus voor modellen:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registreert het Te testen Model van de Schelling, in de AEMContext, zodat kan het in de `@Test` methodes worden geconcretiseerd.
   * **`load().json`** laadt middelstructuren in de modelcontext, toestaand de code om met deze middelen in wisselwerking te staan alsof zij door een echte bewaarplaats werden verstrekt. De brondefinities in het bestand **`BylineImplTest.json`** worden geladen in de standaard JCR-context onder **/content**.
   * **`BylineImplTest.json`** bestaat nog niet, dus laten we het maken en de JCR-bronstructuren definiëren die nodig zijn voor de test.

1. De JSON-bestanden die de modelbronstructuren vertegenwoordigen, worden opgeslagen onder **core/src/test/resources** volgens hetzelfde pakketpad als het JUnit Java-testbestand.

   Maak een nieuw JSON-bestand op **core/test/resources/com/adobe/aem/guides/wknd/core/models/impl** met de naam **BylineImplTest.json** met de volgende inhoud:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Deze JSON definieert een modelbrondefinitie voor de eenheidstest van de Byline-component. Op dit punt, heeft JSON de minimumreeks eigenschappen die worden vereist om een de inhoudsmiddel van de component van de Byline, `jcr:primaryType` en `sling:resourceType`te vertegenwoordigen.

   Een algemene regel van hen wanneer het werken met eenheidstests moet de minimale reeks mokinhoud, context, en code tot stand brengen die wordt vereist om elke test te voldoen. Vermijd de verleiding om een complete mock-context op te bouwen voordat de tests worden geschreven, aangezien dit vaak tot ongewenste artefacten leidt.

   Nu met het bestaan van **BylineImplTest.json**, wanneer `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` wordt uitgevoerd, worden de modelmiddeldefinities geladen in de context bij de weg **/inhoud.**

## getName() testen {#testing-get-name}

Nu we een basismodelcontext-instelling hebben, schrijven we onze eerste test voor getName() **van** BylineImpl. Deze test moet ervoor zorgen dat de methode **getName()** de juiste geschreven naam retourneert die bij de eigenschap &quot;**name&quot;** van de resource is opgeslagen.

1. Werk de methode **testGetName**() in **BylineImplTest.java** als volgt bij:

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
   import static org.junit.jupiter.api.Assertions.assertEquals;
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

   * **`String expected`** Hiermee stelt u de verwachte waarde in. We stellen dit in op &quot;**Jane Done**&quot;.
   * **`ctx.currentResource`** plaatst de context van het modelmiddel om de code tegen te evalueren, zodat wordt dit geplaatst aan **/content/byline** aangezien dat is waar de mock byline inhoudsmiddel wordt geladen.
   * **`Byline byline`** instantieert het Byline Sling Model door het van het modelvoorwerp van het Verzoek aan te passen.
   * **`String actual`** Roept de methode aan die we testen, `getName()`op het Byline Sling Model-object.
   * **`assertEquals`** Geeft aan dat de verwachte waarde overeenkomt met de waarde die wordt geretourneerd door het byline Sling Model-object. Als deze waarden niet gelijk zijn, zal de test mislukken.

1. Test uitvoeren... en het faalt met een `NullPointerException`.

   Merk op dat deze test NIET ontbreekt omdat wij nooit een `name` bezit in het mock JSON bepaalde, die de test zal veroorzaken om te ontbreken hoewel de testuitvoering niet aan dat punt heeft gekregen! Deze test mislukt als gevolg van een fout in het `NullPointerException` byline-object zelf.

1. In de video [Reviewing BylineImpl.java](#reviewing-bylineimpl-java) hierboven bespreken we hoe als een uitzondering wordt `@PostConstruct init()` gegenereerd het Sling Model verhindert om te instantiëren, en dat is wat hier gebeurt.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Het blijkt dat hoewel de ModelFactory OSGi dienst via `AemContext` (als context van Apache Sling) wordt verstrekt, niet alle methodes worden uitgevoerd, met inbegrip `getModelFromWrappedRequest(...)` van die die in de `init()` methode van BylineImpl wordt geroepen. Dit resulteert in een [AbstractMethodError](https://docs.oracle.com/javase/8/docs/api/java/lang/AbstractMethodError.html), die in termijn veroorzaakt `init()` om te ontbreken, en de resulterende aanpassing van `ctx.request().adaptTo(Byline.class)` is een ongeldig voorwerp.

   Aangezien de geleverde modellen onze code niet kunnen aanpassen, moeten wij de modelcontext zelf uitvoeren voor dit, kunnen wij Mockito gebruiken om een modelvoorwerp te creëren ModelFactory, dat een modelvoorwerp van het Beeld terugkeert wanneer op het `getModelFromWrappedRequest(...)` wordt aangehaald.

   Omdat deze modelcontext aanwezig moet zijn om zelfs maar een instantie van het Byline Sling-model te kunnen maken, kunnen we deze toevoegen aan de `@Before setUp()` methode. We moeten ook de klasse toevoegen `MockitoExtension.class` aan de `@ExtendWith` annotatie boven de **klasse BylineImplTest** .

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
   
   import static org.junit.jupiter.api.Assertions.assertEquals;
   import static org.junit.jupiter.api.Assertions.fail;
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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** markeert de klasse van het Geval van de Test die met de Uitbreiding [van Jupiter van](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Mockito JUnit moet worden in werking gesteld die voor het gebruik van de @Mock annotations toestaat om mock voorwerpen op het niveau van de Klasse te bepalen.
   * **`@Mock private Image`** maakt een mock-object van het type `com.adobe.cq.wcm.core.components.models.Image`. Merk op dat dit op klassenniveau wordt bepaald zodat, zoals nodig, de `@Test` methodes zijn gedrag kunnen veranderen zoals nodig.
   * **`@Mock private ModelFactory`** Maakt een mock-object van het type ModelFactory. Merk op dat dit een zuiver Mockito mock is en dat er geen methoden op zijn toegepast. Merk op dat dit op klassenniveau wordt bepaald zodat, zoals nodig, de `@Test`methodes zijn gedrag kunnen veranderen.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registreert modelgedrag voor wanneer `getModelFromWrappedRequest(..)` wordt geroepen op het modelvoorwerp ModelFactory. Het resultaat dat in wordt gedefinieerd, `thenReturn (..)` is het retourneren van het modelafbeeldingsobject. Dit gedrag wordt alleen aangeroepen wanneer: de eerste parameter is gelijk aan het verzoekvoorwerp van `ctx`de kern, is de tweede parameter om het even welk voorwerp van het Middel, en de derde parameter moet de klasse van het Beeld van de Componenten van de Kern zijn. Wij aanvaarden om het even welke Middel omdat door onze tests wij `ctx.currentResource(...)` aan diverse modelmiddelen plaatsen die in **BylineImplTest.json** worden bepaald. Merk op dat wij **lenient ()** striktheid toevoegen omdat wij later dit gedrag van ModelFactory zullen willen met voeten treden.
   * **`ctx.registerService(..)`.** Registreert het modelobject ModelFactory in AemContext, met de hoogste de dienstrangschikking. Dit is vereist omdat ModelFactory die in BylineImpl wordt gebruikt via het `init()` gebied `@OSGiService ModelFactory model` wordt ingespoten. Opdat AemContext **ons** mock-object injecteert, dat oproepen aan `getModelFromWrappedRequest(..)`behandelt, moeten wij het registreren als hoogste rangschikkende Dienst van dat type (ModelFactory).

1. Voer de test opnieuw uit, en nogmaals, het mislukt, maar dit keer is het bericht duidelijk waarom de test is mislukt.

   ![testnaam fout bewering](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName() mislukt door bewering*

   We ontvangen een **AssertionError** , wat betekent dat de voorwaarde van de aanroep in de test is mislukt. De **verwachte waarde is &quot;Jane Doe&quot;** maar de **werkelijke waarde is null**. Dit is logisch omdat het &quot;**name&quot;** bezit niet aan mock **/content/byline** middeldefinitie in **BylineImplTest.json** is toegevoegd, zo laten wij het toevoegen:

1. Update **BylineImplTest.json** om te definiëren `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Voer de test opnieuw uit en ga **`testGetName()`** nu door!

## getOccupations() testen {#testing-get-occupations}

Goed zo! Onze eerste test is geslaagd! Laten we verder gaan en testen `getOccupations()`. Aangezien de initialisatie van de modelcontext in de `@Before setUp()`methode was, zal dit voor alle `@Test` methodes in deze Testcase, met inbegrip van beschikbaar zijn `getOccupations()`.

Onthoud dat deze methode een alfabetisch gesorteerde lijst met beroepen (aflopend) moet retourneren die is opgeslagen in de eigenschap bezettingen.

1. Bijwerken **`testGetOccupations()`** als volgt:

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

   * **`List<String> expected`** het verwachte resultaat te definiëren.
   * **`ctx.currentResource`** plaatst het huidige middel om de context tegen de modelmiddeldefinitie bij /content/byline te evalueren. Dit verzekert **BylineImpl.java** in de context van onze modelmiddel uitvoert.
   * **`ctx.request().adaptTo(Byline.class)`** instantieert het Byline Sling Model door het van het modelvoorwerp van het Verzoek aan te passen.
   * **`byline.getOccupations()`** Roept de methode aan die we testen, `getOccupations()`op het Byline Sling Model-object.
   * **`assertEquals(expected, actual)`** De verwachte lijst van berichten is het zelfde als de daadwerkelijke lijst.

1. Herinner me, enkel zoals **`getName()`** hierboven, **BylineImplTest.json** geen beroepen bepaalt, zodat zal deze test ontbreken als wij het in werking stellen, aangezien `byline.getOccupations()` zal een lege lijst terugkeren.

   Update **BylineImplTest.json** om een lijst van beroepen op te nemen, en zij zullen in niet alfabetische orde worden geplaatst om ervoor te zorgen dat onze tests bevestigen dat de beroepen door **`getOccupations()`**. worden gesorteerd.

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

   ![Bezettingspas ophalen](assets/unit-testing/testgetoccupations-success.png)

   *testGetOccupations() geslaagd*

## Testing isEmpty() {#testing-is-empty}

De laatste methode die moet worden getest **`isEmpty()`**.

Testen `isEmpty()` is interessant omdat het testen onder verschillende omstandigheden vereist. Als u de methode **BylineImpl.java**`isEmpty()` controleert, moeten de volgende voorwaarden worden getest:

* Retourneer true wanneer de naam leeg is
* Retourneer true wanneer de bezettingen null of leeg zijn
* Retourneer true wanneer de afbeelding null is of geen src-URL heeft
* Retourneer false wanneer de naam, de bezetting en de afbeelding (met een URL voor de bron) aanwezig zijn

Hiervoor moeten we nieuwe testmethoden maken, waarbij elke test een specifieke voorwaarde en nieuwe modelbronstructuren moet testen `BylineImplTest.json` om deze tests uit te voeren.

Met deze controle konden we het testen overslaan op wanneer `getName()`, `getOccupations()` en `getImage()` zijn leeg omdat het verwachte gedrag van die status via `isEmpty()`wordt getest.

1. De eerste test zal de voorwaarde van een gloednieuwe component testen, die geen eigenschappen heeft geplaatst.

   Voeg een nieuwe middeldefinitie aan toe `BylineImplTest.json`, die het de semantische naam &quot;**leeg** geeft

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

   **`"empty": {...}`** definieert een nieuwe brondefinitie met de naam &quot;leeg&quot; die alleen een `jcr:primaryType` en `sling:resourceType`.

   Herinner me wij laden `BylineImplTest.json` in `ctx` vóór de uitvoering van elke testmethode in `@setUp`, zodat is deze nieuwe middeldefinitie onmiddellijk beschikbaar aan ons in tests bij **/content/empty.**

1. Werk `testIsEmpty()` als volgt bij, plaatsend het huidige middel aan de nieuwe &quot;**lege**&quot;modelmiddeldefinitie.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Voer de test uit en controleer of deze slaagt.

1. Maak vervolgens een set methoden om ervoor te zorgen dat wanneer een van de vereiste gegevenspunten (naam, bezinking of afbeelding) leeg is, de waarde true wordt `isEmpty()` geretourneerd.

   Voor elke test, wordt een discrete mock middeldefinitie gebruikt, werk **BylineImplTest.json** met de extra middeldefinities voor **zonder-naam** en **zonder-bezettingen** bij.

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

   **`testIsEmpty()`** test tegen de lege modelmiddeldefinitie, en beweert dat waar `isEmpty()` is.

   **`testIsEmpty_WithoutName()`** test tegen een mock middeldefinitie die beroepen maar geen naam heeft.

   **`testIsEmpty_WithoutOccupations()`** test tegen een mock middeldefinitie die een naam maar geen beroepen heeft.

   **`testIsEmpty_WithoutImage()`** test tegen een modelmiddeldefinitie met een naam en bezinkingen maar plaatst het modelBeeld om aan ongeldig terug te keren. Merk op dat wij het `modelFactory.getModelFromWrappedRequest(..)`gedrag willen met voeten treden dat in wordt bepaald `setUp()` om het voorwerp van het Beeld te verzekeren door deze vraag is ongeldig is teruggekeerd. De functie Mockito-stubs is strikt en wil geen dubbele code. Daarom plaatsen wij het mock met **`lenient`** montages om uitdrukkelijk op te merken wij het gedrag in de `setUp()` methode met voeten treden.

   **`testIsEmpty_WithoutImageSrc()`** test tegen een modelmiddeldefinitie met een naam en bezinkingen, maar plaatst het modelBeeld om een leeg koord terug te keren wanneer `getSrc()` wordt aangehaald.

1. Ten slotte, schrijf een test om ervoor te zorgen dat **isEmpty ()** vals terugkeert wanneer de component behoorlijk wordt gevormd. Voor deze voorwaarde, kunnen wij opnieuw gebruiken **/inhoud/byline** die een volledig gevormde component van de Byline vertegenwoordigt.

   ```java
   @Test
   public void testIsNotEmpty() {
   ctx.currentResource("/content/byline");
   when(image.getSrc()).thenReturn("/content/bio.png");
   
   Byline byline = ctx.request().adaptTo(Byline.class);
   
   assertFalse(byline.isEmpty());
   }
   ```

## Codedekking {#code-coverage}

Codedekking is de hoeveelheid broncode waarop eenheidstests betrekking hebben. Moderne IDEs verstrekt tooling die automatisch controleert welke broncode tijdens de eenheidstests wordt uitgevoerd. Hoewel de codedekking op zich geen indicator van codekwaliteit is, is het nuttig om te begrijpen als er belangrijke gebieden van broncode zijn die niet door eenheidstests worden getest.

1. Klik in de Projectverkenner van Eclipse met de rechtermuisknop op **BylineImplTest.java** en selecteer **Dekking als > JUnit-test**

   Zorg ervoor dat de overzichtsweergave Dekking is geopend (Venster > Weergave > Overige > Java > Dekking).

   Dit zal de eenheidstests binnen dit dossier in werking stellen en zal een rapport verstrekken die op de codedekking wijzen. Door in de klasse en methoden te boren, geeft u duidelijker aan welke delen van het bestand worden getest en welke niet.

   ![uitvoeren als codedekking](assets/unit-testing/bylineimpl-coverage.png)

   *Overzicht van codedekking*

   Eclipse biedt een snelle weergave van het aantal klassen en methoden dat door de eenheidstest wordt bestreken. Met deze optie worden de coderegels van even kleuren uitgeknipt:

   * **Groen** is code die door minstens één test wordt uitgevoerd
   * **Geel** geeft een vertakking aan die niet door een test wordt geëvalueerd
   * **Rood** geeft code aan die niet door een test wordt uitgevoerd

1. In het dekkingsrapport is het geïdentificeerd de tak uitvoert wanneer het bezettingsgebied ongeldig is en een lege lijst terugkeert, nooit wordt geëvalueerd. Dit wordt aangegeven door regels 571 en 86 die geel zijn, een vertakking aangeven van de if/else-regel niet wordt uitgevoerd en regel 75 in rood die aangeeft dat de coderegel nooit wordt uitgevoerd.

   ![kleurcodering van dekking](assets/unit-testing/coverage-color-coding.png)

1. Dit kan worden verholpen door een test voor toe te voegen `getOccupations()` die een lege lijst bevestigt is teruggekeerd wanneer er geen bezettingswaarde op het middel is. Voeg de volgende nieuwe testmethode toe aan **BylineImplTests.java**.

   ```java
   @Test
   public void testGetOccupations_WithoutOccupations() {
       List<String> expected = Collections.emptyList();
   
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   **`Collections.emptyList();`** Hiermee stelt u de verwachte waarde in op een lege lijst.

   **`ctx.currentResource("/content/empty")`** plaatst het huidige middel aan /content/empty, die wij weten geen gespecificeerd bezit van beroepen heeft.

1. Herstel de Dekking als, het meldt dat **BylineImpl.java** nu bij 100% dekking is, nochtans is er nog één tak die niet in isEmpty() wordt geëvalueerd die opnieuw met de beroepen te maken heeft. In dit geval worden de bezettingen == null geëvalueerd, maar bezetations.isEmpty() is dit niet omdat er geen modelbrondefinitie is die wordt ingesteld `"occupations": []`.

   ![Dekking met testGetOccupations_WithoutOccupations()](assets/unit-testing/getoccupations-withoutoccupations.png)

   *Dekking met testGetOccupations_WithoutOccupations()*

1. Dit kan gemakkelijk worden opgelost door een andere testmethode te creëren die een modelmiddeldefinitie wordt gebruikt die de beroepen aan de lege serie plaatst.

   Voeg een nieuwe modelbrondefinitie aan **BylineImplTest.json** toe die een exemplaar van **&quot;zonder-bezettingen&quot;** is en voeg een bezigheid bezit toe dat aan de lege serie wordt geplaatst, en noem het **&quot;zonder-bezetations-lege-serie&quot;**.

   ```json
   "without-occupations-empty-array": {
      "jcr:primaryType": "nt:unstructured",
      "sling:resourceType": "wknd/components/content/byline",
      "name": "Jane Doe",
      "occupations": []
    }
   ```

   Creeer een nieuwe methode **@Test** in `BylineImplTest.java` die dit nieuwe modelmiddel gebruikt, bevestigt waar `isEmpty()` terugkeert.

   ```java
   @Test
   public void testIsEmpty_WithEmptyArrayOfOccupations() {
       ctx.currentResource("/content/without-occupations-empty-array");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   ![Dekking met testIsEmpty_WithEmptyArrayOfOccupations()](assets/unit-testing/testisempty_withemptyarrayofoccupations.png)

   *Dekking met testIsEmpty_WithEmptyArrayOfOccupations()*

1. Met deze laatste toevoeging `BylineImpl.java` geniet u van 100% codedekking met al het voorwaardelijke het kleven geëvalueerd.

   De tests bevestigen het verwachte gedrag van `BylineImpl` zonder terwijl het steunen van een minimale reeks implementatiedetails.

## Tests van de eenheid als onderdeel van de constructie {#running-unit-tests-as-part-of-the-build}

Eenheidstests worden uitgevoerd om te slagen als onderdeel van de gefabriceerde build. Dit zorgt ervoor dat alle tests met succes overgaan alvorens een toepassing wordt opgesteld. Het uitvoeren van Gemaakte doelstellingen zoals pakket of installeert roept automatisch en vereist de overgaan van alle eenheidstests in het project.

```shell
$ mvn package
```

![succes van mvn - pakket](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Op dezelfde manier, als wij een testmethode veranderen om te ontbreken, ontbreekt de bouwstijl en rapporten die test ontbrak en waarom.

![mvn-pakket mislukt](assets/unit-testing/mvn-package-fail.png)

## De code controleren {#review-the-code}

Bekijk de gebeëindigde code op [GitHub](https://github.com/adobe/aem-guides-wknd) of herzie en stel plaatselijk de code bij de schakelaar van de Git in `unit-testing/solution`.
