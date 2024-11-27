---
title: Een model voor bloeden maken voor de component
description: Een model voor bloeden maken voor de component
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---


# Een model voor bloeden maken voor de component

Een Sling Model in AEM is een op Java gebaseerd kader dat wordt gebruikt om de ontwikkeling van achterste-eindlogica voor componenten te vereenvoudigen. Ontwikkelaars kunnen gegevens uit AEM bronnen (JCR-knooppunten) met behulp van annotaties toewijzen aan Java-objecten, zodat ze dynamische gegevens voor componenten op een schone en efficiënte manier kunnen verwerken.
Deze klasse, CountriesDropDownImpl, is een implementatie van de interface CountriesDropDown in een AEM (Adobe Experience Manager) project. Het geeft een vervolgkeuzemogelijkheid waarbij gebruikers een land kunnen selecteren op basis van het geselecteerde continent. De vervolgkeuzemenu&#39;s worden dynamisch geladen vanuit een JSON-bestand dat is opgeslagen in de AEM DAM (Digital Asset Manager).

**Gebieden in de Klasse**

* **multiSelect**: Wijst erop of dropdown veelvoudige selecties toestaat.
Wordt vanuit de componenteigenschappen geïnjecteerd met @ValueMapValue met de standaardwaarde false.
* **verzoek**: Vertegenwoordigt het huidige verzoek van HTTP. Nuttig voor toegang tot contextspecifieke informatie.
* **continent**: Slaat het geselecteerde continent voor dropdown (b.v., &quot;azië&quot;, &quot;europa&quot;) op.
Wordt geïnjecteerd vanuit het eigenschappendialoogvenster van de component, met de standaardwaarde &quot;azia&quot; als er geen waarde is opgegeven.
* **resourceResolver**:Gebruikt om tot middelen in de AEM bewaarplaats toegang te hebben en te manipuleren.
* **jsonData**: Een JSONObject dat de geparseerde gegevens van het JSON- dossier opslaat die aan het geselecteerde continent beantwoorden.

**Methoden in de Klasse**

* **getContinent ()** Een eenvoudige methode om de waarde van het continentaal gebied terug te keren.
Logs de waarde die voor het zuiveren doeleinden wordt teruggekeerd.
* **init ()** methode van de Levenscyclus die met @PostConstruct wordt geannoteerd, wordt uitgevoerd nadat de klasse wordt geconstrueerd en de gebiedsdelen worden geïnjecteerd.Construeert dynamisch de weg aan het JSON- dossier dat op de continentale waarde wordt gebaseerd.
Hiermee wordt het JSON-bestand opgehaald van de AEM DAM met de resourceResolver.
Hiermee past u het bestand aan een element aan, leest u de inhoud ervan en parseert u het in een JSONObject.
Hiermee worden eventuele fouten of waarschuwingen tijdens dit proces geregistreerd.
* **getEnums ()** wint alle sleutels (landcodes) van de geparseerde gegevens JSON terug.
Sorteert de sleutels alfabetisch en keert hen als serie terug.
Logs the number of country codes being returned.
* **getEnumNames ()** trekt alle landnamen van de ontlede gegevens JSON uit.
Sorteert de namen alfabetisch en retourneert ze als een array.
Registreert het totale aantal landen en elk teruggewonnen landnaam.
* **isMultiSelect ()** keert de waarde van het multiSelect gebied terug om erop te wijzen of dropdown veelvoudige selecties toestaat.



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## Volgende stappen

[Samenstellen, implementeren en testen](./build.md)
