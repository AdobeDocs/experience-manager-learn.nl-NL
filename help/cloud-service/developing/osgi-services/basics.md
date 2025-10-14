---
title: Grondbeginselen van de ontwikkeling van OSGi-services
description: Leer over de grondbeginselen van het ontwikkelen van de dienst OSGi
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8227
thumbnail: 335476.jpeg
last-substantial-update: 2022-09-16T00:00:00Z
exl-id: a3a9bf59-e9a2-4322-ac93-9c12c70b9a75
duration: 505
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---

# OSGi Services

Leer de grondbeginselen van de dienstenontwikkeling OSGi, die omvatten:

+ Een Java POJO converteren naar een OSGi-service
+ Hoe te om een Dienst OSGi aan een interface van Java te binden

>[!VIDEO](https://video.tv.adobe.com/v/335476?quality=12&learn=on)

## Bronnen

+ [@Component JavaDocs &#x200B;](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.html)
+ [@ProviderType JavaDocs &#x200B;](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/ProviderType.html)
+ [@Version JavaDocs &#x200B;](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/Version.html)

## Code

### Activities.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/Activities.java`

```java
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.ProviderType;

@ProviderType
public interface Activities {    
    String getRandomActivity();
}
```

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Component;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {

    private static final String[] ACTIVITIES = new String[] { 
        "Camping", "Skiing",  "Skateboarding"
    };

    //private final int randomIndex = new Random().nextInt(ACTIVITIES.length);
    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(ACTIVITIES.length);
        return ACTIVITIES[randomIndex];
    }    
}
```

### package-info.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/package-info.java`

```java
@Version("1.0")
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.Version;
```

Het toevoegen van a `package-info.java` wordt vereist om andere bundels OSGi in AEM te verzekeren kan de OSGi de dienstinterface (of om het even welke klasse van Java) oplossen. Als `package-info.java` ontbreekt, worden het Java-pakket en de bijbehorende Java-interfaces of -klassen niet geëxporteerd. Andere bundels OSGi die proberen om deze interfaces of klassen van Java van dit pakket van Java in te voeren, zullen fout met het bericht __niet__ in AEM console van de Bundel worden opgelost OSGi.
