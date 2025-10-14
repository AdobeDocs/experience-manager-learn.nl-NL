---
title: Je geheimen in AEM as a Cloud Service beheren
description: Leer beste praktijken voor het beheren van geheimen binnen AEM as a Cloud Service, gebruikend hulpmiddelen en technieken die door AEM worden verstrekt om uw gevoelige informatie te beschermen, ervoor zorgen uw toepassing veilig en vertrouwelijk blijft.
version: Experience Manager as a Cloud Service
topic: Development, Security
feature: OSGI, Cloud Manager
role: Developer
jira: KT-15880
level: Intermediate, Experienced
exl-id: 856b7da4-9ee4-44db-b245-4fdd220e8a4e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# Beheren van geheimen in AEM as a Cloud Service

Het beheren van geheimen, zoals API sleutels en wachtwoorden, is essentieel voor het handhaven van toepassingsveiligheid. Adobe Experience Manager (AEM) as a Cloud Service biedt robuuste tools om geheimen veilig te verwerken.

In deze zelfstudie leert u de beste praktijken voor het beheren van geheimen in AEM. Wij zullen de hulpmiddelen en de technieken behandelen die door AEM worden verstrekt om uw gevoelige informatie te beschermen, ervoor zorgen uw toepassing veilig en vertrouwelijk blijft.

Deze zelfstudie gaat uit van een praktische kennis van AEM Java-ontwikkeling, OSGi-services, Sling Models en Adobe Cloud Manager.

## Secretenmanager OSGi-service

In AEM as a Cloud Service biedt het beheren van geheimen via OSGi-services een schaalbare en veilige aanpak. De diensten OSGi kunnen worden gevormd om gevoelige informatie, zoals API sleutels en wachtwoorden te behandelen, die door configuraties OSGi wordt bepaald, en die via Cloud Manager wordt geplaatst.

### OSGi service implementation

