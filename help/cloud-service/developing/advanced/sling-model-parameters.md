---
title: Sling-modellen parametereren op basis van HTML
description: Leer hoe u parameters kunt doorgeven van HTML naar een Sling-model in AEM.
version: Experience Manager as a Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
exl-id: 5d852617-720a-4a00-aecd-26d0ab77d9b3
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Sling-modellen parametereren op basis van HTML

Adobe Experience Manager (AEM) biedt een robuust raamwerk voor het bouwen van dynamische en aanpasbare webtoepassingen. Een van de krachtige functies is de mogelijkheid om de parameters van Sling Models te bepalen, waardoor de flexibiliteit en herbruikbaarheid van deze modellen worden verbeterd. Deze zelfstudie begeleidt u bij het maken van een geparametriseerd Sling-model en het gebruik ervan in HTML (HTML Template Language) voor het renderen van dynamische inhoud.

## HTML-script

In dit voorbeeld verzendt het HTML-script twee parameters naar het `ParameterizedModel` Sling Model. Het model manipuleert deze parameters in zijn `getValue()` methode en keert het resultaat voor vertoning terug.

Dit voorbeeld gaat twee parameters van het Koord over, nochtans kunt u om het even welk type van waarde of voorwerp tot het het Schuiven Model overgaan, zolang het [ Verschuivende Model gebiedstype, dat met `@RequestAttribute`](#sling-model-implementation) wordt geannoteerd het type van voorwerp of waarde aanpast die van HTML wordt overgegaan.

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **Parameterization:** de `data-sly-use` verklaring leidt tot een geval van `ParameterizedModel` met `myParameterOne` en `myParameterTwo`.
- **Voorwaardelijke Rendering:** `data-sly-test` controleert of het model klaar alvorens inhoud te tonen is.
- **Placeholder Vraag:** de `placeholderTemplate` handvatten gevallen waar het model niet klaar is.

## Implementatie verkoopmodel

Hieronder wordt beschreven hoe u het Sling-model implementeert:

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **ModelAnnotatie:** de `@Model` annotatie wijst deze klasse als het Verschuiven Model aan, aanpasbaar van `SlingHttpServletRequest` en het uitvoeren van de `ParameterizedModel` interface.
- **Attributen van het Verzoek:** de `@RequestAttribute` annotatie injecteert parameters HTML in het model.
- **Methoden:** `getValue()` voegt de parameters samen, en `isReady()` controleert dat de parameters niet leeg zijn.

De interface `ParameterizedModel` wordt als volgt gedefinieerd:

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## Voorbeeld-uitvoer

Met de parameters `"Hello"` en `"World"` genereert het HTML-script de volgende uitvoer:

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

Dit toont aan hoe geparameterized Verzamelmodellen in AEM kunnen worden beïnvloed gebaseerd op inputparameters die via HTML worden verstrekt.
