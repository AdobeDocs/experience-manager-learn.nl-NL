---
title: SQL-verbindingen met JDBC DataSourcePool
description: Leer hoe u verbinding maakt met SQL-databases vanuit AEM as a Cloud Service met behulp van AEM JDBC DataSourcePool en egress-poorten.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
duration: 117
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 0%

---

# SQL-verbindingen met JDBC DataSourcePool

Verbindingen met SQL gegevensbestanden (en andere niet-HTTP/HTTPS diensten) moeten uit AEM worden proxied, met inbegrip van die gemaakt gebruikend de dienst van AEM DataSourcePool OSGi om de verbindingen te beheren.

## Geavanceerde netwerkondersteuning

Het volgende codevoorbeeld wordt gesteund door de volgende geavanceerde voorzien van een netwerkopties.

Verzeker [&#x200B; aangewezen &#x200B;](../advanced-networking.md#advanced-networking) geavanceerde voorzien van een netwerkconfiguratie voorafgaand aan het volgen van dit leerprogramma is opstelling.

| Geen geavanceerde netwerken | [&#x200B; Flexibele havenuitgang &#x200B;](../flexible-port-egress.md) | [&#x200B; Dedicated egress IP adres &#x200B;](../dedicated-egress-ip-address.md) | [&#x200B; Virtueel Privé Netwerk &#x200B;](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi-configuratie

Het verbindingskoord van de configuratie OSGi gebruikt:

+ `AEM_PROXY_HOST` waarde via de [&#x200B; OSGi variabele van het configuratiemilieu &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` als gastheer van de verbinding
+ `30001` dat de `portOrig` waarde is voor de voorwaartse toewijzing van de Cloud Manager-poort `30001` → `mysql.example.com:3306`

Aangezien geheimen niet in code moeten worden opgeslagen, zijn de gebruikersbenaming en het wachtwoord van de SQL verbinding best verstrekt via OSGi configuratievariabelen, plaatsen gebruikend AIO CLI, of Cloud Manager APIs.

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

Dit Java™ codevoorbeeld is van de dienst OSGi die een verbinding met een extern gegevensbestand MySQL via de dienst van AEM DataSourcePool OSGi maakt.
De DataSourcePool OSGi fabrieksconfiguratie specificeert beurtelings een haven (`30001`) die door de `portForwards` regel in de [&#x200B; enableEnvironmentAdvancedNetworkingConfiguration &#x200B;](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) verrichting aan de externe gastheer en haven, `mysql.example.com:3306` in kaart wordt gebracht.

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
