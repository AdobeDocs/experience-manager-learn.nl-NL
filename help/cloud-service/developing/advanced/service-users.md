---
title: Servicegebruikers
description: Leer hoe u servicegebruikers in uw AEM-code kunt maken en gebruiken om gecontroleerde, programmatische toegang tot de AEM-opslagplaats te bieden.
version: Experience Manager as a Cloud Service
topic: Development
feature: OSGI, Security
role: Developer
level: Intermediate
jira: KT-9113
thumbnail: 337530.jpeg
last-substantial-update: 2022-10-10T00:00:00Z
exl-id: 66f627e4-863d-45d7-bc68-7ec108a1c271
duration: 1053
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# Servicegebruikers

Leer hoe u servicegebruikers in uw AEM-code kunt maken en gebruiken om gecontroleerde, programmatische toegang tot de AEM-opslagplaats te bieden.

>[!VIDEO](https://video.tv.adobe.com/v/337530?quality=12&learn=on)

## Bronnen

+ [ het Sling Repository Initialization (repoinit) documentatie ](https://sling.apache.org/documentation/bundles/repository-initialization.html)
+ [ het Verdelen de documentatie van de Authentificatie van de Dienst ](https://sling.apache.org/documentation/the-sling-engine/service-authentication.html)

## Code

### ContentStatisticsImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/statistics/impl/ContentStatisticsImpl.java`

```java
package com.adobe.aem.wknd.examples.core.statistics.impl;

import com.adobe.aem.wknd.examples.core.statistics.ContentStatistics;
import org.apache.sling.api.resource.LoginException;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.serviceusermapping.ServiceUserMapped;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.query.Query;
import java.util.Collections;
import java.util.Iterator;
import java.util.Map;

@Component(
        reference = {
                @Reference(
                        name = SERVICE_USER_SUB_SERVICE_ID,
                        service = ServiceUserMapped.class,
                        target = "(subServiceName=wknd-examples-statistics)"
                )
        }
)
public class ContentStatisticsImpl implements ContentStatistics {
    private static final Logger log = LoggerFactory.getLogger(ContentStatisticsImpl.class);

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    public int getAssetsCount(final ResourceResolver resourceResolver) {
        return getNumberOfAssets(resourceResolver);
    }

    @Override
    public int getAllAssetsCount() {
        // Create the Map that specifies the SubService ID
        final Map<String, Object> authInfo = Collections.singletonMap(
                ResourceResolverFactory.SUBSERVICE,
                "wknd-examples-statistics");

        // Get the auto-closing Service resource resolver
        // Remember, any ResourceResolver you get, you must ensure is closed!
        try (ResourceResolver serviceResolver = resourceResolverFactory.getServiceResourceResolver(authInfo)) {
            // Do some work with the service user's resource resolver and underlying resources. 
            // Make sure to do all work that relies on the AEM repository access in the try-block, since the serviceResolver will auto-close when it's left
            return queryAndCountAssets(serviceResolver);
        } catch (LoginException e) {
            log.error("Login Exception when obtaining a User for the Bundle Service: {} ", e);
        }

        return -1;
    }

    private int queryAndCountAssets(ResourceResolver resourceResolver) {
        final String ASSETS_QUERY = "SELECT * FROM [dam:Asset] WHERE isdescendantnode(\"/content/dam\")";

        Iterator<Resource> resources = resourceResolver.findResources(ASSETS_QUERY, Query.JCR_SQL2);

        int count = 0;
        while (resources.hasNext()) { count++; resources.next(); }

        log.info("User [ {} ] found [ {} ] assets", resourceResolver.getUserID(), count);

        return count;
    }

    @Activate
    protected void activate() {
        // We can use service users in the context's where no natural Sling security context is available to us,
        // which is usually outside the context of a Sling HTTP request.
        log.debug("Begin calling getAllAssetsCount() from the @Activate method");
        int count = getAllAssetsCount();
        log.debug("Finished calling getAllAssetsCount() from the @Activate method with count [ {} ]", count);

    }
}
```

### org.apache.sling.jcr.repoinit.RepositoryInitializer-wknd-examples-statistics.config

Niet alle Sling Repository Initializer-instructies worden ondersteund met de extensie `.config` , zoals `ACLOptions` . Als u geavanceerde instructies wilt gebruiken, gebruikt u de `.cfg.json` -indeling, waarbij elke regel van de instructie Stling Repository Initializer een afzonderlijke letterlijke tekenreeks is.

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/org.apache.sling.jcr.repoinit.RepositoryInitializer-wknd-examples-statistics.config`

```
scripts=["   
    create service user wknd-examples-statistics-service with forced path system/cq:services/wknd-examples

    # When using principal ACLs, the service user MUST be created under system/cq:services 
    set principal ACL for wknd-examples-statistics-service
        allow jcr:read on /content/dam
    end
"]
```

### org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended-wknd-examples.cfg.json

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended-wknd-examples.cfg.json`

```javascript
{
  "user.mapping": [
    "wknd-examples.core:wknd-examples-statistics=[wknd-examples-statistics-service]"
  ]
}
```
