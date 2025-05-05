---
title: SQL-verbindingen met Java™ API's
description: Leer hoe u verbinding maakt met SQL-databases vanuit AEM as a Cloud Service met Java™ SQL API's en egress-poorten.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9356
thumbnail: KT-9356.jpeg
exl-id: ec9d37cb-70b6-4414-a92b-3b84b3f458ab
duration: 124
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# SQL-verbindingen met Java™ API&#39;s

Verbindingen met SQL-databases (en andere niet-HTTP/HTTPS-services) moeten vanuit AEM worden uitgebreid.

De uitzondering op deze regel is wanneer [ specifiek adres van de uitgang ip ](../dedicated-egress-ip-address.md) in gebruik is, en de dienst op Adobe of Azure is.

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

Verzeker [ aangewezen ](../advanced-networking.md#advanced-networking) geavanceerde voorzien van een netwerkconfiguratie voorafgaand aan het volgen van dit leerprogramma is opstelling.

| Geen geavanceerde netwerken | [ Flexibele havenuitgang ](../flexible-port-egress.md) | [ Dedicated egress IP adres ](../dedicated-egress-ip-address.md) | [ Virtueel Privé Netwerk ](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi-configuratie

Aangezien de geheimen niet in code moeten worden opgeslagen, zijn de SQL gebruikersbenaming en het wachtwoord van de verbinding best verstrekt via [ geheime OSGi configuratievariabelen ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=nl-NL#secret-configuration-values), plaats gebruikend AIO CLI, of Cloud Manager APIs.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/com.adobe.aem.wknd.examples.core.connections.impl.MySqlExternalServiceImpl.cfg.json`

```json
{
  "username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "password": "$[secret:MYSQL_PASSWORD]"
}
```

Het volgende `aio CLI` bevel kan worden gebruikt om de geheimen OSGi op een per milieubasis te plaatsen:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Codevoorbeeld

Dit Java™ codevoorbeeld is van de dienst OSGi die een verbinding met een externe SQL serverWebserver, als volgende Cloud Manager `portForwards` regel van de [ enableEnvironmentAdvancedNetworkingConfiguration ](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) verrichting maakt.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/MySqlExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.sql.*;

@Component
@Designate(ocd = MySqlExternalServiceImpl.Config.class)
public class MySqlExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(MySqlExternalServiceImpl.class);

    // Get the proxy host using the AEM_PROXY_HOST Java environment variable provided by AEM as a Cloud Service
    private static final String PROXY_HOST = System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel");

    // Use the port mapped to the external MySQL service in the Cloud Manager API call
    private static final int PORT_FORWARDS_PORT_ORIG = 30001;

    private Config config;

    @Override
    public boolean isAccessible() {
        log.debug("MySQL connection URL: [ {} ]", getConnectionUrl(config));

        // Establish a connection with the external MySQL service
        // This MySQL connection URL is created in getConnection(..) which will use the AEM_PROXY_HOST is it exists, and the proxied port.
        try (Connection connection = DriverManager.getConnection(getConnectionUrl(config), config.username(), config.password())) {
            // Validate the connection
            connection.isValid(1000);

            // Close the connection, since this is just a simple connectivity check
            connection.close();

            // Return true if AEM could reach the external SQL service
            return true;
        } catch (SQLException e) {
            log.error("Unable to establish an connection with MySQL service using connection URL  [ {} ]", getConnectionUrl(config), e);
        }

        return false;
    }

    /**
     * Create a connection string to the MySQL using the AEM-provided AEM_PROXY_HOST and portForwards.portOrg port
     * defined in the Cloud Manager API mapping.
     *
     * @param config OSGi configuration object
     * @return the MySQL connection URI
     */
    private String getConnectionUrl(Config config) {
        return String.format("jdbc:mysql://%s:%d/wknd-examples", PROXY_HOST, PORT_FORWARDS_PORT_ORIG);
    }

    @Activate
    protected void activate(Config config) throws ClassNotFoundException, SQLException {
        this.config = config;

        // Load the required MySQL Driver class required for Java to make the connection
        // The OSGi bundle that contains this driver is deployed via the project's all project
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
    }

    @ObjectClassDefinition
    @interface Config {
        @AttributeDefinition(type = AttributeType.STRING)
        String username();

        @AttributeDefinition(type = AttributeType.STRING)
        String password();
    }
}
```

## MySQL-stuurprogramma-afhankelijkheden

AEM as a Cloud Service vereist vaak dat u Java™ databasestuurprogramma&#39;s aanbiedt ter ondersteuning van de verbindingen. De stuurprogramma&#39;s kunnen het beste worden geleverd door de OSGi-bundelartefacten met deze stuurprogramma&#39;s in te sluiten in het AEM-project via het `all` -pakket.

### Reactor pom.xml

Neem de afhankelijkheden van het databasestuurprogramma op in de reactor `pom.xml` en verwijs deze naar de subprojecten van `all` .

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## Alle pom.xml

Sluit de afhankelijkheidsartefacten van het databasestuurprogramma in het `all` -pakket in zodat deze kunnen worden geïmplementeerd en beschikbaar zijn op AEM as a Cloud Service. Deze artefacten __moeten__ bundels OSGi zijn die de klasse van Java™ van de gegevensbestandbestuurder uitvoeren.

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
