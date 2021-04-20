---
title: AEM Forms met Marketo (Deel 2)
seo-title: AEM Forms met Marketo (Deel 2)
description: Zelfstudie over de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
seo-description: Zelfstudie over de integratie van AEM Forms met Marketo met behulp van het AEM Forms-formuliergegevensmodel.
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---


# Marketo-verificatieservice

De REST API&#39;s van Marketo worden geverifieerd met 2-podig OAuth 2.0. Wij moeten douaneauthentificatie tot stand brengen om tegen Marketo voor authentiek te verklaren. Deze aangepaste verificatie wordt doorgaans in een OSGI-bundel geschreven. De volgende code toont de aangepaste authenticator die is gebruikt als onderdeel van deze zelfstudie.

## Aangepaste verificatieservice

De volgende code leidt tot het voorwerp AuthenticationDetails dat access_token nodig voor authentificatie tegen Marketo heeft

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

MarketoAuthenticationService implementeert de IAuthentication-interface. Deze interface maakt deel uit van de AEM Forms Client SDK. De dienst krijgt het toegangstoken en neemt het teken in HttpHeader van AuthenticationDetails op. Zodra HttpHeaders van het voorwerp AuthenticationDetails wordt bevolkt wordt het voorwerp AuthenticationDetails teruggekeerd aan de laag van de Dermis van het Model van de Gegevens van de Vorm.

Let op de tekenreeks die door de methode getAuthenticationType wordt geretourneerd. Deze tekenreeks wordt gebruikt wanneer u de gegevensbron configureert.

### Toegangstoken ophalen

Een eenvoudige interface wordt bepaald met één methode die access_token terugkeert. De code voor de klasse die deze interface implementeert, wordt verderop op op de pagina weergegeven.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

De volgende code is van de dienst die access_token terugkeert die in het maken van REST API vraag moet worden gebruikt. De code in deze dienst heeft toegang tot de configuratieparameters nodig om de vraag van de GET te maken. Aangezien u kunt zien wij client_id, client_gehechtheid in GET URL overgaan om access_token te produceren. Dit access_token wordt vervolgens geretourneerd aan de aanroepende toepassing.

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

De scherm-schot hieronder toont de configuratieeigenschappen die moeten worden geplaatst. Deze configuratieeigenschappen worden gelezen in de code hierboven vermeld om access_token te krijgen

![config](assets/marketoconfig.jfif)

### Configuratie

De volgende code is gebruikt om de configuratie-eigenschappen te maken. Deze eigenschappen zijn specifiek voor uw Marketo-instantie

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

De volgende code leest de configuratie-eigenschappen en retourneert hetzelfde via de methoden getter

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. De bundel op uw AEM server bouwen en implementeren.
1. [Verwijs uw browser aan ](http://localhost:4502/system/console/configMgr) configMGrand onderzoek naar de Configuratie van de Dienst van de Credentials van de Marketo
1. Geef de juiste eigenschappen op die specifiek zijn voor uw Marketo-instantie