Wij zullen door de ontwikkeling van de douaneOSGi dienst lopen die [&#x200B; geheimen van configuraties OSGi &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values) blootstelt.

De implementatie leest geheimen van de configuratie OSGi via de `@Activate` methode en stelt hen door de `getSecret(String secretName)` methode bloot. U kunt ook discrete methoden maken, zoals `getApiKey()` voor elk geheim, maar voor deze aanpak is meer onderhoud vereist omdat geheimen worden toegevoegd of verwijderd.

```java
package com.example.core.util.impl;

import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.apache.sling.api.resource.ValueMap;
import org.apache.sling.api.resource.ValueMapDecorator;
import java.util.Map;

@Component(
    service = { SecretsManager.class }
)
public class SecretsManagerImpl implements SecretsManager {
    private static final Logger log = LoggerFactory.getLogger(SecretsManagerImpl.class);
 
    private ValueMap secrets;

    @Override
    public String getSecret(String secretName) {
        return secrets.get(secretName, String.class);
    }

    @Activate
    @Modified
    protected void activate(Map<String, Object> properties) {
        secrets = new ValueMapDecorator(properties);
    }
}
```

Als dienst OSGi, is het best om het via een interface van Java te registreren en te verbruiken. Hieronder is een eenvoudige interface die consumenten toestaat om geheimen door OSGi bezitsnaam te krijgen.

```java
package com.example.core.util;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface SecretsManager {
    String getSecret(String secretName);
}
```

## Toewijzing van geheimen aan configuratie OSGi

Om geheime waarden in de dienst bloot te stellen OSGi, hen in kaart brengen aan configuraties OSGi gebruikend [&#x200B; OSGi geheime configuratiewaarden &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values). Definieer de eigenschapnaam OSGi als sleutel om de geheime waarde van de `SecretsManager.getSecret()` methode terug te winnen.

Definieer de geheimen in het OSGi-configuratiebestand `/apps/example/osgiconfig/config/com.example.core.util.impl.SecretsManagerImpl.cfg.json` in uw AEM Maven-project. Elke eigenschap vertegenwoordigt een geheim dat in AEM wordt weergegeven, met de waarde ingesteld via Cloud Manager. De sleutel is de OSGi bezitsnaam, die wordt gebruikt om de geheime waarde van de `SecretsManager` dienst terug te winnen.

```json
{
    "api.key": "$[secret:api_key]",
    "service.password": "$[secret:service_password]"
}
```

Alternatief aan het gebruiken van de gedeelde geheime dienst OSGi, kunt u geheimen in de configuratie OSGi van de specifieke diensten direct omvatten die hen gebruiken. Deze benadering is nuttig wanneer de geheimen slechts door één enkele dienst OSGi nodig zijn en niet over de veelvoudige diensten worden gedeeld. In dit geval, worden de geheime waarden bepaald in het OSGi configuratiedossier voor de specifieke dienst en in de code van Java van de dienst via de `@Activate` methode betreden.

## Verbruiksgeheimen

De geheimen kunnen van de dienst worden verbruikt OSGi op diverse manieren, zoals van een het Verschilderen Model of een andere dienst OSGi. Hieronder staan voorbeelden van hoe u geheimen van beide kunt consumeren.

### Van verkoopmodel

Sling Models bieden vaak bedrijfslogica voor AEM-sitecomponenten. De service `SecretsManager` OSGi kan worden gebruikt via de `@OsgiService` -annotatie en worden gebruikt in het Sling-model om de geheime waarde op te halen.

```java
import com.example.core.util.SecretsManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingHttpServletRequest;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.OsgiService;

@Model(
    adaptables = {SlingHttpServletRequest.class, Resource.class},
    adapters = {ExampleDatabaseModel.class}
)
public class ExampleDatabaseModelImpl implements ExampleDatabaseModel {

    @OsgiService
    SecretsManager secretsManager;

    @Override 
    public String doWork() {
        final String secret = secretsManager.getSecret("api.key");
        // Do work with secret
    }
}
```

### Van OSGi-service

De diensten OSGi stellen vaak herbruikbare bedrijfslogica binnen AEM bloot, die door het Verschilderen Modellen, de diensten van AEM zoals Workflows, of andere douaneOSGi diensten wordt gebruikt. De service `SecretsManager` OSGi kan via de `@Reference` -annotatie worden gebruikt en binnen de OSGi-service worden gebruikt om de geheime waarde op te halen.

```java
import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component
public class ExampleSecretConsumerImpl implements ExampleSecretConsumer {

    @Reference
    SecretsManager secretsManager;

    public void doWork() {
        final String secret = secretsManager.getSecret("service.password");
        // Do work with the secret
    }
}
```

## Bezig met instellen van geheimen in Cloud Manager

Met de dienst OSGi en configuratie op zijn plaats, is de definitieve stap de geheime waarden in Cloud Manager te plaatsen.

De waarden voor geheimen kunnen via [&#x200B; Cloud Manager API &#x200B;](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#tag/Variables) of, meer algemeen, via [&#x200B; Cloud Manager UI &#x200B;](https://experienceleague.adobe.com/nl/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#overview) worden geplaatst. Een geheime variabele toepassen via de gebruikersinterface van Cloud Manager:

![&#x200B; Cloud Manager Secrets Configuratie &#x200B;](./assets/secrets/cloudmanager-configuration.png)

1. Login aan [&#x200B; Adobe Cloud Manager &#x200B;](https://my.cloudmanager.adobe.com).
1. Selecteer het AEM-programma en -omgeving waarvoor u het geheim wilt instellen.
1. In de mening van de Details van het Milieu, selecteer het **1&rbrace; lusje van de Configuratie &lbrace;.**
1. Selecteer **toevoegen**.
1. In het dialoogvenster Omgevingsconfiguratie:
   - Ga de geheime veranderlijke naam (b.v., `api_key`) in die in de configuratie OSGi van verwijzingen wordt voorzien.
   - Voer de geheime waarde in.
   - Selecteer op welke AEM-service het geheim van toepassing is.
   - Selecteer **Geheim** als type.
1. Selecteer **toevoegen** om het geheim voort te zetten.
1. Voeg zoveel geheimen toe als nodig zijn. Wanneer volledig, uitgezocht **sparen** om de veranderingen onmiddellijk op het milieu van AEM toe te passen.

Het gebruiken van configuraties van Cloud Manager voor geheimen verstrekt het voordeel om verschillende waarden voor verschillende milieu&#39;s of de diensten toe te passen en geheimen te roteren zonder de toepassing van AEM opnieuw op te stellen.
