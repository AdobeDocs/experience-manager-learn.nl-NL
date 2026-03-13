---
title: SAML 2.0 Aangepaste aanmeldingshook
description: Leer hoe u een aangepaste SAML 2.0-aanmeldhaak voor AEM ontwikkelt.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# SAML 2.0-aanmeldhaak

Leer hoe u een aangepaste SAML 2.0-aanmeldhaak voor AEM ontwikkelt. Deze zelfstudie biedt stapsgewijze instructies om een aangepaste aanmeldhaak te maken die integreert met een SAML 2.0-identiteitsprovider, zodat gebruikers zich kunnen verifiëren met hun SAML-referenties.

Als IDP niet de gegevens van het gebruikersprofiel en het lidmaatschap van de gebruikersgroep in de bevestiging van SAML kan verzenden, of als de gegevens vóór synchronisatie aan AEM moeten worden omgezet, kunnen de haken van douaneSAML worden uitgevoerd om het authentificatieproces van SAML uit te breiden. Met SAML-koppelingen kunt u de toewijzing van groepslidmaatschap aanpassen, gebruikersprofielkenmerken wijzigen en aangepaste bedrijfslogica toevoegen tijdens de verificatiestroom.

>[!NOTE]
>De haken van SAML van de douane worden gesteund op **AEM as a Cloud Service** en **AEM LTS**. Deze functie is niet beschikbaar in oudere AEM-versies.

## Vaak voorkomende gevallen

De haken van SAML van de douane zijn nuttig wanneer het noodzakelijk is:

+ Wijs dynamisch groepslidmaatschap toe dat op douanebedrijfslogica voorbij wordt gebaseerd wat in de beweringen van SAML wordt verstrekt
+ Gebruikersprofielgegevens transformeren of verrijken voordat deze worden gesynchroniseerd met AEM
+ Complexe SAML-kenmerkstructuren toewijzen aan AEM-gebruikerseigenschappen
+ Aangepaste machtigingsregels of voorwaardelijke groepstoewijzingen implementeren
+ Aangepast registreren of controleren toevoegen tijdens SAML-verificatie
+ Integreren met externe systemen tijdens het verificatieproces

## `SamlHook` OSGi-service-interface

De interface `com.adobe.granite.auth.saml.spi.SamlHook` biedt twee haakmethoden die in verschillende stadia van het SAML-verificatieproces worden aangeroepen:

### `postSamlValidationProcess()` , methode

Deze methode wordt geroepen **nadat** de reactie van SAML is bevestigd maar **vóór** het proces van de gebruikerssynchronisatie begint. Dit is de ideale plaats om de beweringsgegevens van SAML, zoals het toevoegen van of het transformeren van attributen te wijzigen.

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### Gebruiksscenario&#39;s

+ Extra groepslidmaatschappen aan de bewering toevoegen
+ Kenmerkwaarden transformeren voordat deze worden gesynchroniseerd
+ Verrijk de bewering met gegevens uit externe bronnen
+ Aangepaste bedrijfsregels valideren

### `postSyncUserProcess()` , methode

Deze methode wordt geroepen **nadat** het proces van de gebruikerssynchronisatie is voltooid. Deze haak kan worden gebruikt om extra bewerkingen uit te voeren nadat de AEM-gebruiker is gemaakt of bijgewerkt.

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### Gebruiksscenario&#39;s

+ Extra eigenschappen van gebruikersprofielen bijwerken die niet door standaardsynchronisatie worden gedekt
+ Aangepaste gebruikersgerelateerde bronnen maken of bijwerken in AEM
+ Workflows of meldingen activeren na gebruikersverificatie
+ Aangepaste verificatiegebeurtenissen registreren

**Belangrijk:** om gebruikerseigenschappen in de bewaarplaats te wijzigen, vereist de hakenimplementatie:

+ Een `SlingRepository` referentie ingespoten via `@Reference`
+ Een gevormde [ de dienstgebruiker ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) met aangewezen toestemmingen (die in &quot;het Wijziging van de Dienst van het Mapper van de Dienst van Apache Sling wordt gevormd)
+ Correct sessiebeheer met blokken try-catch-finally

## Een aangepaste SAML-haak implementeren

De volgende stappen schetsen hoe te om een haak van douaneSAML tot stand te brengen en op te stellen.

### De SAML-haak-implementatie maken

Maak een nieuwe Java-klasse in het AEM-project die de interface `com.adobe.granite.auth.saml.spi.SamlHook` implementeert:

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### Vorm de haak SAML

De SAML haak gebruikt configuratie OSGi om te specificeren op welke IDP het zou moeten van toepassing zijn. Creeer een OSGi configuratiedossier in het project bij:

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

