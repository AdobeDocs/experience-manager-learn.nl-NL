---
title: SQL-verbindingen met JDBC DataSourcePool
description: Leer hoe u verbinding maakt met SQL-databases vanuit AEM as a Cloud Service met behulp van AEM JDBC DataSourcePool- en egress-poorten.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
duration: 124
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 0%

---

# SQL-verbindingen met JDBC DataSourcePool

Verbindingen met SQL gegevensbestanden (en andere niet-HTTP/HTTPS diensten) moeten uit AEM worden proxied, met inbegrip van die gemaakt die de dienst DataSourcePool OSGi gebruiken om de verbindingen te beheren.

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

Zorg ervoor dat de [passend](../advanced-networking.md#advanced-networking) de geavanceerde voorzien van een netwerkconfiguratie is opstelling voorafgaand aan het volgen van dit leerprogramma.

| Geen geavanceerde netwerken | [Flexibele poortuitgang](../flexible-port-egress.md) | [IP-adres van specifiek egress](../dedicated-egress-ip-address.md) | [Virtueel privé netwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi-configuratie

Het verbindingskoord van de configuratie OSGi gebruikt:

+ `AEM_PROXY_HOST` waarde via de [OSGi-variabele voor configuratieomgeving](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` als de host van de verbinding
+ `30001` de `portOrig` waarde voor voorwaartse toewijzing van de poort van Cloud Manager `30001` → `mysql.example.com:3306`

Aangezien geheimen niet in code moeten worden opgeslagen, zijn de gebruikersbenaming en het wachtwoord van de SQL verbinding best verstrekt via OSGi configuratievariabelen, plaatsen gebruikend AIO CLI, of de Manager APIs van de Wolk.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST;default=proxy.tunnel]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

Het volgende `aio CLI` bevel kan worden gebruikt om de geheimen OSGi op een per milieubasis te plaatsen:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Codevoorbeeld

Dit Java™ codevoorbeeld is van de dienst OSGi die een verbinding met een extern gegevensbestand MySQL via AEM dienst DataSourcePool OSGi maakt.
De OSGi-fabrieksconfiguratie van DataSourcePool geeft op zijn beurt een poort aan (`30001`) die via de `portForwards` in de [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) exploitatie naar de externe host en poort; `mysql.example.com:3306`.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
    }
}
```

## MySQL-stuurprogramma-afhankelijkheden

AEM as a Cloud Service vereist vaak dat u Java™ databasestuurprogramma&#39;s aanbiedt ter ondersteuning van de verbindingen. De stuurprogramma&#39;s kunt u het beste bereiken door de OSGi-bundelartefacten met deze stuurprogramma&#39;s in te sluiten in het AEM-project via de `all` pakket.

### Reactor pom.xml

De afhankelijkheid van het databasestuurprogramma in de reactor opnemen `pom.xml` en verwijst u naar de `all` subprojecten.

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

Sluit de afhankelijkheidsartefacten van het databasestuurprogramma in de `all` het pakket aan hen wordt opgesteld en beschikbaar op AEM as a Cloud Service. Deze artefacten __moet__ zijn OSGi-bundels die de Java™-klasse van het databasestuurprogramma exporteren.

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