De `idpIdentifier` moet overeenkomen met de `idpIdentifier` -waarde die is geconfigureerd in de overeenkomstige OSGi-fabrieksconfiguratie (PID: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json` ) van de SAML-verificatiehandler. Deze overeenkomst is kritiek: de haak SAML zal slechts voor de instantie van de Handler van de Authentificatie van SAML worden aangehaald die de zelfde `idpIdentifier` waarde heeft. De SAML-verificatiehandler is een fabrieksconfiguratie. Dit houdt in dat u meerdere instanties kunt hebben (bijvoorbeeld `com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json` , `com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json` ) en dat elke haak via de `idpIdentifier` aan een specifieke handler is gekoppeld. De eigenschap `service.ranking` bepaalt de uitvoeringsvolgorde wanneer meerdere haken zijn geconfigureerd (hogere waarden worden het eerst uitgevoerd).

### Geweven afhankelijkheden toevoegen

Voeg de vereiste SAML SPI-afhankelijkheid toe aan het AEM Maven-kernproject `pom.xml` .

**voor de projecten van AEM as a Cloud Service**, gebruik het AEM SDK API gebiedsdeel dat de interfaces SAML omvat:

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

Het `aem-sdk-api` -artefact bevat alle benodigde Adobe Granite SAML-interfaces, inclusief `com.adobe.granite.auth.saml.spi.SamlHook` .

### Servicegebruiker configureren (optioneel)

Als de haak SAML inhoud in de bibliotheek van AEM JCR, zoals gebruikerseigenschappen moet wijzigen, (zoals aangetoond in het `postSyncUserProcess` voorbeeld), a [ de dienstgebruiker ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) moet worden gevormd:

1. Creeer een afbeelding van de de dienstgebruiker in het project bij `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json`:

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. Maak een herpointscript om de servicegebruiker en -machtigingen op `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json` te definiëren:

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

Dit verleent de toestemmingen van de de dienstgebruiker om gebruikerseigenschappen in de bewaarplaats te lezen en te wijzigen.

### Distribueren naar AEM

Implementeer de aangepaste SAML-haak naar AEM as a Cloud Service:

1. Het AEM-project bouwen
1. De code toewijzen aan de Cloud Manager Git-opslagplaats
1. Implementeren met behulp van een volledige stapeldistributiepijplijn
1. De SAML-haak wordt automatisch geactiveerd wanneer een gebruiker via SAML verifieert


### Belangrijke overwegingen

+ **Identifier die** zoekt: `idpIdentifier` in de haak SAML wordt gevormd moet `idpIdentifier` in de de fabrieksconfiguratie van de Handler van de Authentificatie van SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`) precies aanpassen
+ **de namen van Attributen**: Verzeker de attributennamen die in de haak (b.v., `groupMembership` worden van verwijzingen voorzien) passen de attributen aan die in de Handler van de Authentificatie SAML worden gevormd
+ **Prestaties**: Houd hakimplementaties lichtgewichtaangezien zij tijdens elke authentificatie SAML worden uitgevoerd
+ **de behandeling van de Fout**: De haakimplementaties van SAML zouden `com.adobe.granite.auth.saml.spi.SamlHookException` moeten werpen wanneer de kritieke fouten voorkomen die de authentificatie zouden moeten ontbreken. De SAML-verificatiehandler zal deze uitzonderingen afvangen en `AuthenticationInfo.FAIL_AUTH` retourneren. Voor repository-bewerkingen vangt u altijd de juiste fouten op `RepositoryException` en registreert u de fouten. Gebruik blokken met de &#39;try-catch-finally&#39; om te zorgen voor een correcte opschoning van bronnen
+ **het Testen**: De haken van de Test grondig in lagere milieu&#39;s alvorens aan productie op te stellen
+ **Veelvoudige haken**: De veelvoudige SAML hakenimplementaties kunnen worden gevormd; alle passende haken zullen worden uitgevoerd. Gebruik het `service.ranking` bezit in de component OSGi om de uitvoeringsorde (de hogere rangschikkingswaarden voeren eerst uit) te controleren. Om een haak SAML over veelvoudige de fabrieksconfiguraties van de Handler van de Authentificatie van SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`) opnieuw te gebruiken, creeer veelvoudige haken configuraties (OSGi fabrieksconfiguraties), elk met een verschillende `idpIdentifier` aanpassing van de respectieve Handler van de Authentificatie van SAML
+ **Veiligheid**: Valideer en ontsmet alle gegevens van de beweringen van SAML alvorens hen in bedrijfslogica te gebruiken
+ **Toegang van de Bewaarplaats**: Wanneer het wijzigen van gebruikerseigenschappen in `postSyncUserProcess`, gebruik altijd a [ de dienstgebruiker ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) met aangewezen toestemmingen eerder dan administratieve zittingen
+ **de gebruikerstoestemmingen van de Dienst**: Verleen minimale vereiste toestemmingen aan de [ de dienstgebruiker ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) (b.v., slechts `jcr:read` en `rep:write` op `/home/users`, niet volledige admin rechten)
+ **Sessiebeheer**: Gebruik altijd blokken try-catch-finally om te zorgen dat de sessies in de opslagplaats correct gesloten zijn, zelfs als er uitzonderingen optreden
+ **de synchronisatietiming van de Gebruiker**: De `postSyncUserProcess` haak voert uit nadat de gebruiker aan OAK is gesynchroniseerd, zodat is het gebruikersvoorwerp gewaarborgd om in de bewaarplaats op dat punt te bestaan
